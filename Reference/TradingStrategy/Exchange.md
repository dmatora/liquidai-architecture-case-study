# Exchange

*class* Exchange[source]

Bases: `object`

A decentralised exchange. Each chain can have multiple active or abandoned decentralised exchanges of different types, like AMM based or order book based. The dataset server automatically discovers exchanges and tries to add meaningful label and risk data for them.

Most of the fields are optional and having values depends on the oracle data indexing phase. Regarding 30d and life time stats like buy_volume_30d: These stats are calculated only if exchange is deemed active and we can convert the volume to a supported quote token. Any unsupported token volume does not show up in these stats. Useful mostly for risk assessment, as this data is **not** accurate, but gives some reference information about the popularity of the token.

## Constructor

```python
def __init__(chain_id: ChainId,
             chain_slug: str, 
             exchange_id: int,
             exchange_slug: str,
             address: str,
             exchange_type: ExchangeType,
             pair_count: int,
             active_pair_count: Optional[int] = None,
             first_trade_at: Optional[int] = None,
             last_trade_at: Optional[int] = None,
             name: Optional[str] = None,
             homepage: Optional[str] = None,
             buy_count_all_time: Optional[int] = None,
             sell_count_all_time: Optional[int] = None,
             buy_volume_all_time: Optional[float] = None,
             sell_volume_all_time: Optional[float] = None,
             buy_count_30d: Optional[int] = None,
             sell_count_30d: Optional[int] = None,
             buy_volume_30d: Optional[float] = None,
             sell_volume_30d: Optional[float] = None,
             default_router_address: Optional[str] = None,
             init_code_hash: Optional[str] = None) -> None
```

## Methods

- `__init__` - Constructor
- `from_dict` - Create instance from dictionary
- `from_json` - Create instance from JSON string  
- `schema` - Get schema information
- `to_dict` - Convert instance to dictionary
- `to_json` - Convert instance to JSON string

## Attributes

- `chain_id: ChainId` - The chain id on which chain this pair is trading (1 for Ethereum). For JSON, serialised as enum member name e.g. "ethereum".

- `chain_slug: str` - The URL slug derived from the blockchain name. Used as primary key in URLs. Example: "ethereum", "polygon"

- `exchange_id: int` - The exchange identifier

- `exchange_slug: str` - URL slug derived from exchange name. Used as primary key in URLs.

- `address: str` - Factory smart contract address of Uniswap based exchanges

- `exchange_type: ExchangeType` - Type of exchange

- `pair_count: int` - Number of discovered trading pairs for this exchange

- `active_pair_count: Optional[int]` - Number of supported trading pairs. See docs for more info.

- `first_trade_at: Optional[int]` - Timestamp of first trade on exchange

- `last_trade_at: Optional[int]` - Timestamp of most recent trade

- `name: Optional[str]` - Exchange name if known/guessed

- `homepage: Optional[str]` - Exchange homepage URL (https://)

- `buy_count_all_time: Optional[int]` - Lifetime buy transaction count
 
- `sell_count_all_time: Optional[int]` - Lifetime sell transaction count

- `buy_volume_all_time: Optional[float]` - Lifetime buy volume

- `sell_volume_all_time: Optional[float]` - Lifetime sell volume

- `buy_count_30d: Optional[int]` - 30 day buy transaction count

- `sell_count_30d: Optional[int]` - 30 day sell transaction count

- `buy_volume_30d: Optional[float]` - 30 day buy volume

- `sell_volume_30d: Optional[float]` - 30 day sell volume

- `default_router_address: Optional[str]` - Default router contract address if available

- `init_code_hash: Optional[str]` - Uniswap v2 init code hash (0x prefixed hex string)