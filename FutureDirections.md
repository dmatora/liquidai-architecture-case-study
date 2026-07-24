## **Immediate Priorities (High Impact, High Urgency)**

1. **Implement Historical Data in Live Strategy**  
   - **Why**: Avoid long “warm-up” wait times (like 7 or 30 days) by loading relevant historical data at startup.  
   - **Impact**: The bot starts trading with a fully populated rolling window (e.g., for mean-reversion calculations), drastically improving user experience and immediate utility.

2. **Price Impact in Trading Decisions** (from [*AMMMeanReversionBacktest.md*](Components/AMMMeanReversionBacktest.md))  
   - **Why**: Prevent large trades from skewing P&L by factoring in AMM liquidity constraints before placing orders.  
   - **Impact**: Ensures realistic trade signals and reduces potential losses due to slippage.

3. **Real-time Trade Sizing** (from [*AMMPriceSimulator.md*](Components/AMMPriceSimulator.md))  
   - **Why**: Dynamically scaling trades (buy/sell amounts) to maintain slippage below a threshold.  
   - **Impact**: Produces more accurate performance outcomes and less wasted capital.

4. **More Granular Intervals for Gas** (from [*GasDataManager.md*](Components/GasDataManager.md))  
   - **Why**: Daily gas data can miss intraday spikes, causing inaccurate or delayed cost estimates for trades.  
   - **Impact**: Improves simulation and live execution reliability.

5. **V3: Incremental Updates for Live Data** (from [*Architecture/DataPerformance.md*](Architecture/DataPerformance.md))  
   - **Why**: Keep the strategy’s data feed near-real-time; avoid stale market conditions.  
   - **Impact**: Critical for responding to fast-moving markets and accurate daily calculations.

---

## **Mid-Term Priorities (Important, but Less Urgent)**

1. **Consolidated Data Retrieval** (from [*TradingStrategyDataManager.md*](Components/TradingStrategyDataManager.md)  
   - **Why**: A single, high-level API to fetch candles, liquidity, historical gas, etc.  
   - **Impact**: Simplifies strategy code and maintenance overhead.

2. **Parquet Migration for Gas Data** (from [*GasDataManager.md*](Components/GasDataManager.md)  
   - **Why**: As gas intervals become finer, JSON will bloat. Columnar storage (Parquet) is more efficient.  
   - **Impact**: Improves performance and scalability when dealing with large data sets.

3. **Data Collection & Accumulation Scripts** (from [*GasDataManager.md*](Components/GasDataManager.md)  
   - **Why**: Regularly fetch intraday data to maintain a rolling, up-to-date repository for gas and possibly other metrics.  
   - **Impact**: Automates “fresh data” ingestion without manual effort.

4. **Dashboard for Pair Exploration** (from [*AMMMeanReversionBacktestConfig.md*](Components/AMMMeanReversionBacktestConfig.md)  
   - **Why**: Help less-technical users see which pairs are available and viable (liquidity, trading volume, etc.).  
   - **Impact**: Reduces trial-and-error and “pair not found” errors.

5. **Drop/Implement `gas_fee_buffer`** (from [*AMMMeanReversionBacktestConfig.md*](Components/AMMMeanReversionBacktestConfig.md)  
   - **Why**: Decide whether to fully support a buffer parameter or remove it to avoid confusion.  
   - **Impact**: Clearer, less error-prone logic for gas expense modeling.

6. **Remove Reliance on Extra Hummingbot Classes** (from [*AMMMeanReversionBacktestConfig.md*](Components/AMMMeanReversionBacktestConfig.md)  
   - **Why**: Overly broad configs (e.g. `markets`, `candles_config`, `controllers_config`) may not be necessary for minimal setups.  
   - **Impact**: Streamlines code and helps make the strategy more portable.

---

## **Long-Term Priorities (Complex or Forward-Looking)**

1. **V4: Advanced Analytics & ML** (from [*Architecture/DataPerformance.md*](Architecture/DataPerformance.md)  
   - **Why**: Unlock automated parameter optimization, scenario-based tests, and adaptive strategies.  
   - **Impact**: Increases competitiveness and appeals to advanced users seeking data-driven decisions.

2. **Dynamic Fee Structures** (from [*AMMPriceSimulator.md*](Components/AMMPriceSimulator.md)  
   - **Why**: Many AMMs (Uniswap V3, Curve) offer variable fees. Static assumptions reduce realism.  
   - **Impact**: More accurate for smaller or specialized pools with non-standard fee tiers.

3. **Concentrated Liquidity** (from [*AMMPriceSimulator.md*](Components/AMMPriceSimulator.md)  
   - **Why**: Uniswap V3’s liquidity ranges change how reserves shift for large swaps.  
   - **Impact**: Major step up in complexity but critical for big, modern pools.

4. **Selective Adoption of V4 Enhancements** (from [*AMMPriceSimulator.md*](Components/AMMPriceSimulator.md)  
   - **Why**: Potential edge in early adoption but uncertain maturity.  
   - **Impact**: Gains advanced features at the risk of tracking less-stable protocols.

5. **Automated Data Validation** (from [*TradingStrategyDataManager.md*](Components/TradingStrategyDataManager.md)  
   - **Why**: Multiple data sources can introduce errors. Automated checks ensure data integrity.  
   - **Impact**: Reduces risk of corrupted backtests or misleading signals.

6. **Performance Profiling** (from [*TradingStrategyDataManager.md*](Components/TradingStrategyDataManager.md)  
   - **Why**: Larger data sets and advanced analytics can slow processing.  
   - **Impact**: Helps identify and eliminate bottlenecks for stable, large-scale usage.

7. **(Optional) “Wait 30 days” or “Wait 7 days”** (from [*AMMMeanReversion.md*](Components/AMMMeanReversion.md))  
   - **Why**: Some strategies require a 30-day window, but if we’re now loading historical data, it’s less of a concern.  
   - **Impact**: This approach effectively becomes moot once offline historical loading is in place.
