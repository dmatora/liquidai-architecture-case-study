# XYLiquidity

API documentation for tradingstrategy.liquidity.XYLiquidity Python class in Trading Strategy framework.

*class* XYLiquidity[[source]](#)

Bases: `object`

Data structure that presents TVL/liquidity status in a DEX pool.

**Note**: The class name is misleading, as nowadays this class presents both XY Liquidity and TVL in CLMM DEX.

This data structure is for naive x*y=k AMM pool. Liquidity is not the part of the normal technical analysis, so the dataset server has separate datasets for it. Liquidity is expressed as US dollar value of the quote token of the pool. For example if the pool is 50 $FOO in reserve0 and 50 $USDC in reserve1, the liquidity of the pool would be expressed as 50 USD. Liquidity events, like candles, have open, high, low and close values, depending on which time of the candle they were sampled.

## __init__(*pair_id*, *timestamp*, *exchange_rate*, *open*, *close*, *high*, *low*, *adds*, *removes*, *syncs*, *add_volume*, *start_block*, *end_block*)

### Parameters:
- **pair_id** (*int*)
- **timestamp** (*int*)
- **exchange_rate** (*float*)
- **open** (*float*)
- **close** (*float*)
- **high** (*float*)
- **low** (*float*)
- **adds** (*int*)
- **removes** (*int*)
- **syncs** (*int*)
- **add_volume** (*float*)
- **start_block** (*int*)
- **end_block** (*int*)

**Return type**: None

## Methods

### `__init__`(pair_id, timestamp, exchange_rate, ...)
### `convert_web_candles_to_dataframe`(web_candles)
Return Pandas dataframe presenting TVL data fetched from JSON endpoint.

### `from_dict`(kvs, *[, infer_missing])
### `from_json`(s, *[, parse_float, parse_int, ...])
### `schema`(*[, infer_missing, only, exclude, ...])
### `to_dataframe`()
Return empty Pandas dataframe presenting liquidity sample.

### `to_dict`([encode_json])
### `to_json`(*[, skipkeys, ensure_ascii, ...])
### `to_pyarrow_schema`([small_candles])
Construct schema for writing Parquet files for these candles.

## Attributes

### pair_id: *int*
Primary key to identity the trading pair. Use pair universe to map this to chain id and a smart contract address

### timestamp: *int*
Open timestamp for this time bucket.

### exchange_rate: *float*
USD exchange rate of the quote token used to convert to dollar amounts in this time bucket. Note that currently any USD stablecoin (USDC, DAI) is assumed to be 1:1 and the candle server cannot handle exchange rate difference among stablecoins. The rate is taken at the beginning of the 1 minute time bucket. For other time buckets, the exchange rate is the simple average for the duration of the bucket.

### open: *float*
Liquidity absolute values in the pool in different time points. Note - for minute candles - if the candle contains only one event (mint, burn, sync) the open liquidity value is the value AFTER this event. The dataset server does not track the closing value of the previous liquidity event. This applies for minute candles only.

### close: *float*
Liquidity absolute values in the pool in different time points

### high: *float*
Liquidity absolute values in the pool in different time points

### low: *float*
Liquidity absolute values in the pool in different time points

### adds: *int*
Number of liquidity supplied events for pool

### removes: *int*
Number of liquidity removed events for the pool

### syncs: *int*
Number of total events affecting liquidity during the time window. This is adds, removes AND swaps AND sync().

### add_volume: *float*
How much new liquidity was removed, in the terms of the quote token converted to US dollar

### start_block: *int*
Blockchain tracking information

### end_block: *int*
Blockchain tracking information

## Class Methods

### *classmethod* to_pyarrow_schema(*small_candles=False*)[[source]](#)
Construct schema for writing Parquet files for these candles.

**Parameters**: **small_candles** – Use even smaller word sizes for frequent (1m) candles.

**Return type**: *Schema*

### *classmethod* to_dataframe()[[source]](#)
Return empty Pandas dataframe presenting liquidity sample.

**Return type**: *DataFrame*

### *classmethod* convert_web_candles_to_dataframe(*web_candles*)[[source]](#)
Return Pandas dataframe presenting TVL data fetched from JSON endpoint. Convert JSON data to Pandas. Uses /candles endpoint data format https://tradingstrategy.ai/api/explorer/#/Trading%20pair/web_candles

**Parameters**: **web_candles** (*list[dict]*)

**Return type**: *DataFrame*