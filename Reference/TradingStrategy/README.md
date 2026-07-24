# Trading Strategy Reference

This folder contains reference documentation for classes and more importantly **data structures** in the **TradingStrategy** framework. These references describe how on-chain DEX data—such as candles, liquidity, pairs, and exchanges—is modeled and accessed in our system.

---

## Contents

- [**Candle.md**](Candle.md)  
  Data structure describing an OHLCV trading candle, plus DEX-specific fields.  
  Explains how candle data is stored and handled (open/high/low/close, volume, average price, block ranges, etc.).

- [**DEXPair.md**](DEXPair.md)  
  Defines the representation of a trading pair (e.g., WETH-USDC), including token addresses, decimals, DEX exchange info, and pair-level stats.

- [**Exchange.md**](Exchange.md)  
  Describes DEX exchange metadata (e.g., Uniswap V2 on Ethereum mainnet), including exchange slug, addresses, trade stats, and supported pairs.

- [**XYLiquidity.md**](XYLiquidity.md)  
  Data structure for tracking liquidity or TVL in a naive x*y=k AMM (or partially with CLMM logic). Shows how we store pool TVL snapshots and relevant stats like adds/removes of liquidity.

---

Back to [Reference Overview](../README.md)  
Go to [Documentation Overview](../../README.md)  
