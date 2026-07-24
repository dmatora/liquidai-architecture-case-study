# Candle

API documentation for tradingstrategy.candle.Candle Python class in Trading Strategy framework.

## class Candle[source]

Bases: `object`

Data structure presenting one OHLCV trading candle. Based on the open-high-low-close-volume concept. Trading Strategy candles come with additional information available on the top of core OHLCV, as chain analysis has deeper visibility than one would get on traditional exchanges. For example for enhanced attributes see `Candle.buys` (buy count) or `Candle.start_block` (blockchain starting block number of the candle). We also separate "buys" and "sells". Although this separation might not be meaningful on order-book based exchanges, we define "buy" as a DEX swap where quote token (USD, ETH) was swapped into more exotic token (AAVE, SUSHI, etc.)

### __init__(pair_id, timestamp, exchange_rate, open, close, high, low, buys, sells, volume, buy_volume, sell_volume, avg, start_block, end_block)

Parameters:
- **pair_id** (*int*)
- **timestamp** (*int*)
- **exchange_rate** (*float*)
- **open** (*float*)
- **close** (*float*) 
- **high** (*float*)
- **low** (*float*)
- **buys** (*int | None*)
- **sells** (*int | None*)
- **volume** (*float*)
- **buy_volume** (*float | None*)
- **sell_volume** (*float | None*)
- **avg** (*float*)
- **start_block** (*int*)
- **end_block** (*int*)

Return type: None

### Methods

- `__init__(pair_id, timestamp, exchange_rate, ...)`
- `from_dict(kvs, *[, infer_missing])`
- `from_json(s, *[, parse_float, parse_int, ...])`
- `generate_synthetic_sample(pair_id, ...[, volume])` - Generate a candle dataframe.
- `schema(*[, infer_missing, only, exclude, ...])`
- `to_dataframe()` - Return empty Pandas dataframe presenting candle data.
- `to_dict([encode_json])`
- `to_json(*[, skipkeys, ensure_ascii, ...])`
- `to_pyarrow_schema([small_candles])` - Construct schema for writing Parquet files for these candles.
- `to_qstrader_dataframe()` - Return empty Pandas dataframe presenting candle data for QStrader.

### Attributes

#### DATAFRAME_FIELDS
Schema definition for :py:class:`pd.DataFrame`

#### trades
Amount of all trades during the candle period.

#### pair_id: int
Primary key to identity the trading pair. Use pair universe to map this to chain id and a smart contract address

#### timestamp: int
Open timestamp for this candle.

#### exchange_rate: float
USD exchange rate of the quote token used to convert to dollar amounts in this candle.

#### open: float
OHLC core data

#### close: float
OHLC core data

#### high: float
OHLC core data

#### low: float
OHLC core data

#### buys: int | None
Number of buys happened during the candle period. Only available on DEXes where buys and sells can be separated.

#### sells: int | None
Number of sells happened during the candle period. Only available on DEXes where buys and sells can be separated.

#### volume: float
Trade volume

#### buy_volume: float | None
Buy side volume. Swap quote token -> base token volume

#### sell_volume: float | None
Sell side volume. Swap base token -> quote token volume

#### avg: float
Average trade size

#### start_block: int
The first blockchain block that includes trades that went into this candle.

#### end_block: int
The last blockchain block that includes trades that went into this candle.

#### DATAFRAME_FIELDS
```python
{
    'avg': 'float',
    'buy_volume': 'float',
    'buys': 'float',
    'close': 'float',
    'end_block': 'int',
    'exchange_rate': 'float',
    'high': 'float',
    'low': 'float',
    'open': 'float',
    'pair_id': 'int',
    'sell_volume': 'float',
    'sells': 'float',
    'start_block': 'int',
    'timestamp': 'datetime64[s]',
    'volume': 'float'
}
```
Schema definition for :py:class:`pd.DataFrame`. Defines Pandas datatypes for columns in our candle data format. Useful e.g. when we are manipulating JSON/hand-written data.

### Class Methods

#### classmethod to_dataframe()[source]
Return empty Pandas dataframe presenting candle data.

Return type: *DataFrame*

#### classmethod to_qstrader_dataframe()[source]
Return empty Pandas dataframe presenting candle data for QStrader.
TODO: Fix QSTrader to use "standard" column names.

Return type: *DataFrame*

#### classmethod to_pyarrow_schema(small_candles=False)[source]
Construct schema for writing Parquet files for these candles.

Parameters: **small_candles** – Use even smaller word sizes for frequent (1m) candles.

Return type: *Schema*

#### static generate_synthetic_sample(pair_id, timestamp, price, volume=None)[source]
Generate a candle dataframe. Used in testing when manually fiddled data is needed. All open/close/high/low set to the same price. Exchange rate is 1.0. Other data set to zero.

Parameters:
- **pair_id** (*int*)
- **timestamp** (*Timestamp*)
- **price** (*float*)
- **volume** (*float | None*)

Returns: One dict of filled candle data
Return type: dict