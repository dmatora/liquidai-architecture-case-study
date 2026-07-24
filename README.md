# LiquidAI: High-Performance DEX Trading & Backtesting Engine

> **Portfolio Architecture Case Study**  
> This repository contains the complete technical architecture and system specifications designed for **LiquidAI** (an inactive Web3/Fintech project). The specifications here served as the core foundation for the official client documentation.
>
> 🌐 **Live Public Documentation:** [LiquidAI GitBook Portal](https://liquidai.gitbook.io/docs/overview/introduction)

---

## 🏗 System Architecture & C4 Model

For a top-down architectural view of the platform (System Context, Containers, Components, and Data Models), refer to our C4 documentation:

👉 [**C4 Architecture Overview Document**](Architecture/C4_Architecture.md)

---

## 📚 Technical Documentation Matrix

Below is the complete mapping of local architectural specifications and their corresponding production-rendered documentation on GitBook:

| System Module / Document | Local Technical Spec (GitHub) | Live Rendered Docs & Diagrams (GitBook)                                                                                 |
| :--- | :--- |:------------------------------------------------------------------------------------------------------------------------|
| **System C4 Model Overview** | [`C4_Architecture.md`](Architecture/C4_Architecture.md) | - |
| **Data Performance Evolution (V1→V4)** | [`DataPerformance.md`](Architecture/DataPerformance.md) | [Live Workflow & Diagrams 🔗](https://liquidai.gitbook.io/docs/technology-deep-dive/high-performance-data-architecture) |
| **Backtest Engine & Workflow** | [`AMMMeanReversionBacktest.md`](Components/AMMMeanReversionBacktest.md) | [Live Workflow & Diagrams 🔗](https://liquidai.gitbook.io/docs/technology-deep-dive/amm-mean-reversion-backtest)        |
| **AMM Price Simulator** | [`AMMPriceSimulator.md`](Components/AMMPriceSimulator.md) | [Live Workflow & Diagrams 🔗](https://liquidai.gitbook.io/docs/technology-deep-dive/amm-price-simulator)                |
| **Liquidity Calculator** | [`AMMLiquidityCalculator.md`](Components/AMMLiquidityCalculator.md) | [Live Workflow & Diagrams 🔗](https://liquidai.gitbook.io/docs/technology-deep-dive/amm-liquidity-calculator)           |
| **Gas Data Manager** | [`GasDataManager.md`](Components/GasDataManager.md) | [Live Workflow & Diagrams 🔗](https://liquidai.gitbook.io/docs/technology-deep-dive/gas-data-manager)                   |
| **Trading Strategy Data Manager** | [`TradingStrategyDataManager.md`](Components/TradingStrategyDataManager.md) | [Live Workflow & Diagrams 🔗](https://liquidai.gitbook.io/docs/technology-deep-dive/trading-strategy-data-manager)      |

---

## 🔮 Roadmap & Technical References

* 🚀 [**Future Directions & Roadmap**](FutureDirections.md) — Prioritized technical backlog and architectural roadmap (Immediate, Mid-Term, and Long-Term goals).
* 📖 [**Reference Specs & Schemas**](Reference/README.md) — External API specifications ([Owlracle Gas API](Reference/Owlracle.md)) and core framework data models ([Candle](Reference/TradingStrategy/Candle.md), [DEXPair](Reference/TradingStrategy/DEXPair.md), [Exchange](Reference/TradingStrategy/Exchange.md), [XYLiquidity](Reference/TradingStrategy/XYLiquidity.md)).

---

## 💡 Key Architectural Highlights

1. **Offline-First High-Performance Data Architecture:**
    * Transitioned from direct rate-limited API calls (hours per backtest) to columnar Parquet file ingestion and in-memory caches, reducing execution times to **seconds**.
2. **Strict Separation of Concerns (SoC):**
    * Decoupled DEX pool math (`AMMLiquidityCalculator`), constant product price simulation (`AMMPriceSimulator`), and EVM gas calculations (`GasDataManager`) from strategy decision logic.
3. **AppSec & Financial Precision:**
    * Handled decimal precision issues in Python, simulated AMM price impact ($x \cdot y = k$) to prevent slippage losses, and integrated multi-chain gas oracles.

---

## 🛠 Tech Stack

* **Language & Frameworks:** Python 3.10+, Cython, Hummingbot Framework, Pandas, PyArrow
* **Data Storage & Formats:** Apache Parquet, JSON, In-Memory Caches
* **Modelling & Documentation:** C4 Model, Mermaid (Diagrams as Code), Markdown
* **EVM Integration & Oracles:** Web3, Owlracle API, TradingStrategy.ai