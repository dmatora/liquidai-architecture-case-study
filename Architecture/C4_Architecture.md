# Architecture: C4 Model Overview

This document provides a high-level architectural overview of the LiquidAI Trading & Backtesting Engine using the **C4 Model** methodology. 

It maps the system from the highest level of abstraction (Context) down to internal components and data structures, serving as an entry point to the detailed technical documentation.

---

## Level 1: System Context (C1)

The System Context diagram shows the LiquidAI platform as a whole and its interactions with users and external systems.

```mermaid
flowchart TD
    classDef person fill:#08427b,color:#fff,stroke:#073b6f,stroke-width:2px;
    classDef system fill:#1168bd,color:#fff,stroke:#0b4884,stroke-width:2px;
    classDef external fill:#888888,color:#fff,stroke:#555555,stroke-width:2px;

    user["👤 <b>Trader / User</b><br/>Explores trading pairs<br/>Manages bot lifecycles (creates/deletes)<br/>Configures strategies & analyzes PnL"]:::person

    liquidai["<b>LiquidAI Engine</b><br/>DEX trading & backtesting platform<br/>built on top of Hummingbot Framework<br/>Supports AMM strategies"]:::system

    ts_ext["<b>TradingStrategy.ai</b><br/>Historical DEX candles,<br/>pairs & liquidity data"]:::external
    owlracle_ext["<b>Owlracle API</b><br/>Gas price history<br/>& fee estimations"]:::external
    chain_ext["<b>Blockchain Nodes</b><br/>Ethereum / Polygon / BSC<br/>RPC endpoints"]:::external
    oracle_ext["<b>Rate Oracle (Binance)</b><br/>Token-to-USD conversion rates"]:::external

    user -->|Web UI / Dashboard| liquidai
    liquidai -->|HTTPS / REST API| ts_ext
    liquidai -->|HTTPS / REST API| owlracle_ext
    liquidai -->|JSON-RPC / Web3| chain_ext
    liquidai -->|Python API / HTTPS| oracle_ext
```

---

## Level 2: Container Diagram (C2)

Zooming into the `LiquidAI Engine`, the Container diagram shows the executables, data storage technologies, and how responsibilities are distributed across the system.

```mermaid
flowchart TD
    classDef person fill:#08427b,color:#fff,stroke:#073b6f,stroke-width:2px;
    classDef container fill:#1168bd,color:#fff,stroke:#0b4884,stroke-width:2px;
    classDef db fill:#1168bd,color:#fff,stroke:#0b4884,stroke-width:2px;
    classDef external fill:#888888,color:#fff,stroke:#555555,stroke-width:2px;

    user["👤 <b>Trader / User</b><br/>Explores trading pairs<br/>Manages bot lifecycles (creates/deletes)<br/>Configures strategies & analyzes PnL"]:::person

    subgraph LiquidAI ["<b>LiquidAI Platform</b>"]
        web_ui["<b>Web UI Dashboard</b><br/><i>[React / Next.js]</i><br/>UI for bot management,<br/>strategy config & reporting"]:::container
        
        subgraph Core ["<b>Execution Core</b>"]
            hummingbot["<b>Hummingbot Core</b><br/><i>[Python / Cython]</i><br/>Market connectors, clock loop<br/>& event handling"]:::container
            liquidai_mod["<b>LiquidAI Modules</b><br/><i>[Python / Pandas]</i><br/>Backtesting engine, AMM simulator<br/>& data loaders"]:::container
        end

        parquet[("<b>Market Data</b><br/><i>[Parquet Files]</i><br/>Historical candles & TVL")]:::db
        json_db[("<b>Gas Data</b><br/><i>[JSON Files]</i><br/>Daily gas price OHLC")]:::db
    end

    ts_ext["<b>TradingStrategy.ai</b><br/>Historical DEX datasets"]:::external
    owlracle_ext["<b>Owlracle API</b><br/>Gas price history API"]:::external
    chain_ext["<b>Blockchain Nodes</b><br/>Ethereum / Polygon / BSC"]:::external
    oracle_ext["<b>Rate Oracle</b><br/>Token-to-USD rates"]:::external

    user -->|HTTPS| web_ui
    web_ui -->|REST API / WebSockets| hummingbot
    hummingbot -->|Python API| liquidai_mod

    liquidai_mod -->|PyArrow / Disk I/O| parquet
    liquidai_mod -->|Local Disk I/O| json_db

    liquidai_mod -->|HTTPS / REST API| ts_ext
    liquidai_mod -->|HTTPS / REST API| owlracle_ext
    hummingbot -->|JSON-RPC / Web3| chain_ext
    liquidai_mod -->|Python API| oracle_ext
```

---

## Level 3: Component Diagram (C3)

Zooming into the `LiquidAI Modules` container, we expose the isolated domain components that demonstrate our **Separation of Concerns**:

```mermaid
graph TD
    subgraph "Simulation & Processing Engine"
        A[AMMMeanReversionBacktest Orchestrator] --> B(TradingStrategyDataManager)
        A --> C(GasDataManager)
        A --> D(AMMLiquidityCalculator)
        A --> E(AMMPriceSimulator)
    end
    
    style A fill:#cfc,stroke:#333,stroke-width:2px
    style B fill:#ccf,stroke:#333,stroke-width:1px
    style C fill:#ccf,stroke:#333,stroke-width:1px
    style D fill:#ccf,stroke:#333,stroke-width:1px
    style E fill:#ccf,stroke:#333,stroke-width:1px
```

> **Detailed component specifications:**
> * [**AMMMeanReversionBacktest**](../Components/AMMMeanReversionBacktest.md) - Core backtesting engine orchestrating historical simulations.
> * [**TradingStrategyDataManager**](../Components/TradingStrategyDataManager.md) - Manages PyArrow/Parquet data loading and caching.
> * [**GasDataManager**](../Components/GasDataManager.md) - Handles gas fee calculations and Owlracle data processing.
> * [**AMMPriceSimulator**](../Components/AMMPriceSimulator.md) - Simulates x*y=k constant product price impact and slippage.
> * [**AMMLiquidityCalculator**](../Components/AMMLiquidityCalculator.md) - Converts and normalizes pool reserves and TVL.

---

## Level 4: Code & Data Schemas (C4)

Key Data Models and Schemas utilized across the system:

* **`PoolState`**: A lightweight container for pool reserves. [See details](../Components/AMMLiquidityCalculator.md#poolstate-data-model).
* **`Candle`**: OHLCV schema definition. [See reference](../Reference/TradingStrategy/Candle.md).
* **`DEXPair`**: Smart contract address and metadata mapping. [See reference](../Reference/TradingStrategy/DEXPair.md).
* **`XYLiquidity`**: TVL tracking structure. [See reference](../Reference/TradingStrategy/XYLiquidity.md).