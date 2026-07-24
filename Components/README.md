## Strategy Components: Table of Contents

This page provides an overview of the main components in our AMM mean-reversion strategy. For each component, you’ll find links to dedicated documentation detailing how it works and how it integrates with the rest of the system.

### Configuration Components
Strategy configuration classes and utilities:

- [**AMMMeanReversionBacktestConfig**](AMMMeanReversionBacktestConfig.md) - Configuration parameters for the backtesting component

### AMM Mean Reversion Strategy Components
These components work together to implement DEX strategies based on mean reversion:

- [**AMMMeanReversionBacktest**](AMMMeanReversionBacktest.md) - Core backtesting engine that simulates strategy performance with historical data
- [**AMMPriceSimulator**](AMMPriceSimulator.md) - Simulates trade executions using constant product formula to calculate price impact and slippage 
- [**AMMLiquidityCalculator**](AMMLiquidityCalculator.md) - Converts between different pool reserve representations and handles TVL calculations

### Data Management Components
Components that handle loading and processing historical data:

- [**GasDataManager**](GasDataManager.md) - Loads and processes historical gas price data to estimate transaction costs
- [**TradingStrategyDataManager**](TradingStrategyDataManager.md) - Manages access to historical DEX trading data from TradingStrategy.ai

---

Back to [Documentation Overview](../README.md)  
