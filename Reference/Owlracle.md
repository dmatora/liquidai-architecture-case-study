# Owlracle API Documentation

Owlracle API provides endpoints for easy integration with your app. Providing your users with Owlracle's data will ensure they never get stuck transactions again or overpay for gas, whether minting a fresh NFT, performing a swap at a DEX, lending, or borrowing assets.

Note that the first time you log in with your API key, you will be asked to confirm your email address. To be able to enjoy the free request limit, you need to provide your email first. Do not worry though, you will only be sent emails regarding Owlracle's new features and updates.

When you access our endpoints, it is recommended that you use an API key. You can request our data up to `100` times per hour for free. After reaching that limit, you can continue to make requests either by waiting a few minutes or using your api credit. You can also make requests without an api key for testing purposes, up to `10` times per hour. API keys without a bound email address cannot enjoy the increased free limit.

---

## Quick Start Integration Guide

It is very straightforward to use Owlracle API in your application. Whether you are building a website, a bot, or any other kind of software, the best-around gas price information can be fetched with a simple request.

### Sample Code Snippets

#### Node

```javascript
const fetch = require('node-fetch');
const network = 'eth'; // could be any supported network
const key = 'YOUR_API_KEY'; // fill your api key here

const res = await fetch(`https://api.owlracle.info/v4/${network}/gas?apikey=${key}`);
const data = await res.json();
console.log(data);
```

#### Python

```python
import requests
network = 'eth'  # could be any supported network
key = 'YOUR_API_KEY'  # fill your api key here

res = requests.get('https://api.owlracle.info/v4/{}/gas?apikey={}'.format(network, key))
data = res.json()
print(data)
```

#### PHP

```php
$network = 'eth'; // could be any supported network
$key = 'YOUR_API_KEY'; // fill your api key here
$url = "https://api.owlracle.info/v4/$network/gas?apikey=$key";

$curl = curl_init($url);
curl_setopt($curl, CURLOPT_URL, $url);
curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);

$resp = curl_exec($curl);
curl_close($curl);
var_dump($resp);
```

#### Java

```java
import java.net.*;
import java.io.*;

class Main {
    public static void main(String[] args) throws Exception {
        String network = "eth"; // could be any supported network
        String key = "YOUR_API_KEY"; // fill your api key here
        URL url = new URL(String.format("https://api.owlracle.info/v4/%s/gas?apikey=%s", network, key));
        URLConnection conn = url.openConnection();
        BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
        String inputLine;

        while ((inputLine = in.readLine()) != null) 
            System.out.println(inputLine);
        in.close();
    }
}
```

Suppose you are building a web3 app and want to use Owlracle's estimations in a smart contract interaction. In that case, it is just a matter of using the gas price information returned by your previous request in the `gasPrice` argument.

Below are a couple of examples—one for a browser-based Metamask request, and one for a Node script using web3.

#### Interaction using user's browser (Javascript + Metamask)

```javascript
// Interaction using user's browser through Metamask extension. 
// Docs: https://docs.metamask.io/guide/sending-transactions.html#example

const txHash = await ethereum.request({
    method: 'eth_sendTransaction',
    params: {
        from: ethereum.selectedAddress,  // Metamask's selected address
        to: '0x0000',                   // contract address
        gasPrice: 1000000000 * OWLRACLE_GAS, 
        // set the gas price received from Owlracle (in Gwei). 
        // The multiplication converts Gwei to Wei.
    },
});
```

#### Interaction using Node + web3 (example for BNB chain)

```javascript
// This sample code sends 1 BUSD from your address to Owlracle's address on the BNB chain.

const Web3 = require('web3');
const web3 = new Web3('https://bsc-dataseed.binance.org'); // BNB Chain RPC

// big info: get it from https://github.com/paxosglobal/busd-contract/blob/master/BUSD.abi
const ERC20TransferABI = 'CONTRACT_ABI_HERE';

const BUSD_ADDRESS = "0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56";
const busdToken = new web3.eth.Contract(ERC20TransferABI, BUSD_ADDRESS);

const privateKey = 'YOUR_ADDRESS_PRIVATE_KEY';
web3.eth.accounts.wallet.add(privateKey);
const senderAddress = "YOUR_ADDRESS";

const receiverAddress = "0xA6E126a5bA7aE209A92b16fcf464E502f27fb658"; // Owlracle address

// get token balance
busdToken.methods.balanceOf(senderAddress).call(function (err, res) {
    if (err) {
        console.log("An error occured", err);
        return;
    }
    console.log("The balance is: ", res);
});

// send 1 BUSD from your address to Owlracle
busdToken.methods.transfer(
    receiverAddress, 
    web3.utils.toWei('1', 'ether')
).send({
    from: senderAddress,
    gasPrice: web3.utils.toWei(OWLRACLE_DATA, 'gwei'), // insert owlracle gas price
    gas: web3.utils.toHex('320000'), // gas limit
}, function (err, res) {
    if (err) {
        console.log("An error occured", err);
        return;
    }
    console.log("Hash of the transaction: " + res);
});
```

---

## Owlracle's API endpoints

All public API endpoints are detailed below. After the endpoint description, you should see a table explaining each endpoint's arguments, response fields, and a sandbox to visually build your own requests.

---

### 1. Gas fee estimation

**Endpoint**  
```
GET https://api.owlracle.info/v4/eth/gas
```
Aliases for Ethereum: `ethereum`, `1`. For other networks, replace `eth` with the corresponding network alias.

Owlracle scans recent past blocks (`blocks` param) to build an estimate of required gas fee. It looks for the minimum gas accepted on a transaction for every scanned block (based on the chosen `percentile`), then shows you the minimum gas you should pay to be accepted on a percentage of your choice (`accept` param) of those blocks.

<details>
<summary><strong>Arguments</strong></summary>

| Field      | Type   | Description                                                                                                                                                                                                                                        | Default          |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|
| version    | param  | API version.                                                                                                                                                                                                                                        | v4              |
| network    | param  | Network you will request information from.                                                                                                                                                                                                          | eth             |
| apikey     | query  | Your API key. [Learn how to generate and use one](#api-keys-sec).                                                                                                                                                                                   | *none*          |
| blocks     | query  | Number of past blocks you want Owlracle to scan to build the estimation. <i>Maximum 1000</i>.                                                                                                                                                        | 200             |
| percentile | query  | Block gas percentile. For every analyzed block, Owlracle calculates the minimum gas needed to be accepted on that block. The value must be between 0.01 and 0.99 (percentile in ascending array) or an integer >= 1 (direct tx index).              | 0.3             |
| accept     | query  | Acceptance threshold. The percentage of blocks you want the transaction to be accepted in. You can provide a single value or comma-separated values (representing multiple speeds).                                                                  | 35,60,90,100    |
| feeinusd   | query  | If `false`, `estimatedFee` is reported in the native token (ETH, BNB, etc). Otherwise, in USD.                                                                                                                                                      | true            |
| eip1559    | query  | If `false`, the response uses legacy gas type. (Valid only for EIP-1559 networks.)                                                                                                                                                                  | true            |
| reportwei  | query  | If `true`, the fields `gasPrice`, `maxFeePerGas`, etc., will be reported in Wei (string) instead of Gwei (float).                                                                                                                                   | false           |
| calcfrom   | query  | If `'priorityFee'`, then `maxPriorityFeePerGas` is based on scanned blocks. Otherwise `'maxFee'` is the default.                                                                                                                                    | maxFee          |
| gasused    | query  | When `feeinusd` is enabled, this is the gas used to compute `estimatedFee`. If not informed, uses the average gas used in the scanned blocks.                                                                                                        | *none*          |

</details>

<details>
<summary><strong>Response</strong></summary>

| Field     | Description                                                                                                                                                               |
|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| timestamp | ISO 8601 date for when the API returned the result.                                                                                                                        |
| lastBlock | Number of the last block scanned.                                                                                                                                         |
| avgTime   | Average time between each block confirmation.                                                                                                                             |
| avgTx     | Average number of transactions in the blocks.                                                                                                                             |
| avgGas    | Average gas used on transactions in the scanned blocks.                                                                                                                   |
| speeds    | Array with the information for every speed requested in `accept`.                                                                                                         |
| acceptance                  | Ratio of blocks accepting transactions with the suggested gas fee.                                                                                 |
| baseFee                     | Maximum Base Fee reported from the network in the lower `accept`% of scanned blocks (only if EIP-1559 network + `eip1559=true`).                    |
| maxFeePerGas                | Suggested maximum gas price in GWei (if `eip1559=true`).                                                                                           |
| maxPriorityFeePerGas        | Suggested gas tip for miners in GWei (if `eip1559=true`).                                                                                         |
| gasPrice                    | Suggested gas price in GWei (if `eip1559=false`).                                                                                                  |
| estimatedFee                | Estimated fee in USD or native token.                                                                                                              |

Example JSON response:

```json
{
  "timestamp": "0000-00-00T00:00:00.000Z",
  "lastBlock": 0,
  "avgTime": 0,
  "avgTx": 0,
  "avgGas": 0,
  "speeds": [
    {
      "acceptance": 0,
      "maxFeePerGas": 0,
      "maxPriorityFeePerGas": 0,
      "baseFee": 0,
      "estimatedFee": 0
    }
  ]
}
```

---

### 2. Gas history

**Endpoint**  
```
GET https://api.owlracle.info/v4/eth/history
```
Retrieves the price history for gas, token price, and fee for the **Ethereum** network. If you prefer another network, replace `eth` with the corresponding network symbol.

The API returns an array of [candlesticks](https://www.investopedia.com/terms/c/candlestick.asp) built from aggregated data of the requested timeframe, ordered with decreasing timestamps.

<details>
<summary><strong>Arguments</strong></summary>

| Field      | Type   | Description                                                                                                                                                          | Default                |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| version    | param  | API version.                                                                                                                                                          | v4                    |
| network    | param  | Network you will request information from.                                                                                                                            | eth                   |
| apikey     | query  | Your API key. [Learn more](#api-keys-sec).                                                                                                                           | *none*                |
| from       | query  | [Unix timestamp](https://www.unixtimestamp.com/) representing the start time of the search.                                                                          | 0                     |
| to         | query  | Unix timestamp representing the end time of the search.                                                                                                              | current timestamp      |
| candles    | query  | Max number of results. <i>Maximum 1000</i>.                                                                                                                          | 100                   |
| page       | query  | The page to retrieve if the time range has more than `candles` results.                                                                                              | 1                     |
| timeframe  | query  | Time in minutes to aggregate results. Allowed: `10m`, `30m`, `1h`, `2h`, `4h`, `1d` or numeric equivalents (10, 30, 60, 120, 240, 1440).                              | 30                    |
| tokenprice | query  | Whether to include the native token price history in the response.                                                                                                   | false                 |
| txfee      | query  | Whether to include the historical average fee in USD for txs.                                                                                                        | false                 |

</details>

<details>
<summary><strong>Response</strong></summary>

| Field     | Description                                                                      |
|-----------|----------------------------------------------------------------------------------|
| timestamp | ISO 8601 date for the candlestick time.                                          |
| samples   | Number of samples composing the candle.                                          |
| avgGas    | Average gas limit set for transactions within the candle's blocks.               |
| gasPrice  | Object containing open, close, low, high of the gas price for that candle.       |
| tokenPrice| (Optional) Object with open, close, low, high of the native token price.         |
| txFee     | (Optional) Object with open, close, low, high for average gas fee in USD.        |

Example JSON response:

```json
{
  "candles": [
    {
      "timestamp": "0000-00-00T00:00:00.000Z",
      "samples": 0,
      "avgGas": 0,
      "gasPrice": {
        "open": 0,
        "close": 0,
        "low": 0,
        "high": 0
      },
      "tokenPrice": {
        "open": 0,
        "close": 0,
        "low": 0,
        "high": 0
      },
      "txFee": {
        "open": 0,
        "close": 0,
        "low": 0,
        "high": 0
      }
    }
  ]
}
```

---

### 3. API key information

**Endpoint**  
```
GET https://api.owlracle.info/v4/keys/:apikey
```
Request information about your API key.

<details>
<summary><strong>Arguments</strong></summary>

| Field   | Type   | Description                                                                    | Default |
|---------|--------|--------------------------------------------------------------------------------|---------|
| version | param  | API version.                                                                    | v4      |
| apikey  | param  | Your API key. [Learn more](#api-keys-sec).                                      | *none*  |

</details>

<details>
<summary><strong>Response</strong></summary>

| Field       | Description                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------|
| apiKey      | Your API key.                                                                                            |
| creation    | ISO 8601 date the API key was created.                                                                   |
| credit      | The unspent credit in USD.                                                                               |
| origin      | The request origin if you specified it during key creation (optional).                                   |
| note        | A personal note about the key if you specified one during creation (optional).                           |
| usage       | Object containing info about usage stats.                                                                |
| usage.ip1h      | Requests from your IP in the last hour.                                                          |
| usage.total1h   | Total requests made using your API key in the last hour.                                          |
| usage.charged1h | Requests beyond the free limit in the last hour.                                                  |

Example JSON response:

```json
{
  "apiKey": "00000000000000000000000000000000",
  "creation": "0000-00-00T00:00:00.000Z",
  "credit": "0.000000000",
  "origin": "domain.com",
  "note": "note to myself",
  "usage": {
    "ip1h": 0,
    "total1h": 0,
    "charged1h": 0
  }
}
```

---

### 4. API key credit recharge history

Get information about every API recharge.

**Endpoint**  
```
GET https://api.owlracle.info/v4/credit/:apikey
```

On success, the response is a JSON containing `message` and `results`. The `results` array has one object per recharge transaction:

| Field       | Description                                                                                                                             |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| network     | Network the transaction was sent on (e.g. "bsc", "poly", "ftm", "avax", "eth").                                                         |
| tx          | The transaction hash of your deposit. Check the network's block explorer for details.                                                  |
| timestamp   | ISO 8601 date for the transaction time.                                                                                                 |
| value       | Amount deposited in Gwei (1 Gwei = 0.000000001 ETH).                                                                                    |
| price       | ETH/USDT price at the transaction time.                                                                                                 |
| fromWallet  | The wallet that sent the credit to your API wallet.                                                                                     |

Example JSON response:

```json
{
  "recharges": [
    {
      "network": "xxx",
      "tx": "0x0000000000000000000000000000000000000000000000000000000000000000",
      "timestamp": "2000-00-00T00:00:00.000Z",
      "value": "0",
      "price": "0",
      "fromWallet": "0x0000000000000000000000000000000000000000"
    }
  ]
}
```

---

### 5. API key usage log

Get information about your API key usage.

**Endpoint**  
```
GET https://api.owlracle.info/v4/logs/:apikey
```

Optional arguments `fromtime` and `totime` can define the time range for your search.

<details>
<summary><strong>Arguments</strong></summary>

| Field     | Type   | Description                                         | Default                  |
|-----------|--------|-----------------------------------------------------|--------------------------|
| version   | param  | API version.                                        | v4                      |
| apikey    | param  | Your API key.                                       | *none*                  |
| fromtime  | query  | Start of the time range (unix timestamp).           | One hour in the past     |
| totime    | query  | End of the time range (unix timestamp).             | Current timestamp        |
| limit     | query  | Maximum number of log entries to receive.           | 1000                     |

</details>

<details>
<summary><strong>Response</strong></summary>

| Field     | Description                                                                                                                               |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------|
| ip        | The IP address of the request. `null` if from a private IP.                                                                               |
| origin    | The domain (website) that originates the request. `null` if not from a website (e.g., called from a local client).                         |
| timestamp | ISO 8601 date for the request time.                                                                                                       |
| endpoint  | The endpoint requested. Possible values: `"gas"`, `"history"`.                                                                            |
| network   | The requested network. Possible values: symbol for any supported network (e.g. `"bsc"`, `"poly"`, `"ftm"`, `"avax"`, `"eth"`).            |

Example JSON response:

```json
{
  "logs": [
    {
      "ip": "255.255.255.255",
      "origin": "domain.com",
      "timestamp": "0000-00-00T00:00:00.000Z",
      "endpoint": "xxx",
      "network": "xxx"
    }
  ]
}
```

---

### 6. RPC endpoint

Get information about the RPCs used by Owlracle.

**Endpoint**  
```
GET https://api.owlracle.info/rpc
```

<details>
<summary><strong>Response</strong></summary>

| Field    | Description                                                                                          |
|----------|------------------------------------------------------------------------------------------------------|
| network  | Network the RPC is connected to.                                                                     |
| rpc      | The RPC URL.                                                                                        |
| lastTime | The last time the RPC reported a new block.                                                          |
| timeDiff | The difference between the last block time and the time the RPC was checked.                         |
| healthy  | Whether the RPC is healthy or not (`timeDiff < 60` seconds).                                         |

Example JSON response:

```json
[
  {
    "network": "xxx",
    "rpc": "https://urltotherpc.com",
    "lastTime": 0,
    "timeDiff": 0,
    "healthy": true
  }
]
```

---

## SW3 API

With the Seamless Web3 API, you can easily onboard your users to the blockchain. It is a very powerful API that allows you to create wallets, send transactions, and more. All your users need is a username and password while navigating your app.

Check out the [SW3 API documentation](https://api.owlracle.info/sw3) for more information.
