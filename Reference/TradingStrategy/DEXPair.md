# DEXPair

API documentation for `tradingstrategy.pair.DEXPair` Python class in Trading Strategy framework.

## class DEXPair[source]

Bases: `object`

Trading pair information for a single pair. Presents a single trading pair on decentralised exchanges. DEX trading pairs can be uniquely identified by:

- Internal id
- (Chain id, address) tuple - the same address can exist on multiple chains
- (Chain slug, exchange slug, pair slug) tuple

Token names and symbols are *not* unique - anyone can create any number of trading pair tickers and token symbols. Do not rely on token symbols for anything.

### About data:

There is a different between token0 and token1 and base_token and quote_token conventions - the former are raw DEX (Uniswap) data while the latter are preprocessed by the server to make the data more understandable. Base token is the token you are trading and the quote token is the token you consider "money" for the trading. E.g. in WETH-USDC, USDC is the quote token. In SUSHI-WETH, WETH is the quote token.

Optional fields may be available if the candle server:
1. detected the pair popular enough
2. managed to fetch the third party service information related to the token

When you download a trading pair dataset from the server, not all trading pairs are available. For more information about trading pair availability see trading pair tracking.

The class provides some JSON helpers to make it more usable with JSON based APIs. This data class is serializable via dataclasses-json methods.

Example:
```python
info_as_string = pair.to_json()
```

You can also do `__json__()` convention data export:
```python
info_as_dict = pair.__json__()
```

### Note

Currently all flags are disabled and will be removed in the future. The historical dataset does not contain any filtering flags, because the data has to be filtered prior to download, to keep the download dump in a reasonasble size. The current data set of 800k trading pairs produce 100 MB dataset of which most of the pairs are useless. The server prefilters trading pairs and thus you cannot access historical data of pairs that have been prefiltered.

### __init__(pair_id, chain_id, exchange_id, address, token0_address, token1_address, token0_symbol, token1_symbol, dex_type=None, base_token_symbol=None, quote_token_symbol=None, token0_decimals=None, token1_decimals=None, exchange_slug=None, exchange_address=None, pair_slug=None, first_swap_at_block_number=None, last_swap_at_block_number=None, first_swap_at=None, last_swap_at=None, flag_inactive=None, flag_blacklisted_manually=None, flag_unsupported_quote_token=None, flag_unknown_exchange=None, fee=None, buy_count_all_time=None, sell_count_all_time=None, buy_volume_all_time=None, sell_volume_all_time=None, buy_count_30d=None, sell_count_30d=None, buy_volume_30d=None, sell_volume_30d=None, buy_tax=None, transfer_tax=None, sell_tax=None, exchange_name=None)

Parameters:
- **pair_id** (*int*) 
- **chain_id** (*ChainId*)
- **exchange_id** (*int*)
- **address** (*str*)
- **token0_address** (*str*)
- **token1_address** (*str*)
- **token0_symbol** (*Optional[str]*)
- **token1_symbol** (*Optional[str]*)
- **dex_type** (*Optional[ExchangeType]*)
- **base_token_symbol** (*Optional[str]*)
- **quote_token_symbol** (*Optional[str]*)
- **token0_decimals** (*Optional[int]*)
- **token1_decimals** (*Optional[int]*)
- **exchange_slug** (*Optional[str]*)
- **exchange_address** (*Optional[str]*)
- **pair_slug** (*Optional[str]*)
- **first_swap_at_block_number** (*Optional[int]*)
- **last_swap_at_block_number** (*Optional[int]*)
- **first_swap_at** (*Optional[int]*)
- **last_swap_at** (*Optional[int]*)
- **flag_inactive** (*Optional[bool]*)
- **flag_blacklisted_manually** (*Optional[bool]*)
- **flag_unsupported_quote_token** (*Optional[bool]*)
- **flag_unknown_exchange** (*Optional[bool]*)
- **fee** (*Optional[int]*)
- **buy_count_all_time** (*Optional[int]*)
- **sell_count_all_time** (*Optional[int]*)
- **buy_volume_all_time** (*Optional[float]*)
- **sell_volume_all_time** (*Optional[float]*)
- **buy_count_30d** (*Optional[int]*)
- **sell_count_30d** (*Optional[int]*)
- **buy_volume_30d** (*Optional[float]*)
- **sell_volume_30d** (*Optional[float]*)
- **buy_tax** (*Optional[float]*)
- **transfer_tax** (*Optional[float]*)
- **sell_tax** (*Optional[float]*)
- **exchange_name** (*Optional[str]*)

Return type: None

## Methods

### `__init__`(pair_id, chain_id, exchange_id, ...)

### `convert_to_dataframe`(pairs)
Convert Python DEXPair objects back to the Pandas dataframe presentation.

### `convert_to_pyarrow_table`(pairs[, check_schema])
Convert a list of DEXPair instances to a Pyarrow table.

### `create_from_row`(row)
Convert a DataFrame for to a DEXPair instance.

### `from_dict`(kvs, *[, infer_missing])

### `from_json`(s, *[, parse_float, parse_int, ...])

### `from_series`(pair_id, series)
Create a DEXPair instance from Pandas Series.

### `get_base_token`()
Return token class presentation of base token in this trading pair.

### `get_friendly_name`(exchange_universe)
Get a very human readable name for this trading pair.

### `get_link`()
Get the trading pair link page on TradingStrategy.ai

### `get_quote_token`()
Return token class presentation of quote token in this trading pair.

### `get_ticker`()
Return trading 'ticker'

### `get_trading_pair_page_url`()
Get information page for this trading pair.

### `is_tradeable`([liquidity_threshold, ...])
Can this pair be traded.

### `schema`(*[, infer_missing, only, exclude, ...])

### `to_dict`([encode_json])

### `to_human_description`()
Get human description for this pair.

### `to_json`(*[, skipkeys, ensure_ascii, ...])

### `to_pyarrow_schema`()
Construct schema for reading writing Parquet filss for pair information.

## Attributes

### `pair_id`
Internal primary key for any trading pair

### `chain_id`
The chain id on which chain this pair is trading.

### `exchange_id`
The exchange where this token trades

### `address`
Smart contract address for the pair.

### `token0_address`
Token pair contract address on-chain.

### `token1_address`
Token pair contract address on-chain Lowercase, non-checksummed.

### `token0_symbol`
Token0 as in raw Uniswap data.

### `token1_symbol`
Token1 as in raw Uniswap data ERC-20 contracst are not guaranteed to have this data.

### `dex_type`
What kind of exchange this pair is on

### `base_token_symbol`
Naturalised base and quote token.

### `quote_token_symbol`
Naturalised base and quote token.

### `token0_decimals`
Number of decimals to convert between human amount and Ethereum fixed int raw amount.

### `token1_decimals`
Number of decimals to convert between human amount and Ethereum fixed int raw amount Note - this information might be missing from ERC-20 smart contracts.

### `exchange_slug`
Denormalised web page and API look up information

### `exchange_address`
Exchange factory address.

### `pair_slug`
Denormalised web page and API look up information

### `first_swap_at_block_number`
Block number of the first Uniswap Swap event

### `last_swap_at_block_number`
Block number of the last Uniswap Swap event

### `first_swap_at`
Timestamp of the first Uniswap Swap event

### `last_swap_at`
Timestamp of the first Uniswap Swap event

### `flag_inactive`
Pair has been flagged inactive, because it has not traded at least once during the last 30 days.

### `flag_blacklisted_manually`
Pair is blacklisted by operators.

### `flag_unsupported_quote_token`
Quote token is one of USD, ETH, BTC, MATIC or similar popular token variants.

### `flag_unknown_exchange`
Pair is listed on an exchange we do not if it is good or not TODO - inactive, remove.

### `fee`
Swap fee in basis points if known

### `buy_count_all_time`
Risk assessment summary data

### `sell_count_all_time`
Risk assessment summary data

### `buy_volume_all_time`
Risk assessment summary data

### `sell_volume_all_time`
Risk assessment summary data

### `buy_count_30d`
Risk assessment summary data

### `sell_count_30d`
Risk assessment summary data

### `buy_volume_30d`
Risk assessment summary data

### `sell_volume_30d`
Risk assessment summary data

### `buy_tax`
Buy token tax for this trading pair.

### `transfer_tax`
Transfer token tax for this trading pair.

### `sell_tax`
Sell tax for this trading pair.

### `exchange_name`
Exchange name.

### `base_token_address`
Get smart contract address for the base token.

### `base_token_decimals`
Get token decimal count for the base token.

### `fee_tier`
Return the trading pair fee as 0...1.

### `quote_token_address`
Get smart contract address for the quote token

### `quote_token_decimals`
Get token decimal count for the quote token

### `volume_30d`
Denormalise trading volume last 30 days.