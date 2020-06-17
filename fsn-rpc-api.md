# FUSION JSON RPC API

FUSION RPC is compatible with Ethereum's [JSON-RPC](https://github.com/ethereum/wiki/wiki/JSON-RPC) and [web3.js](https://github.com/ethereum/web3.js) API. In addition, FUSION's extended API include: Ticket, Asset, Timelock, USAN, Swap, Staking, etc.

## JSON RPC

[JSON](http://json.org/) is a lightweight data-interchange format. It can represent numbers, strings, ordered sequences of values, and collections of name/value pairs.

[JSON-RPC](http://www.jsonrpc.org/specification) is a stateless, light-weight remote procedure call (RPC) protocol. Primarily this specification defines several data structures and the rules around their processing. It is transport agnostic in that the concepts can be used within the same process, over sockets, over HTTP, or in many various message passing environments. It uses JSON ([RFC 4627](http://www.ietf.org/rfc/rfc4627.txt)) as data format.


## JavaScript API

To talk to an fusion node from inside a JavaScript application use the [web3.js](https://github.com/ethereum/web3.js) library, which gives a convenient interface for the RPC methods.
FUSION's extended JavaScript API, See the [JavaScript API](https://fusionapi.readthedocs.io/) for more.

## JSON-RPC Endpoint

Online RPC service:

Testnet: https://testnet.fsn.dev/api

Mainnet: https://fsn.dev/api

Default JSON-RPC endpoints:

| Client | URL |
|-------|:------------:|
| Go |http://localhost:9000 |


You can start the HTTP JSON-RPC with the `--rpc` flag
```bash
efsn --rpc
```

change the default port (9000) and listing address (localhost) with:

```bash
efsn --rpc --rpcaddr <ip> --rpcport <portnumber>
```

If accessing the RPC from a browser, CORS will need to be enabled with the appropriate domain set. Otherwise, JavaScript calls are limit by the same-origin policy and requests will fail:

```bash
efsn --rpc --rpccorsdomain "http://localhost:9000"
```

The JSON RPC can also be started from the efsn console using the `admin.startRPC(addr, port)` command.

## Curl Examples Explained

The curl options below might return a response where the node complains about the content type, this is because the --data option sets the content type to application/x-www-form-urlencoded . If your node does complain, manually set the header by placing -H "Content-Type: application/json" at the start of the call.

The examples also do not include the URL/IP & port combination which must be the last argument given to curl e.x. 127.0.0.1:9000

## JSON-RPC methods

* [fsn_ticketPrice](#fsn_ticketPrice)
* [fsn_getStakeInfo](#fsn_getStakeInfo)
* [fsn_getBlockReward](#fsn_getBlockReward)
* [fsntx_buyTicket](#fsntx_buyTicket)
* [fsntx_buildBuyTicketTx](#fsntx_buildBuyTicketTx)
* [fsn_allTickets](#fsn_allTickets)
* [fsn_allTicketsByAddress](#fsn_allTicketsByAddress)
* [fsn_totalNumberOfTickets](#fsn_totalNumberOfTickets)
* [fsn_totalNumberOfTicketsByAddress](#fsn_totalNumberOfTicketsByAddress)
* [fsntx_sendTimeLock](#fsntx_sendTimeLock)
* [fsntx_assetToTimeLock](#fsntx_assetToTimeLock)
* [fsntx_buildAssetToTimeLockTx](#fsntx_buildAssetToTimeLockTx)
* [fsntx_timeLockToTimeLock](#fsntx_timeLockToTimeLock)
* [fsntx_buildTimeLockToTimeLockTx](#fsntx_buildTimeLockToTimeLockTx)
* [fsntx_timeLockToAsset](#fsntx_timeLockToAsset)
* [fsntx_buildTimeLockToAssetTx](#fsntx_buildTimeLockToAssetTx)
* [fsn_getTimeLockValueByInterval](#fsn_getTimeLockValueByInterval)
* [fsn_getTimeLockBalance](#fsn_getTimeLockBalance)
* [fsn_getAllTimeLockBalances](#fsn_getAllTimeLockBalances)
* [fsntx_genAsset](#fsntx_genAsset)
* [fsntx_buildGenAssetTx](#fsntx_buildGenAssetTx)
* [fsntx_decAsset](#fsntx_decAsset)
* [fsntx_buildDecAssetTx](#fsntx_buildDecAssetTx)
* [fsntx_incAsset](#fsntx_incAsset)
* [fsntx_buildIncAssetTx](#fsntx_buildIncAssetTx)
* [fsntx_sendAsset](#fsntx_sendAsset)
* [fsntx_buildSendAssetTx](#fsntx_buildSendAssetTx)
* [fsn_getAllBalances](#fsn_getAllBalances)
* [fsn_getLatestNotation](#fsn_getLatestNotation)
* [fsntx_genNotation](#fsntx_genNotation)
* [fsntx_buildGenNotationTx](#fsntx_buildGenNotationTx)
* [fsn_getNotation](#fsn_getNotation)
* [fsn_getAddressByNotation](#fsn_getAddressByNotation)
* [fsn_getAsset](#fsn_getAsset)
* [fsn_getBalance](#fsn_getBalance)
* [fsntx_makeSwap](#fsntx_makeSwap)
* [fsntx_buildMakeSwapTx](#fsntx_buildMakeSwapTx)
* [fsn_getSwap](#fsn_getSwap)
* [fsntx_takeSwap](#fsntx_takeSwap)
* [fsntx_buildTakeSwapTx](#fsntx_buildTakeSwapTx)
* [fsntx_recallSwap](#fsntx_recallSwap)
* [fsntx_buildRecallSwapTx](#fsntx_buildRecallSwapTx)
* [fsntx_makeMultiSwap](#fsntx_makeMultiSwap)
* [fsntx_buildMakeMultiSwapTx](#fsntx_buildMakeMultiSwapTx)
* [fsn_getMultiSwap](#fsn_getMultiSwap)
* [fsntx_takeMultiSwap](#fsntx_takeMultiSwap)
* [fsntx_buildTakeMultiSwapTx](#fsntx_buildTakeMultiSwapTx)
* [fsntx_recallMultiSwap](#fsntx_recallMultiSwap)
* [fsntx_buildRecallMultiSwapTx](#fsntx_buildRecallMultiSwapTx)
* [fsntx_isAutoBuyTicket](#fsntx_isAutoBuyTicket)
* [miner_startAutoBuyTicket](#miner_startAutoBuyTicket)
* [miner_stopAutoBuyTicket](#miner_stopAutoBuyTicket)
* [fsn_getTransactionAndReceipt](#fsn_getTransactionAndReceipt)
* [fsn_allInfoByAddress](#fsn_allInfoByAddress)

## JSON RPC API Reference

***

#### fsn_ticketPrice

Return price of the ticket.

##### Parameters

`String|HexNumber|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.

```js
params: [
  "latest"  // state at the latest block
]
```

##### Return

`QUANTITY` - integer of the current ticket price in wei.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_ticketPrice","params":["latest"],"id":1}'

// Result
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "5000000000000000000000"
}

```

***

#### fsn_getStakeInfo

Returns information about all miners.

##### Parameters

`String|HexNumber|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.

```js
params: [
  "latest"  // state at the latest block
]
```

##### Return

`DATA`  - all miners' address, total number of miners and tickets.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getStakeInfo","params":["latest"],"id":1}'

// Result
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "stakeInfo": {
      "0x3a1b3b81ed061581558a81f11d63e03129347437": 2
    },
    "summary": {
      "totalMiners": 1,
      "totalTickets": 2
    }
  }
}

```
#### fsn_getBlockReward 

Returns block rewards.

##### Parameters

`String|HexNumber|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.

```js
params: [
  "latest"  // state at the latest block
]
```

##### Return

`DATA`  - the rewards of the block.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getBlockReward","params":["latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"2500021952000000000"
}

```
***

#### fsntx_buyTicket

Return `hash` if successful ticket purchase. (Account has been unlocked)

##### Parameters

`DATA`, from - The address for the account.

```js
params: [{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac"}]
```

##### Return

`DATA`, 32 Bytes - The transaction hash.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buyTicket","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x7bee2c9931da61d53da0cfbbc35b7614dd0283959f2e6023dc7792baeafe97f5"
}

```
***

#### fsntx_buildBuyTicketTx

Build buyTicket unsigned raw transaction.   

##### Parameters

`DATA`, from - The address for the account.

```js
params: [{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac"}]
```

##### Return

`DATA`,  unsigned raw transaction.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildBuyTicketTx","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "nonce":"0x0",
    "gasPrice":"0x3b9aca00",
    "gas":"0x15f90",
    "to":"0xffffffffffffffffffffffffffffffffffffffff",
    "value":"0x0",
    "input":"0xcd048bca845ee88333845f101033",
    "v":"0x0",
    "r":"0x0",
    "s":"0x0",
    "hash":"0xe73510d1333c4eea413325cfb0774de7e27dbf5417f1bbc52aab45c9614e850a"
    }
}

```

***

#### fsn_allTickets

Return all tickets to all accounts.

##### Parameters

`String|HexNumber|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.

```js
params: [
  "latest"  // state at the latest block
]
```

##### Return

`DATA` - All tickets.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_allTickets","params":["latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "0x07d523bfcfc6d1cb5973403e799b7808f94ea0a6b3fca745a9d814c3813015ca":
    {
      "Owner":"0x3a1b3b81ed061581558a81f11d63e03129347437",
      "Height":132,
      "StartTime":1567405502,
      "ExpireTime":1569997502,
      "Value":5000000000000000000000
      },
    "0x0932192a10e7be74ea33ebd83b4cdffa613c0bd896803ac03b67b52f06cc20d7":
    {
      "Owner":"0x0963a18ea497b7724340fdfe4ff6e060d3f9e388",
      "Height":113,
      "StartTime":1567405255,
      "ExpireTime":1569997255,
      "Value":5000000000000000000000
      }
    }
}
```

***

#### fsn_allTicketsByAddress

Return all tickets of this address.

##### Parameters

1. `DATA`, address - The address for the account.
2. `String|HexNumber|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.

```js
params: [
  "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "latest"  // state at the latest block
]
```

##### Return

`DATA` - All tickets of the address.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_allTicketsByAddress","params":["0x5b15a29274c74cd7cae59cabf656873a0ea706ac","latest"],"id":1}

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":
  {
    "0x5bd28f3dbe5d56294c4f8db4f6db86df2210544e1d17d127930742a1f0b7edda":
    {
      "Owner":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
      "Height":180,
      "StartTime":1567406170,
      "ExpireTime":1569998170,
      "Value":5000000000000000000000
      }
    }
}
```

***

#### fsn_totalNumberOfTickets

Return the total number of all addresses.

##### Parameters

`String|HexNumber|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.

```js
params: [
  "latest"  // state at the latest block
]
```

##### Return

`Number` - The total number of all tickets.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_totalNumberOfTickets","params":["latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":67
}
```

***

#### fsn_totalNumberOfTicketsByAddress

Return the total number of tickets by address.

##### Parameters

1. `String|Address`, 20 Bytes - The address for the account.
2. `String|HexNumber|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.

```js
params: [
  "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "latest"  // state at the latest block
]
```

##### Return

`Number` - The total number tickets of the address.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_totalNumberOfTicketsByAddress","params":["0x5b15a29274c74cd7cae59cabf656873a0ea706ac","latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":1
}
```

***
#### fsntx_sendTimeLock

Convert assets or timelock to timelock balance.(Account has been unlocked)

##### Parameters

1. `String|Address`, from - The address for the sending account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, asset -  The hash of the asset.
6. `String|Address|Number`, to|toUSAN - The address for the receiving account | The notation of receiving account address.
7. `String|BigNumber`, value -  The value for sending.
8. `String|HexNumber`, start - (optional) The start time of the time lock.
9. `Number`, end - (optional) The end time of the time lock.


```js
1.params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0xd1c675d3fc4c19d71b50bfe056a09627ca7e85a1",
  "to":"0x66272b2bc4f7bc74f81a40ee8f4259bcf93d638e",
  "start":"0x5e4f5011", //2020/2/21 11:35:45
  "end":"0x5e758b91", //2020/3/21 11:35:45
  "value":"0x1BC16D674EC80000" //2,000,000,000,000,000,000
}]
2.params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0xd1c675d3fc4c19d71b50bfe056a09627ca7e85a1",
  "toUSAN":104,
  "start":"0x5e4f5011", //2020/2/21 11:35:45
  "end":"0x5e758b91", //2020/3/21 11:35:45
  "value":"0x1BC16D674EC80000" //2,000,000,000,000,000,000
}]
```

##### Return

`DATA`, 32 Bytes - The transaction hash.

##### Example
```js
// Request
1.curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_sendTimeLock","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0xd1c675d3fc4c19d71b50bfe056a09627ca7e85a1","to":"0x66272b2bc4f7bc74f81a40ee8f4259bcf93d638e","start":"0x5e4f5011","end":"0x5e758b91","value":"0x1BC16D674EC80000"}],"id":1}'

2.curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_sendTimeLock","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0xd1c675d3fc4c19d71b50bfe056a09627ca7e85a1","toUSAN":104,"start":"0x5e4f5011","end":"0x5e758b91","value":"0x1BC16D674EC80000"}],"id":1}'
// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xd3c4777df1e1ec47586c1ec16952dcdcc328f4a27808793824a7041a1a2035d6"
}

```

#### fsntx_assetToTimeLock

Convert assets to timelock balance.(Account has been unlocked)

##### Parameters

1. `String|Address`, from - The address for the sending account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, asset -  The hash of the asset.
6. `String|Address|Number`, to|toUSAN - The address for the receiving account | The notation of receiving account address.
7. `String|BigNumber`, value -  The value for sending.
8. `String|HexNumber`, start - (optional) The start time of the time lock.
9. `Number`, end - (optional) The end time of the time lock.


```js
1.params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "start":"0x5D6CD2FC", //2019/9/2 16:29:48
  "end":"0x5D945FFC", //2019/10/2 16:29:48
  "value":"0x1BC16D674EC80000" //2,000,000,000,000,000,000
}]
2.params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "toUSAN":104,
  "start":"0x5D6CD2FC", //2019/9/2 16:29:48
  "end":"0x5D945FFC", //2019/10/2 16:29:48
  "value":"0x1BC16D674EC80000" //2,000,000,000,000,000,000
}]
```

##### Return

`DATA`, 32 Bytes - The transaction hash.

##### Example
```js
// Request
1.curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_assetToTimeLock","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","start":"0x5D6CD2FC","end":"0x5D945FFC","value":"0x1BC16D674EC80000"}],"id":1}'

2.curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_assetToTimeLock","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","toUSAN":104,"start":"0x5D6CD2FC","end":"0x5D945FFC","value":"0x1BC16D674EC80000"}],"id":1}'
// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xa5c08546790e19974fbbb554878289147e439fff9f57e12cb40e63d1e3ea8096"
}

```
***
#### fsntx_buildAssetToTimeLockTx

Build assetToTimeLock unsigned raw transaction.  

##### Parameters

1. `String|Address`, from - The address for the sending account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, asset -  The hash of the asset.
6. `String|Address|Number`, to|toUSAN - The address for the receiving account | The notation of receiving account address.
7. `String|BigNumber`, value -  The value for sending.
8. `String|HexNumber`, start - (optional) The start time of the time lock.
9. `Number`, end - (optional) The end time of the time lock.


```js
1.params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "to":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "start":"0x5ee884db", //2020/6/16 16:37:32
  "end":"0x5f1011cc", //2020/7/16 16:37:32
  "value":"0x1BC16D674EC80000" //2,000,000,000,000,000,000
}]
2.params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "toUSAN":104,
  "start":"0x5ee884db", //2020/6/16 16:37:32
  "end":"0x5f1011cc", //2020/7/16 16:37:32
  "value":"0x1BC16D674EC80000" //2,000,000,000,000,000,000
}]
```

##### Return

`DATA`,  unsigned raw transaction.

##### Example
```js
// Request
1.curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildAssetToTimeLockTx","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","to":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","start":"0x5ee884db","end":"0x5f1011cc","value":"0x1BC16D674EC80000"}],"id":1}'

2.curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildAssetToTimeLockTx","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","toUSAN":104,"start":"0x5ee884db","end":"0x5f1011cc","value":"0x1BC16D674EC80000"}],"id":1}'
// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "nonce":"0x0",
    "gasPrice":"0x3b9aca00",
    "gas":"0x15f90",
    "to":"0xffffffffffffffffffffffffffffffffffffffff",
    "value":"0x0",
    "input":"0xf84f03b84cf84a80a0ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff94cc9ea1c564fa513d6abd9339587dc4f886d7acc4845ee884db845f1011cc881bc16d674ec80000",
    "v":"0x0",
    "r":"0x0",
    "s":"0x0",
    "hash":"0x169f3ee737cc46f3ba020e9311b9e91ff9d46beb47bdb2d605e7601d97b9d545"
    }
}

```

***

#### fsntx_timeLockToTimeLock

Send a time locked asset to another account.(Account has been unlocked)

##### Parameters

1. `String|Address`, from - The address for the account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, asset -  The hash of the asset.
6. `String|Address|Number`, to|toUSAN - The address for the receiving account | The notation of receiving account address.
7. `String|BigNumber`, value -  The value for sending.
8. `String|HexNumber`, start -  The start time of the time lock.
9. `String|HexNumber`, end - The end time of the time lock.


```js
1. params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "to":"0x07718f21f889b84451727ada8c65952a597b2e78",
  "start":"0x5D6CD2FC",  //2019/9/2 16:29:48
  "end":"0x5D945FFC",   //2019/10/2 16:29:48
  "value":"0x1BC16D674EC80000"  //2,000,000,000,000,000,000‬
  }]

2. params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "toUSAN":104,
  "start":"0x5D6CD2FC",  //2019/9/2 16:29:48
  "end":"0x5D945FFC",   //2019/10/2 16:29:48
  "value":"0x1BC16D674EC80000"  //2,000,000,000,000,000,000‬
  }]
```

##### Return

`DATA`, 32 Bytes - The transaction hash.

##### Example
```js
// Request
1. curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_timeLockToTimeLock","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","to":"0x07718f21f889b84451727ada8c65952a597b2e78","start":"0x5D6CD2FC","end":"0x5D945FFC","value":"0x1BC16D674EC80000"}],"id":1}'

2. curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_timeLockToTimeLock","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","toUSAN":104,"start":"0x5D6CD2FC","end":"0x5D945FFC","value":"0x1BC16D674EC80000"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x7ca4fa378a8ece6f2db1fe41521c80edcda45fff270b0bc12c81ab269c651d27"
}
```
***

#### fsntx_buildTimeLockToTimeLockTx

Build timeLockToTimeLock unsigned raw transaction.
##### Parameters

1. `String|Address`, from - The address for the account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, asset -  The hash of the asset.
6. `String|Address|Number`, to|toUSAN - The address for the receiving account | The notation of receiving account address.
7. `String|BigNumber`, value -  The value for sending.
8. `String|HexNumber`, start -  The start time of the time lock.
9. `String|HexNumber`, end - The end time of the time lock.


```js
1. params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "to":"0x07718f21f889b84451727ada8c65952a597b2e78",
  "start":"0x5ee884db", //2020/6/16 16:37:32
  "end":"0x5f1011cc", //2020/7/16 16:37:32
  "value":"0x1BC16D674EC80000"  //2,000,000,000,000,000,000‬
  }]

2. params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "toUSAN":104,
  "start":"0x5ee884db", //2020/6/16 16:37:32
  "end":"0x5f1011cc", //2020/7/16 16:37:32
  "value":"0x1BC16D674EC80000"  //2,000,000,000,000,000,000‬
  }]
```

##### Return

`DATA`,  unsigned raw transaction.

##### Example
```js
// Request
1. curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildTimeLockToTimeLockTx","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","to":"0x07718f21f889b84451727ada8c65952a597b2e78","start":"0x5ee884db","end":"0x5f1011cc","value":"0x1BC16D674EC80000"}],"id":1}'

2. curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildTimeLockToTimeLockTx","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","toUSAN":104,"start":"0x5ee884db","end":"0x5f1011cc","value":"0x1BC16D674EC80000"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "nonce":"0x0",
    "gasPrice":"0x3b9aca00",
    "gas":"0x15f90",
    "to":"0xffffffffffffffffffffffffffffffffffffffff",
    "value":"0x0",
    "input":"0xf84f03b84cf84a01a0ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff9407718f21f889b84451727ada8c65952a597b2e78845ee884db845f1011cc881bc16d674ec80000",
    "v":"0x0",
    "r":"0x0",
    "s":"0x0",
    "hash":"0xc27d2840674a0f6eae2b393668ce96adc143047f1f3f39e9dbb19498c94689d7"
    }
}
```

***

#### fsntx_timeLockToAsset

Convert timelock balance to asset.(Account has been unlocked)


##### Parameters

1. `String|Address`, from - The address for the account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, asset -  The hash of the asset.
6. `String|Address|Number`, to|toUSAN - The address for the receiving account | The notation of receiving account address.
7. `String|BigNumber`, value -  The value for sending.
8. `String|HexNumber`, start - (optional) The start time of the time lock.
9. `String|HexNumber`, end - (optional) The end time of the time lock.

```js
1. params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "start":"0x5D6CD2FC",  //2019/9/2 16:29:48
  "end":"0x5D945FFC",   //2019/10/2 16:29:48
  "value":"0x1BC16D674EC80000"  //2,000,000,000,000,000,000‬
  }]

2. params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "toUSAN":104,
  "start":"0x5D6CD2FC",  //2019/9/2 16:29:48
  "end":"0x5D945FFC",   //2019/10/2 16:29:48
  "value":"0x1BC16D674EC80000"  //2,000,000,000,000,000,000‬
  }]
```

##### Return

`DATA`, 32 Bytes - The transaction hash.

##### Example
```js
// Request
1.curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_timeLockToAsset","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","start":"0x5D6CD2FC","end":"0x5D945FFC","value":"0x1BC16D674EC80000"}],"id":1}'

2.curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_timeLockToAsset","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","toUSAN":104,"start":"0x5D6CD2FC","end":"0x5D945FFC","value":"0x1BC16D674EC80000"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xb8242235c1f4fa1bcbc22b65bdfd2ab19db97cdfdc3b804fa302f87eafcd69c2"
}
```
***
#### fsntx_buildTimeLockToAssetTx

Build timeLockToAsset unsigned raw transaction.


##### Parameters

1. `String|Address`, from - The address for the account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, asset -  The hash of the asset.
6. `String|Address|Number`, to|toUSAN - The address for the receiving account | The notation of receiving account address.
7. `String|BigNumber`, value -  The value for sending.
8. `String|HexNumber`, start - (optional) The start time of the time lock.
9. `String|HexNumber`, end - (optional) The end time of the time lock.

```js
1. params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "start":"0x5ee884db", //2020/6/16 16:37:32
  "end":"0x5f1011cc", //2020/7/16 16:37:32
  "value":"0x1BC16D674EC80000"  //2,000,000,000,000,000,000‬
  }]

2. params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "toUSAN":104,
  "start":"0x5ee884db", //2020/6/16 16:37:32
  "end":"0x5f1011cc", //2020/7/16 16:37:32
  "value":"0x1BC16D674EC80000"  //2,000,000,000,000,000,000‬
  }]
```

##### Return

`DATA`,  unsigned raw transaction.

##### Example
```js
// Request
1.curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildTimeLockToAssetTx","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","to":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","start":"0x5ee884db","end":"0x5f1011cc","value":"0x1BC16D674EC80000"}],"id":1}'

2.curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildTimeLockToAssetTx","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","toUSAN":104,"start":"0x5ee884db","end":"0x5f1011cc","value":"0x1BC16D674EC80000"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "nonce":"0x0",
    "gasPrice":"0x3b9aca00",
    "gas":"0x15f90",
    "to":"0xffffffffffffffffffffffffffffffffffffffff",
    "value":"0x0",
    "input":"0xf85303b850f84e02a0ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff94cc9ea1c564fa513d6abd9339587dc4f886d7acc4845ee88d1a88ffffffffffffffff881bc16d674ec80000",
    "v":"0x0",
    "r":"0x0",
    "s":"0x0",
    "hash":"0xfe26e01c9f17a0c6c6b037c8872c61a398abb324fca4bf4387c55f63e63b468b"
    }
}
```

***

#### fsn_getTimeLockValueByInterval

Return transfer amount within specified time.


##### Parameters

1. `String|Hash`, assetID - The hash of the asset.
2. `String|Address`, address - The address for the account.
3. `String|HexNumber`, start - (optional) The start time of the time lock.
4. `String|HexNumber`, end - (optional) The end time of the time lock.
5. `String|HexNumber|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.


```js
params: [{
  "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "0xd1c675d3fc4c19d71b50bfe056a09627ca7e85a1",
  0,          // start time
  0,         // end time
  "latest"  // state at the latest block
  }]
```

##### Return

`DATA`, The transfer amount within specified time.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getTimeLockValueByInterval","params":["0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","0xd1c675d3fc4c19d71b50bfe056a09627ca7e85a1",0,0,"latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"6000000000000000000"
}
```

***

#### fsn_getTimeLockBalance

Return time locked balance of assetID.


##### Parameters

1. `String|Hash`, assetID - The hash of the asset.
2. `String|Address`, address - The address for the account.
3. `String|HexNumber|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.


```js
params: [{
  "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "latest"  // state at the latest block
  }]
```

##### Return

`DATA`, The detail of the TimeLock balance.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getTimeLockBalance","params":["0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","0x5b15a29274c74cd7cae59cabf656873a0ea706ac","latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":
  {
    "Items":
    [{
      "StartTime":1570002245,
      "EndTime":18446744073709551615,
      "Value":"5000000000000000000000"
      }]
    }
}
```

***

#### fsn_getAllTimeLockBalances

Return all time lock balances.

##### Parameters

1. `DATA`, 20 Bytes - The address for the account.
2. `String|HexNumber|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.

```js
params: [{
  "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "latest"  // state at the latest block
  }]
```

##### Return

`DATA`, The detail of the TimeLock balance.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getAllTimeLockBalances","params":["0x5b15a29274c74cd7cae59cabf656873a0ea706ac","latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":
  {
    "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff":
    {
      "Items":
      [{
        "StartTime":1570002245,
        "EndTime":18446744073709551615,
        "Value":"5000000000000000000000"
        }]
        }
      }
}

```

***

#### fsntx_genAsset

 Generate new asset.(Account has been unlocked)

##### Parameters

1. `String|Address`, from - The address for the account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String`, name - The name for the asset.
6. `String`, symbol -  The symbol for the asset.
7. `Number`, decimals -  The decimals for the asset.
8. `String|BigNumber`, total -  The total value for the asset.
9. `bool`, canChange -  Values can be changed.  

```js
params: [{
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "name":"FusionTest",
  "symbol":"FT",
  "decimals":1,
  "total":"0x200",
  "canChange":true
  }]
```

##### Return

`DATA`, 32 Bytes - The transaction hash.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_genAsset","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","name":"FusionTest","symbol":"FT","decimals":1,"total":"0x200","canChange":true}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x6e566d3de4da6bd8628ddc228dd571ddcc7e708ff11d02152f810570fb0d0b9a"
}

```
***

#### fsntx_buildGenAssetTx

Build genAsset unsigned raw transaction.

##### Parameters

1. `String|Address`, from - The address for the account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String`, name - The name for the asset.
6. `String`, symbol -  The symbol for the asset.
7. `Number`, decimals -  The decimals for the asset.
8. `String|BigNumber`, total -  The total value for the asset.
9. `bool`, canChange -  Values can be changed.  

```js
params: [{
  "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "name":"FusionTest",
  "symbol":"FT",
  "decimals":1,
  "total":"0x200",
  "canChange":true
  }]
```

##### Return

`DATA`,  unsigned raw transaction.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildGenAssetTx","params":[{"from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","name":"FusionTest","symbol":"FT","decimals":1,"total":"0x200","canChange":true}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "nonce":"0x0",
    "gasPrice":"0x3b9aca00",
    "gas":"0x15f90",
    "to":"0xffffffffffffffffffffffffffffffffffffffff",
    "value":"0x0",
    "input":"0xd70195d48a467573696f6e54657374824654018202000180",
    "v":"0x0",
    "r":"0x0",
    "s":"0x0",
    "hash":"0x4473c9c46aa575aca6665e0b485c5bb85eee55cb6161a002140c4829066f459b"
    }
}

```

***

#### fsntx_decAsset

Reduction the total amount of asset.(Account has been unlocked && "canChange" is true for the asset)

##### Parameters

1. `String|Address`, from - The address for the account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, asset - The hash of the asset.
6. `String|Address`, to - The address for the receiving account.
7. `String|BigNumber`, value - The value of decrease.
8. `String`, transacData - (optional) The data for transaction.


```js
params: [{
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "value":"0x10",
  "asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"
  }]
```

##### Return

`DATA`, 32 Bytes - The transaction hash.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_decAsset","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","value":"0x10","asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x85d95f61676e99046053346419129afec9d7a8fee2924d9062419aee253f17a4"
}
```
***

#### fsntx_buildDecAssetTx

Build decAsset unsigned raw transaction.

##### Parameters

1. `String|Address`, from - The address for the account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, asset - The hash of the asset.
6. `String|Address`, to - The address for the receiving account.
7. `String|BigNumber`, value - The value of decrease.
8. `String`, transacData - (optional) The data for transaction.


```js
params: [{
  "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "to":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "value":"0x10",
  "asset":"0x4473c9c46aa575aca6665e0b485c5bb85eee55cb6161a002140c4829066f459b"
  }]
```

##### Return

`DATA`,  unsigned raw transaction.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildDecAssetTx","params":[{"from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","to":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","value":"0x10","asset":"0x4473c9c46aa575aca6665e0b485c5bb85eee55cb6161a002140c4829066f459b"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "nonce":"0x1",
    "gasPrice":"0x3b9aca00",
    "gas":"0x15f90",
    "to":"0xffffffffffffffffffffffffffffffffffffffff",
    "value":"0x0",
    "input":"0xf83e0cb83bf839a04473c9c46aa575aca6665e0b485c5bb85eee55cb6161a002140c4829066f459b94cc9ea1c564fa513d6abd9339587dc4f886d7acc4108080",
    "v":"0x0",
    "r":"0x0",
    "s":"0x0",
    "hash":"0x9798a2295357a31484948f6cbbd47f0fbfd82f6de79480383af747d75039356f"
    }
}
```

***

#### fsntx_incAsset

Increase the total amount of asset.(Account has been unlocked && "canChange" is true for the asset)


##### Parameters

1. `String|Address`, from - The address for the account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, asset - The hash of the asset.
6. `String|Address`, to - The address for the receiving account.
7. `String|BigNumber`, value - The value of increase.
8. `String`, transacData - (optional) The data for transaction.


```js
params: [{
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "value":"0x10",
  "asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"
  }]
```

##### Return

`DATA`, 32 Bytes - The transaction hash.

##### Example
```js
// Request
 curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_incAsset","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","value":"0x10","asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xdb48fa3fdad4ad6c85b3700d89c37e776cb4fe61487425c752b863c25fe8a7d8"
}
```
***

#### fsntx_buildIncAssetTx

Build incAsset unsigned raw transaction.


##### Parameters

1. `String|Address`, from - The address for the account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, asset - The hash of the asset.
6. `String|Address`, to - The address for the receiving account.
7. `String|BigNumber`, value - The value of increase.
8. `String`, transacData - (optional) The data for transaction.


```js
params: [{
  "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "to":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "value":"0x10",
  "asset":"0x4473c9c46aa575aca6665e0b485c5bb85eee55cb6161a002140c4829066f459b"
  }]
```

##### Return

`DATA`,  unsigned raw transaction.

##### Example
```js
// Request
 curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildIncAssetTx","params":[{"from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","to":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","value":"0x10","asset":"0x4473c9c46aa575aca6665e0b485c5bb85eee55cb6161a002140c4829066f459b"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "nonce":"0x1",
    "gasPrice":"0x3b9aca00",
    "gas":"0x15f90",
    "to":"0xffffffffffffffffffffffffffffffffffffffff",
    "value":"0x0",
    "input":"0xf83e0cb83bf839a04473c9c46aa575aca6665e0b485c5bb85eee55cb6161a002140c4829066f459b94cc9ea1c564fa513d6abd9339587dc4f886d7acc4100180",
    "v":"0x0",
    "r":"0x0",
    "s":"0x0",
    "hash":"0x1caa8be9419ba4e9a8ddf7d262af46fbbc11d879b94356f11f807a4d4d291db1"
    }
}
```

***

#### fsntx_sendAsset

Send assets to other accounts.(Account has been unlocked)

##### Parameters

1. `String|Address`, from - The address for the sending account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, asset - The hash of the asset.
6. `String|Address|Number`, to|toUSAN - The address for the receiving account | The notation of receiving account address.
7. `String|BigNumber`, value - The value for sending.


```js
1. params: [{
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "to":"0x07718f21f889b84451727ada8c65952a597b2e78",
  "value":"0x10",
  "asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"
  }]
2. params:[{
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "toUSAN":104,
  "value":"0x10",
  "asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"
}]

```

##### Return

`DATA`, 32 Bytes - The transaction hash.

##### Example
```js
// Request
1. curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_sendAsset","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","to":"0x07718f21f889b84451727ada8c65952a597b2e78","value":"0x10","asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"}],"id":1}'
2. curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_sendAsset","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","toUSAN":104,"value":"0x10","asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xbb9651c88974a0f79cde02e763f7999f0da36df751719934b40d04814b0640af"
}
```
***

#### fsntx_buildSendAssetTx

Build sendAsset unsigned raw transaction.

##### Parameters

1. `String|Address`, from - The address for the sending account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, asset - The hash of the asset.
6. `String|Address|Number`, to|toUSAN - The address for the receiving account | The notation of receiving account address.
7. `String|BigNumber`, value - The value for sending.


```js
1. params: [{
  "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "to":"0x07718f21f889b84451727ada8c65952a597b2e78",
  "value":"0x10",
  "asset":"0x4473c9c46aa575aca6665e0b485c5bb85eee55cb6161a002140c4829066f459b"
  }]
2. params:[{
  "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "toUSAN":"104",
  "value":"0x10",
  "asset":"0x4473c9c46aa575aca6665e0b485c5bb85eee55cb6161a002140c4829066f459b"
}]

```

##### Return

`DATA`,  unsigned raw transaction.

##### Example
```js
// Request
1. curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildSendAssetTx","params":[{"from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","to":"0x07718f21f889b84451727ada8c65952a597b2e78","value":"0x10","asset":"0x4473c9c46aa575aca6665e0b485c5bb85eee55cb6161a002140c4829066f459b"}],"id":1}'
2. curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildSendAssetTx","params":[{"from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","toUSAN":104,"value":"0x10","asset":"0x4473c9c46aa575aca6665e0b485c5bb85eee55cb6161a002140c4829066f459b"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "nonce":"0x1",
    "gasPrice":"0x3b9aca00",
    "gas":"0x15f90",
    "to":"0xffffffffffffffffffffffffffffffffffffffff",
    "value":"0x0",
    "input":"0xf83b02b838f7a04473c9c46aa575aca6665e0b485c5bb85eee55cb6161a002140c4829066f459b9407718f21f889b84451727ada8c65952a597b2e7810",
    "v":"0x0",
    "r":"0x0",
    "s":"0x0",
    "hash":"0x8826547721392cd3b9f1accb3c739e42a57e531c227346df598bd2424457a1b2"
    }
}
```

***

#### fsn_getAllBalances

Get all assets balances of account.

##### Parameters

1. `String|Address`, address - The address for the account.
2. `String|HexNumber|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.


```js
params: [
   "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
   "latest"  // state at the latest block
]
```

##### Return

`QUANTITY` - integer of the current balance in wei.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getAllBalances","params":["0x5b15a29274c74cd7cae59cabf656873a0ea706ac","latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff":"3499978048000000000"}}
```

***

#### fsn_getLatestNotation

Get the last Notation in the blockchain.


##### Parameters

1. `String|HexNumber|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.


```js
params: [
  "latest"  // state at the latest block
]
```

##### Return

`DATA` - the latest notation of the blockNumber.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getLatestNotation","params":["latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":104
}
```

***

#### fsntx_genNotation

Create a notation for a account. (Account has been unlocked)


##### Parameters

1. `String|Address`, from - The address for the account.
2. `Number` gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number` nonce - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.


```js
params: [{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac"}]
```

##### Return

`DATA`, 32 Bytes - The transaction hash.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_genNotation","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xae4b4949f9e2ad46343df4e11d5a245306281b60cb6d217dd10c011d18638445"
}
```
***
#### fsntx_buildGenNotationTx

Build genNotation unsigned raw transaction.


##### Parameters

1. `String|Address`, from - The address for the account.
2. `Number` gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number` nonce - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.


```js
params: [{"from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4"}]
```

##### Return

`DATA`,  unsigned raw transaction.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildGenNotationTx","params":[{"from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "nonce":"0x1",
    "gasPrice":"0x3b9aca00",
    "gas":"0x15f90",
    "to":"0xffffffffffffffffffffffffffffffffffffffff",
    "value":"0x0",
    "input":"0xc28080",
    "v":"0x0",
    "r":"0x0",
    "s":"0x0",
    "hash":"0xcfbe0320045fa877f9076e54bc1f7d64f0c85d2a6d20a36ba96059f4afe232b5"
    }
}
```

***

#### fsn_getNotation

Get notation of account.


##### Parameters

1. `String|Address` - The address for the account.
2. `String|HexNumber|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.

```js
params: [
   "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
   "latest"  // state at the latest block
]
```

##### Return

`Number` - the number of notation.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getNotation","params":["0x5b15a29274c74cd7cae59cabf656873a0ea706ac","latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":104
}
```

***

#### fsn_getAddressByNotation

Return the address of the notation.


##### Parameters

1. `Number`, notation - The number of the notation.
2. `String|HexNumber|TAG` - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`.

```js
params: [
   104,  //Notation
   "latest" // state at the latest block
]
```

##### Return

`DATA` - 20 bytes - the address of the notation.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getAddressByNotation","params":[104,"latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac"
}
```

***

#### fsn_getAsset

Get the asset information.


##### Parameters

1. `String|Hash`, assetID - The hash of the asset.
2. `String|HexNumber|TAG`, blockNr - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`.


```js
params: [
   "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff", //AssetId
   "latest"  // state at the latest block
]
```

##### Return

`DATA` - the detail of the asset.


##### Example
```js
// Request
 curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getAsset","params":["0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":
  {
    "ID":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "Owner":"0x0000000000000000000000000000000000000000",
    "Name":"Fusion",
    "Symbol":"FSN",
    "Decimals":18,
    "Total":"81920000000000000000000000",
    "CanChange":false,
    "Description":"https://fusion.org"
  }
}
```
***

#### fsn_getBalance

Get the balance of account.


##### Parameters

1. `String|Number`, assetID - The asset of the account.
2. `String|Address`, address - The address of the account.
3. `String|HexNumber|TAG`, blockNr - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`.


```js
params: [
   "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff", //AssetId
   "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
   "latest"  // state at the latest block
]
```

##### Return

`QUANTITY`  - integer of the current balance in wei.


##### Example
```js
// Request
  curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getBalance","params":["0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","0x5b15a29274c74cd7cae59cabf656873a0ea706ac","latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"5999978048000000000"
}
```

***

#### fsntx_makeSwap

Create a quantum swap order.(Account has been unlocked)


##### Parameters

1. `String|Address`, from - The address for the sending account.
2. `Number`, gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice- (optional) The price of gas for this transaction in wei.
4. `Number`, nonce - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Number`, FromAssetID - The assetID of the from account.
6. `String|HexNumber`, FromStartTime - (optional) The start time of the sending account.
7. `String|HexNumber`, FromEndTime - (optional) The end time of the sending account.
8. `String|BigNumber`, MinFromAmount - The amonut of the sending account.
9. `String|Number`,  ToAssetID - The assetID of the to account.
10. `String|HexNumber`,  ToStartTime - (optional) The start time of the receiving account.
11. `String|HexNumber`, ToEndTime - (optional) The end time of the receiving account..
12. `String|BigNumber`, MinToAmount - The amonut of the receiving account.
13. `Number`, SwapSize - The size of swap.
14. `Array String|Address`, Targes - Only these addresses can be exchanged.


```js
params: [
   {
    "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
    "FromAssetID":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff", //If FromAssetID is "0xfffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffe",it means make a Notation swap
    "ToAssetID":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "MinToAmount":"0x1BC16D674EC80000",    //2000000000000000000
    "MinFromAmount":"0x29A2241AF62C0000",  //3000000000000000000
    "FromStartTime":"0x5D6CE866",          //2019/9/2 18:1:10
    "FromEndTime":"0x5D9475B9",            //2019/10/2 18:2:33
    "SwapSize":2,
    "Targes":["0x07718f21f889b84451727ada8c65952a597b2e78"]
  }
]
```

##### Return

`DATA`, 32 Bytes - the transaction hash.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_makeSwap","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","FromAssetID":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","ToAssetID":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","MinToAmount":"0x1BC16D674EC80000","MinFromAmount":"0x29A2241AF62C0000","FromStartTime":"0x5D6CE866","FromEndTime":"0x5D9475B9","SwapSize":2,"Targes":["0x07718f21f889b84451727ada8c65952a597b2e78"]}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xfbd0eb501114d23aa42d92be50d705a3e987bda658772a92283e1483fcc6027a"
}
```
***
#### fsntx_buildMakeSwapTx

Build makeSwap unsigned raw transaction.


##### Parameters

1. `String|Address`, from - The address for the sending account.
2. `Number`, gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice- (optional) The price of gas for this transaction in wei.
4. `Number`, nonce - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Number`, FromAssetID - The assetID of the from account.
6. `String|HexNumber`, FromStartTime - (optional) The start time of the sending account.
7. `String|HexNumber`, FromEndTime - (optional) The end time of the sending account.
8. `String|BigNumber`, MinFromAmount - The amonut of the sending account.
9. `String|Number`,  ToAssetID - The assetID of the to account.
10. `String|HexNumber`,  ToStartTime - (optional) The start time of the receiving account.
11. `String|HexNumber`, ToEndTime - (optional) The end time of the receiving account..
12. `String|BigNumber`, MinToAmount - The amonut of the receiving account.
13. `Number`, SwapSize - The size of swap.
14. `Array String|Address`, Targes - Only these addresses can be exchanged.


```js
params: [
   {
    "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
    "FromAssetID":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff", //If FromAssetID is "0xfffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffe",it means make a Notation swap
    "ToAssetID":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "MinToAmount":"0x1BC16D674EC80000",    //2000000000000000000
    "MinFromAmount":"0x29A2241AF62C0000",  //3000000000000000000
    "FromStartTime":"0x5ee884db",          //2019/9/2 18:1:10
    "FromEndTime":"0x5f1011cc",            //2019/10/2 18:2:33
    "SwapSize":2,
    "Targes":["0xd1c675d3fc4c19d71b50bfe056a09627ca7e85a1"]
  }
]
```

##### Return

`DATA`,  unsigned raw transaction.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildMakeSwapTx","params":[{"from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","FromAssetID":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","ToAssetID":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","MinToAmount":"0x1BC16D674EC80000","MinFromAmount":"0x29A2241AF62C0000","FromStartTime":"0x5ee884db","FromEndTime":"0x5f1011cc","SwapSize":2,"Targes":["0xd1c675d3fc4c19d71b50bfe056a09627ca7e85a1"]}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "nonce":"0x1",
    "gasPrice":"0x3b9aca00",
    "gas":"0x15f90",
    "to":"0xffffffffffffffffffffffffffffffffffffffff",
    "value":"0x0",
    "input":"0xf88a0ab887f885a0ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff845ee884db845f1011cc8829a2241af62c0000a0ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff8088ffffffffffffffff881bc16d674ec8000002d594d1c675d3fc4c19d71b50bfe056a09627ca7e85a1845ee8943680",
    "v":"0x0",
    "r":"0x0",
    "s":"0x0",
    "hash":"0x1184284f0a299efbb8e4c99d24b629039a4aff041b790d5f3ef756ee55c0246c"
    }
}
```

***

#### fsn_getSwap

Get the details of MakeSwap.


##### Parameters

1. `String|Hash` - the hash of the MakeSwap transaction.
2. `String|HexNumber|TAG`, blockNr - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`.

```js
params: [
  "0x2bd4929f673c53bd4ff90ee640f8d3ccd6b756977893009ca6ef5561e54af535",
  "latest"
]
```

##### Return

`DATA` - the detail of the MakeSwap.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getSwap","params":["0x2bd4929f673c53bd4ff90ee640f8d3ccd6b756977893009ca6ef5561e54af535","latest"],"id":1}'

// Result
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "ID": "0xaac295a8b71c4698615d2ceb08e205355ad5f0bb622020fe1bac145e47a24141",
    "Owner": "0x3a1b3b81ed061581558a81f11d63e03129347437",
    "FromAssetID": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "FromStartTime": 1567418470,
    "FromEndTime": 1570010553,
    "MinFromAmount": 3e+18,
    "ToAssetID": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "ToStartTime": 0,
    "ToEndTime": 18446744073709552000,
    "MinToAmount": 2e+18,
    "SwapSize": 2,
    "Targes": [
      "0xb49edfcd6ab3dac4cc908f31fc4b3f7772773113"
    ],
    "Time": 1569381182,
    "Description": "",
    "Notation": 0
  }
}
```

***

#### fsntx_takeSwap

Take the quantum swap order.(Account has been unlocked)


##### Parameters

1. `String|Address`, The address for the sending account.
2. `Number`, gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, SwapID - The hash of the swap.
6. `Number`, Size - The size of swap.


```js
params: [{
  "from":"0x07718f21f889b84451727ada8c65952a597b2e78",
  "SwapID":"0x3be968fd7368b73d2cd8ccac12fedb0a9a3b85b8b86f10bdb69b4a8e3285dbab",
  "Size":1
  }]
```

##### Return

`DATA`, 32 Bytes - the transaction hash.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_takeSwap","params":[{"from":"0x07718f21f889b84451727ada8c65952a597b2e78","SwapID":"0x3be968fd7368b73d2cd8ccac12fedb0a9a3b85b8b86f10bdb69b4a8e3285dbab","Size":1}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x61923c6b6c5bf635130b5f2d093754a70fc7a9986cc2acc8c4fcfc87a580c25d"
}
```
***

#### fsntx_buildTakeSwapTx

Build takeSwap unsigned raw transaction.


##### Parameters

1. `String|Address`, The address for the sending account.
2. `Number`, gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, SwapID - The hash of the swap.
6. `Number`, Size - The size of swap.


```js
params: [{
  "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "SwapID":"0x5aac73c44f6bd8a0908a283c553bccd35e70452ea176b5001a0401bd943ef334",
  "Size":1
  }]
```

##### Return

`DATA`,  unsigned raw transaction.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildTakeSwapTx","params":[{"from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","SwapID":"0x5aac73c44f6bd8a0908a283c553bccd35e70452ea176b5001a0401bd943ef334","Size":1}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "nonce":"0x2",
    "gasPrice":"0x3b9aca00",
    "gas":"0x15f90",
    "to":"0xffffffffffffffffffffffffffffffffffffffff",
    "value":"0x0",
    "input":"0xe50ba3e2a05aac73c44f6bd8a0908a283c553bccd35e70452ea176b5001a0401bd943ef33401",
    "v":"0x0",
    "r":"0x0",
    "s":"0x0",
    "hash":"0x857cc1bc6b23ec12420e17848a50d4395c1fcaeed6ff98298f85f475d9a82062"
    }
}
```

***

#### fsntx_recallSwap

Destroy Quantum swap order and Return Assets.(Account has been unlocked)

##### Parameters

1. `String|Address`, from - The address for the account.
2. `Number`, gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, SwapID - The hash of the swap.


```js
params: [{
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "SwapID":"0x3be968fd7368b73d2cd8ccac12fedb0a9a3b85b8b86f10bdb69b4a8e3285dbab"
  }]
```

##### Return

`DATA`, 32 Bytes - the transaction hash.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_recallSwap","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","SwapID":"0x3be968fd7368b73d2cd8ccac12fedb0a9a3b85b8b86f10bdb69b4a8e3285dbab"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x4270a81d7377fffb7e4aa4785f729baab5ca6711b9ae6df8777916a6f036285d"
}
```
***
#### fsntx_buildRecallSwapTx

Build recallSwap unsigned raw transaction.

##### Parameters

1. `String|Address`, from - The address for the account.
2. `Number`, gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, SwapID - The hash of the swap.


```js
params: [{
  "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "SwapID":"0x5aac73c44f6bd8a0908a283c553bccd35e70452ea176b5001a0401bd943ef334"
  }]
```

##### Return

`DATA`,  unsigned raw transaction.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildRecallSwapTx","params":[{"from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","SwapID":"0x5aac73c44f6bd8a0908a283c553bccd35e70452ea176b5001a0401bd943ef334"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "nonce":"0x2",
    "gasPrice":"0x3b9aca00",
    "gas":"0x15f90",
    "to":"0xffffffffffffffffffffffffffffffffffffffff",
    "value":"0x0",
    "input":"0xe407a2e1a05aac73c44f6bd8a0908a283c553bccd35e70452ea176b5001a0401bd943ef334",
    "v":"0x0",
    "r":"0x0",
    "s":"0x0",
    "hash":"0x25fe3ca136e91ba0448f775e36360348a4f09cf176afca8ffab2209515003e31"
    }
}
```

***
#### fsntx_makeMultiSwap

Create a multi swap order.(Account has been unlocked)


##### Parameters

1. `String|Address`, from - The address for the sending account.
2. `Number`, gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Number`, FromAssetID - The assetID of the from account.
6. `String|HexNumber`, FromStartTime - (optional) The start time of the sending account.
7. `String|HexNumber`, FromEndTime - (optional) The end time of the sending account.
8. `String|BigNumber`, MinFromAmount - The amonut of the sending account.
9. `String|Number`,  ToAssetID - The assetID of the to account.
10. `String|HexNumber`,  ToStartTime - (optional) The start time of the receiving account.
11. `String|HexNumber`, ToEndTime - (optional) The end time of the receiving account..
12. `String|BigNumber`, MinToAmount - The amonut of the receiving account.
13. `Number`, SwapSize - The size of swap.
14. `Array String|Address`, Targes - Only these addresses can be exchanged.


```js
params: [
  {
    "from":"0x3a1b3b81ed061581558a81f11d63e03129347437",
    "FromAssetID":[
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"],
      "ToAssetID":[
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"],
      "MinToAmount":["0x1BC16D674EC80000","0x1BC16D674EC80000"],
      "MinFromAmount":["0x29A2241AF62C0000","0x29A2241AF62C0000"],
      "FromStartTime":["0x5D6CE866","0x5D6CE866"],
      "FromEndTime":["0x5D9475B9","0x5D9475B9"],
      "SwapSize":2,
      "Targes":[]
  }
]
```

##### Return

`DATA`, 32 Bytes - the transaction hash.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_makeMultiSwap","params":[{"from":"0x3a1b3b81ed061581558a81f11d63e03129347437","FromAssetID":["0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"],"ToAssetID":["0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"],"MinToAmount":["0x1BC16D674EC80000","0x1BC16D674EC80000"],"MinFromAmount":["0x29A2241AF62C0000","0x29A2241AF62C0000"],"FromStartTime":["0x5D6CE866","0x5D6CE866"],"FromEndTime":["0x5D9475B9","0x5D9475B9"],"SwapSize":2,"Targes":[]}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x89e699deac578e491675f1e4fe2233966e79fd388f9725035479268605801c87"
}
```
***
#### fsntx_buildMakeMultiSwapTx

Build makeMultiSwap unsigned raw transaction.


##### Parameters

1. `String|Address`, from - The address for the sending account.
2. `Number`, gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Number`, FromAssetID - The assetID of the from account.
6. `String|HexNumber`, FromStartTime - (optional) The start time of the sending account.
7. `String|HexNumber`, FromEndTime - (optional) The end time of the sending account.
8. `String|BigNumber`, MinFromAmount - The amonut of the sending account.
9. `String|Number`,  ToAssetID - The assetID of the to account.
10. `String|HexNumber`,  ToStartTime - (optional) The start time of the receiving account.
11. `String|HexNumber`, ToEndTime - (optional) The end time of the receiving account..
12. `String|BigNumber`, MinToAmount - The amonut of the receiving account.
13. `Number`, SwapSize - The size of swap.
14. `Array String|Address`, Targes - Only these addresses can be exchanged.


```js
params: [
  {
    "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
    "FromAssetID":[
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"],
      "ToAssetID":[
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"],
      "MinToAmount":["0x1BC16D674EC80000","0x1BC16D674EC80000"],
      "MinFromAmount":["0x29A2241AF62C0000","0x29A2241AF62C0000"],
      "FromStartTime":["0x5ee884db","0x5ee884db"],
      "FromEndTime":["0x5f1011cc","0x5f1011ff"],
      "SwapSize":2,
      "Targes":[]
  }
]
```

##### Return

`DATA`,  unsigned raw transaction.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildMakeMultiSwapTx","params":[{"from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","FromAssetID":["0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"],"ToAssetID":["0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"],"MinToAmount":["0x1BC16D674EC80000","0x1BC16D674EC80000"],"MinFromAmount":["0x29A2241AF62C0000","0x29A2241AF62C0000"],"FromStartTime":["0x5ee884db","0x5ee884db"],"FromEndTime":["0x5f1011cc","0x5f1011ff"],"SwapSize":2,"Targes":[]}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "nonce":"0x2",
    "gasPrice":"0x3b9aca00",
    "gas":"0x15f90",
    "to":"0xffffffffffffffffffffffffffffffffffffffff",
    "value":"0x0",
    "input":"0xf8e70db8e4f8e2f842a0ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffa0ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffca845ee884db845ee884dbca845f1011cc845f1011ffd28829a2241af62c00008829a2241af62c0000f842a0ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffa0ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffc28080d288ffffffffffffffff88ffffffffffffffffd2881bc16d674ec80000881bc16d674ec8000002c0845ee8970e80",
    "v":"0x0",
    "r":"0x0",
    "s":"0x0",
    "hash":"0xe3070d1240181df70ae3665cbfe00b743529aa24e1c5faa04be3da898ad97452"
    }
}
```

***

#### fsn_getMultiSwap

Get the details of the multi MakeSwap.


##### Parameters

1. `String|Hash` - the hash of the Multi MakeSwap transaction.
2. `String|HexNumber|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.

```js
params: [
  "0xf616d50440414ce2bfd2204ace993a34e53e58cc656c82b339be935670f2070e",
  "latest"
  ]
```

##### Return

`DATA`, the detail of the multi MakeSwap.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getMultiSwap","params":["0xf616d50440414ce2bfd2204ace993a34e53e58cc656c82b339be935670f2070e","latest"],"id":1}'

// Result
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "ID": "0xac05ff3d0d7ad5440eba0ee751ba19f876105b790dec63051c4db0b434cefa47",
    "Owner": "0x3a1b3b81ed061581558a81f11d63e03129347437",
    "FromAssetID": [
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
    ],
    "FromStartTime": [
      1567418470,
      1567418470
    ],
    "FromEndTime": [
      1570010553,
      1570010553
    ],
    "MinFromAmount": [
      3e+18,
      3e+18
    ],
    "ToAssetID": [
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
    ],
    "ToStartTime": [
      0,
      0
    ],
    "ToEndTime": [
      18446744073709552000,
      18446744073709552000
    ],
    "MinToAmount": [
      2e+18,
      2e+18
    ],
    "SwapSize": 2,
    "Targes": [],
    "Time": 1569387880,
    "Description": "",
    "Notation": 0
  }
}
```

***

#### fsntx_takeMultiSwap

Take the multi swap order.(Account has been unlocked)


##### Parameters

1. `String|Address`, from - The address for the sending account.
2. `Number`, gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, SwapID - The hash of the multi swap.
6. `Number`, Size - The size of multi swap.


```js
params: [{
  "from":"0xb49edfcd6ab3dac4cc908f31fc4b3f7772773113",
  "SwapID":"0xac05ff3d0d7ad5440eba0ee751ba19f876105b790dec63051c4db0b434cefa47",
  "Size":1
  }]
```

##### Return

`DATA`, 32 Bytes - the transaction hash.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_takeMultiSwap","params":[{"from":"0xb49edfcd6ab3dac4cc908f31fc4b3f7772773113","SwapID":"0xac05ff3d0d7ad5440eba0ee751ba19f876105b790dec63051c4db0b434cefa47","Size":1}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x75ebbd003ae590e1df879ebc931d34c6c494f9479b828017baecf60b44e2c59c"
}
```
***
#### fsntx_buildTakeMultiSwapTx

Build takeMultiSwap unsigned raw transaction.


##### Parameters

1. `String|Address`, from - The address for the sending account.
2. `Number`, gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Hash`, SwapID - The hash of the multi swap.
6. `Number`, Size - The size of multi swap.


```js
params: [{
  "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "SwapID":"0x18f7b73f2321acafcfb3aec2c24a4bfcb6af59f8a0137a565af5fe378bb6f1f5",
  "Size":1
  }]
```

##### Return

`DATA`,  unsigned raw transaction.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildTakeMultiSwapTx","params":[{"from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","SwapID":"0x18f7b73f2321acafcfb3aec2c24a4bfcb6af59f8a0137a565af5fe378bb6f1f5","Size":1}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "nonce":"0x3",
    "gasPrice":"0x3b9aca00",
    "gas":"0x15f90",
    "to":"0xffffffffffffffffffffffffffffffffffffffff",
    "value":"0x0",
    "input":"0xe50fa3e2a018f7b73f2321acafcfb3aec2c24a4bfcb6af59f8a0137a565af5fe378bb6f1f501",
    "v":"0x0",
    "r":"0x0",
    "s":"0x0",
    "hash":"0x21fb72ada22013431bd42bfc610330074f9352a5999528a78fac748cec5c2e99"
    }
}
```

***

#### fsntx_recallMultiSwap

Destroy multi swap order and Return Assets.(Account has been unlocked)

##### Parameters

1. `String|Address`, from - The address for the account.
2. `Number`, gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Number`, SwapID - The hash of multi swap.


```js
params: [{
  "from":"0x3a1b3b81ed061581558a81f11d63e03129347437",
  "SwapID":"0xac05ff3d0d7ad5440eba0ee751ba19f876105b790dec63051c4db0b434cefa47"
  }]
```

##### Return

`DATA`, 32 Bytes - the transaction hash.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_recallMultiSwap","params":[{"from":"0x3a1b3b81ed061581558a81f11d63e03129347437","SwapID":"0xac05ff3d0d7ad5440eba0ee751ba19f876105b790dec63051c4db0b434cefa47"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xe2dd257d22e2717f9591d207d18e493acb5aae5d19dc94a3f95ded7227f7c825"
}
```

***
#### fsntx_isAutoBuyTicket

Return `true` if the ticket is purchased automatically.(Account has been unlocked)

##### Parameters

`String|HexNumber|TAG`, blockNr - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`.

```js
params: ["latest"]
```

##### Return

`bool`, If the ticket is purchased automatically, return true.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_isAutoBuyTicket","params":["latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":true
}

```
***
#### fsntx_buildRecallMultiSwapTx

Build recallMultiSwap unsigned raw transaction.

##### Parameters

1. `String|Address`, from - The address for the account.
2. `Number`, gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `String|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Number`, SwapID - The hash of multi swap.


```js
params: [{
  "from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4",
  "SwapID":"0x18f7b73f2321acafcfb3aec2c24a4bfcb6af59f8a0137a565af5fe378bb6f1f5"
  }]
```

##### Return

`DATA`,  unsigned raw transaction.


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buildRecallMultiSwapTx","params":[{"from":"0xcc9ea1c564fa513d6abd9339587dc4f886d7acc4","SwapID":"0x18f7b73f2321acafcfb3aec2c24a4bfcb6af59f8a0137a565af5fe378bb6f1f5"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "nonce":"0x3",
    "gasPrice":"0x3b9aca00",
    "gas":"0x15f90",
    "to":"0xffffffffffffffffffffffffffffffffffffffff",
    "value":"0x0",
    "input":"0xe40ea2e1a018f7b73f2321acafcfb3aec2c24a4bfcb6af59f8a0137a565af5fe378bb6f1f5",
    "v":"0x0",
    "r":"0x0",
    "s":"0x0",
    "hash":"0x9822e3274a33296084e2699509fcc7738a51a219129afc08b355c8020da4c80e"
    }
}
```

***
#### miner_startAutoBuyTicket

Start buying tickets automatically.(Account has been unlocked)

##### Parameters

none

##### Return

`String`, Return null.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"miner_startAutoBuyTicket","params":[],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":null
}

```

***
#### miner_stopAutoBuyTicket

Stop buying tickets automatically.(Account has been unlocked)

##### Parameters

none

##### Return

`String`, Return null.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"miner_stopAutoBuyTicket","params":[],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":null
}

```

***
#### fsn_getTransactionAndReceipt

Return the tx and receipt of the transaction.

##### Parameters

`DATA`, 32 Bytes - the hash of the transaction.

```js
params: ["0x8700056ef2896b47760e661902b21d8f294a80bff87c7e4108d7bbd5bce4ce6d"]
```

##### Return

`DATA` - the tx and receipt of the transaction.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getTransactionAndReceipt","params":["0x8700056ef2896b47760e661902b21d8f294a80bff87c7e4108d7bbd5bce4ce6d"],"id":1}'

// Result
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "fsnTxInput": {
      "FuncType": "SendAssetFunc",
      "FuncParam": {
        "AssetID": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "To": "0x37a200388caa75edcc53a2bd329f7e9563c6acb6",
        "Value": 1e+18
      }
    },
    "tx": {
      "blockHash": "0xd8d4b5f054cb398b1f0b5bb5d4add5e80d10a432f2c15226f620609577536b6b",
      "blockNumber": "0xad1a5",
      "from": "0x0122bf3930c1201a21133937ad5c83eb4ded1b08",
      "gas": "0x15f90",
      "gasPrice": "0x3b9aca00",
      "hash": "0x8700056ef2896b47760e661902b21d8f294a80bff87c7e4108d7bbd5bce4ce6d",
      "input": "0xf84402b841f83fa0ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff9437a200388caa75edcc53a2bd329f7e9563c6acb6880de0b6b3a7640000",
      "nonce": "0xa7cc",
      "to": "0xffffffffffffffffffffffffffffffffffffffff",
      "transactionIndex": "0x2",
      "value": "0x0",
      "v": "0x16ce3",
      "r": "0x8244e44f720023b240faafab08bb401b1b3167087f2882fa6b8f4fc87b59bdfc",
      "s": "0x277635df431668f4a8b8c8b0702077634dd404db9a1139539d3c276651d3d1ce"
    },
    "receipt": {
      "blockHash": "0xd8d4b5f054cb398b1f0b5bb5d4add5e80d10a432f2c15226f620609577536b6b",
      "blockNumber": "0xad1a5",
      "contractAddress": null,
      "cumulativeGasUsed": "0x10f60",
      "from": "0x0122bf3930c1201a21133937ad5c83eb4ded1b08",
      "fsnLogData": {
        "AssetID": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "To": "0x37a200388caa75edcc53a2bd329f7e9563c6acb6",
        "Value": 1e+18
      },
      "fsnLogTopic": "SendAssetFunc",
      "gasUsed": "0x63e0",
      "logs": [
        {
          "address": "0xffffffffffffffffffffffffffffffffffffffff",
          "topics": [
            "0x0000000000000000000000000000000000000000000000000000000000000002"
          ],
          "data": "0x7b2241737365744944223a22307866666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666222c22546f223a22307833376132303033383863616137356564636335336132626433323966376539353633633661636236222c2256616c7565223a313030303030303030303030303030303030307d",
          "blockNumber": "0xad1a5",
          "transactionHash": "0x8700056ef2896b47760e661902b21d8f294a80bff87c7e4108d7bbd5bce4ce6d",
          "transactionIndex": "0x2",
          "blockHash": "0xd8d4b5f054cb398b1f0b5bb5d4add5e80d10a432f2c15226f620609577536b6b",
          "logIndex": "0x2",
          "removed": false
        }
      ],
      "logsBloom": "0x04000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000080000000000000002000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000008000000000000000000000",
      "status": "0x1",
      "to": "0xffffffffffffffffffffffffffffffffffffffff",
      "transactionHash": "0x8700056ef2896b47760e661902b21d8f294a80bff87c7e4108d7bbd5bce4ce6d",
      "transactionIndex": "0x2"
    },
    "receiptFound": true
  }
}
```

***
#### fsn_allInfoByAddress

Returns all information about the address.

##### Parameters

1. `String|Address`, 20 Bytes - the address to be queried.
2. `String|HexNumber|TAG`, blockNr - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`.

```js
params: ["0x3a1b3b81ed061581558a81f11d63e03129347437","latest"]
```
##### Return

`DATA` - All information of the address.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_allInfoByAddress","params":["0x3a1b3b81ed061581558a81f11d63e03129347437","latest"],"id":1}'

// Result
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "tickets": {
      "0x8fd87180c90cc7133be48b83b03781810c8ae4ad86446870dbed6b1a9a3193a8": {
        "Owner": "0x3a1b3b81ed061581558a81f11d63e03129347437",
        "Height": 251,
        "StartTime": 1569381648,
        "ExpireTime": 1571973648,
        "Value": 5e+21
      },
      "0xf9972189b9c077207cc05ab81d2e35bfe4ce6db28683d68c9ebfff6ff640722e": {
        "Owner": "0x3a1b3b81ed061581558a81f11d63e03129347437",
        "Height": 255,
        "StartTime": 1569381700,
        "ExpireTime": 1571973700,
        "Value": 5e+21
      }
    },
    "balances": {
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff": "98980637500000000000000000"
    },
    "timeLockBalances": {
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff": {
        "Items": [
          {
            "StartTime": 1569381700,
            "EndTime": 18446744073709552000,
            "Value": "9994000000000000000000"
          },
          {
            "StartTime": 1571973649,
            "EndTime": 18446744073709552000,
            "Value": "5000000000000000000000"
          },
          {
            "StartTime": 1571973701,
            "EndTime": 18446744073709552000,
            "Value": "5000000000000000000000"
          },
          {
            "StartTime": 1570010554,
            "EndTime": 18446744073709552000,
            "Value": "6000000000000000000"
          }
        ]
      }
    },
    "notation": 0
  }
}
```

***
