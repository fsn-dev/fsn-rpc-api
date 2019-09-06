# FUSION JSON RPC API

[JSON](http://json.org/) is a lightweight data-interchange format. It can represent numbers, strings, ordered sequences of values, and collections of name/value pairs.

[JSON-RPC](http://www.jsonrpc.org/specification) is a stateless, light-weight remote procedure call (RPC) protocol. Primarily this specification defines several data structures and the rules around their processing. It is transport agnostic in that the concepts can be used within the same process, over sockets, over HTTP, or in many various message passing environments. It uses JSON ([RFC 4627](http://www.ietf.org/rfc/rfc4627.txt)) as data format.


## JavaScript API

To talk to an ethereum node from inside a JavaScript application use the [web3.js](https://github.com/ethereum/web3.js) library, which gives a convenient interface for the RPC methods.
See the [JavaScript API](https://fusionapi.readthedocs.io/) for more.

## JSON-RPC Endpoint

Online RPC service:

Testnet: https://testnet.fsn.dev/api

Mainnet: https://fsn.dev/api

Default JSON-RPC endpoints:

| Client | URL |
|-------|:------------:|
| Go |http://localhost:32659 | 


You can start the HTTP JSON-RPC with the `--rpc` flag
```bash
efsn --rpc
```

change the default port (32659) and listing address (localhost) with:

```bash
efsn --rpc --rpcaddr <ip> --rpcport <portnumber>
```

If accessing the RPC from a browser, CORS will need to be enabled with the appropriate domain set. Otherwise, JavaScript calls are limit by the same-origin policy and requests will fail:

```bash
efsn --rpc --rpccorsdomain "http://localhost:8111"
```

The JSON RPC can also be started from the efsn console using the `admin.startRPC(addr, port)` command.

## Curl Examples Explained

The curl options below might return a response where the node complains about the content type, this is because the --data option sets the content type to application/x-www-form-urlencoded . If your node does complain, manually set the header by placing -H "Content-Type: application/json" at the start of the call.

The examples also do not include the URL/IP & port combination which must be the last argument given to curl e.x. 127.0.0.1:32659

## JSON-RPC methods

* [fsntx_buyTicket](#fsntx_buyTicket)
* [fsn_allTickets](#fsn_allTickets)
* [fsn_allTicketsByAddress](#fsn_allTicketsByAddress)
* [fsn_totalNumberOfTickets](#fsn_totalNumberOfTickets)
* [fsn_totalNumberOfTicketsByAddress](#fsn_totalNumberOfTicketsByAddress)
* [fsntx_assetToTimeLock](#fsntx_assetToTimeLock)
* [fsntx_timeLockToTimeLock](#fsntx_timeLockToTimeLock)
* [fsntx_timeLockToAsset](#fsntx_timeLockToAsset)
* [fsn_getTimeLockBalance](#fsn_getTimeLockBalance)
* [fsn_getAllTimeLockBalances](#fsn_getAllTimeLockBalances)
* [fsntx_genAsset](#fsntx_genAsset)
* [fsntx_decAsset](#fsntx_decAsset)
* [fsntx_incAsset](#fsntx_incAsset)
* [fsntx_sendAsset](#fsntx_sendAsset)
* [fsn_getAllBalances](#fsn_getAllBalances)
* [fsntx_genNotation](#fsntx_genNotation)
* [fsn_getNotation](#fsn_getNotation)
* [fsn_getAddressByNotation](#fsn_getAddressByNotation)
* [fsn_getAsset](#fsn_getAsset)
* [fsn_getBalance](#fsn_getBalance)
* [fsntx_makeSwap](#fsntx_makeSwap)
* [fsntx_takeSwap](#fsntx_takeSwap)
* [fsntx_recallSwap](#fsntx_recallSwap)
* [fsntx_isAutoBuyTicket](#fsntx_isAutoBuyTicket)
* [fsntx_startAutoBuyTicket](#fsntx_startAutoBuyTicket)
* [fsntx_stopAutoBuyTicket](#fsntx_stopAutoBuyTicket)

## JSON RPC API Reference

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

#### fsn_allTickets

Return all tickets to all accounts.

##### Parameters

`QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.

```js
params: [
  "latest"  // state at the latest block
]
```

##### Return

`JSON` - All tickets.

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
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.

```js
params: [
  "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "latest"  // state at the latest block
]
```

##### Return

`JSON` - All tickets of the address.

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

`QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.

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

1. `DATA`, 20 Bytes - The address for the account.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.

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

#### fsntx_assetToTimeLock

Convert assets to timelock balance.(Account has been unlocked)

##### Parameters

1. `String|Number`, from - The address for the sending account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `Number|String|BN|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Number`, asset -  The hash of the asset.
6. `String|Number`, to - The address for the receiving account.
7. `Number|String|BN|BigNumber`, value -  The value for sending.
8. `Number`, start - (optional) The start time of the time lock.
9. `Number`, end - (optional) The end time of the time lock.


```js
params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
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
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_assetToTimeLock","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","start":"0x5D6CD2FC","end":"0x5D945FFC","value":"0x1BC16D674EC80000"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xa5c08546790e19974fbbb554878289147e439fff9f57e12cb40e63d1e3ea8096"
}

```

***

#### fsntx_timeLockToTimeLock

Send a time locked asset to another account.(Account has been unlocked)

##### Parameters

1. `String|Number`, from - The address for the account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `Number|String|BN|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Number`, asset -  The hash of the asset.
6. `String|Number`, to -  The address for the receiving account.
7. `Number|String|BN|BigNumber`, value -  The value for sending. 
8. `Number`, start -  The start time of the time lock.
9. `Number`, end - The end time of the time lock.


```js
params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "to":"0x07718f21f889b84451727ada8c65952a597b2e78",
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
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_timeLockToTimeLock","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","to":"0x07718f21f889b84451727ada8c65952a597b2e78","start":"0x5D6CD2FC","end":"0x5D945FFC","value":"0x1BC16D674EC80000"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x7ca4fa378a8ece6f2db1fe41521c80edcda45fff270b0bc12c81ab269c651d27"
}
```

***

#### fsntx_timeLockToAsset

Convert timelock balance to asset.(Account has been unlocked)


##### Parameters

1. `String|Number`, from - The address for the account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `Number|String|BN|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Number`, asset -  The hash of the asset.
6. `String|Number`, to -  The address for the receiving account.
7. `Number|String|BN|BigNumber`, value -  The value for sending. 
8. `Number`, start - (optional) The start time of the time lock.
9. `Number`, end - (optional) The end time of the time lock.

```js
params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
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
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_timeLockToAsset","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","start":"0x5D6CD2FC","end":"0x5D945FFC","value":"0x1BC16D674EC80000"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xb8242235c1f4fa1bcbc22b65bdfd2ab19db97cdfdc3b804fa302f87eafcd69c2"
}
```

***

#### fsn_getTimeLockBalance

Return time locked balance of assetID.


##### Parameters

1. `String|Number`, assetID - The hash of the asset.
2. `String|Number`, address - The address for the account.
3. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.


```js
params: [{
  "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "latest"  // state at the latest block
  }]
```

##### Return

`JSON`, The detail of the TimeLock balance.

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
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.

```js
params: [{
  "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "latest"  // state at the latest block
  }]
```

##### Return

`JSON`, The detail of the TimeLock balance.

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

1. `String|Number`, from - The address for the account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `Number|String|BN|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String`, name - The name for the asset.
6. `String`, symbol -  The symbol for the asset.
7. `Number`, decimals -  The decimals for the asset.
8. `Number|String|BN|BigNumber`, total -  The total value for the asset.
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

#### fsntx_decAsset

Reduction the total amount of asset.(Account has been unlocked && "canChange" is true for the asset)

##### Parameters

1. `String|Number`, from - The address for the account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `Number|String|BN|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Number`, asset - The hash of the asset.
6. `String|Number`, to - The address for the receiving account.
7. `Number|String|BN|BigNumber`, value - The value of decrease.
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

#### fsntx_incAsset

Increase the total amount of asset.(Account has been unlocked && "canChange" is true for the asset)


##### Parameters

1. `String|Number`, from - The address for the account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `Number|String|BN|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Number`, asset - The hash of the asset.
6. `String|Number`, to - The address for the receiving account.
7. `Number|String|BN|BigNumber`, value - The value of increase.
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

#### fsntx_sendAsset

Send assets to other accounts.(Account has been unlocked)

##### Parameters

1. `String|Number`, from - The address for the sending account.
2. `Number`, gas -  (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `Number|String|BN|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce -  (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Number`, asset - The hash of the asset.
6. `String|Number`, to - The address for the receiving account.
7. `Number|String|BN|BigNumber`, value - The value for sending.


```js
params: [{
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "to":"0x07718f21f889b84451727ada8c65952a597b2e78",
  "value":"0x10",
  "asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"
  }]
```

##### Return

`DATA`, 32 Bytes - The transaction hash.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_sendAsset","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","to":"0x07718f21f889b84451727ada8c65952a597b2e78","value":"0x10","asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xbb9651c88974a0f79cde02e763f7999f0da36df751719934b40d04814b0640af"
}
```

***

#### fsn_getAllBalances

Get all assets balances of account.

##### Parameters

1. `String|Number`, address - The address for the account.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.


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

#### fsntx_genNotation

Create a notation for a account.


##### Parameters

1. `String|Number`, from - The address for the account.
2. `Number` gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `Number|String|BN|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
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

#### fsn_getNotation

Get notation of account.


##### Parameters

1. `DATA`, 20 Bytes - The address for the account.
2. `QUANTITY|TAG` - integer block number, or the string `"latest"`, `"earliest"` or `"pending"`.

```js
params: [
   "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
   "latest"  // state at the latest block
]
```

##### Return

`number` - the number of notation.


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
2. `QUANTITY|TAG` - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](#the-default-block-parameter).

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

1. `String|Number`, assetID - The hash of the asset.
2. `QUANTITY|TAG`, blockNr - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](#the-default-block-parameter).


```js
params: [
   "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff", //AssetId
   "latest"  // state at the latest block
]
```

##### Return

`JSON` - the detail of the asset.


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
2. `String|Number`, address - The address of the account.
3. `QUANTITY|TAG`, blockNr - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](#the-default-block-parameter).


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

1. `String|Number`, from- The address for the sending account.
2. `Number`, gas- (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `Number|String|BN|BigNumber`, gasPrice- (optional) The price of gas for this transaction in wei.
4. `Number`, nonce-(optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Number`, FromAssetID - The assetID of the from account.
6. `Number`, FromStartTime - The start time of the sending account.
7. `Number`, FromEndTime - The end time of the sending account.
8. `Number|String|BN|BigNumber`, MinFromAmount - The amonut of the sending account.
9. `String|Number`,  ToAssetID - The assetID of the to account.
10. `Number`,  ToStartTime - The start time of the receiving account.
11. `Number`, ToEndTime - The end time of the receiving account..
12. `Number|String|BN|BigNumber`, MinToAmount - The amonut of the receiving account.
13. `Number`, SwapSize - The size of swap.
14. `Array String|Number`, Targes - Only these addresses can be exchanged.


```js
params: [
   {
    "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
    "FromAssetID":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
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

#### fsntx_takeSwap

Take the quantum swap order.(Account has been unlocked)


##### Parameters

1. `String|Number`, The address for the sending account.
2. `Number`, gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `Number|String|BN|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Number`, SwapID - The hash of the swap.
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
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_takeSwap","params":[{"from":"0x07718f21f889b84451727ada8c65952a597b2e78","SwapID":"0x3be968fd7368b73d2cd8ccac12fedb0a9a3b85b8b86f10bdb69b4a8e3285dbab","Size":1}],"id":1}' http://10.192.33.196:8133

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x61923c6b6c5bf635130b5f2d093754a70fc7a9986cc2acc8c4fcfc87a580c25d"
}
```

***


#### fsntx_recallSwap

Destroy Quantum swap order and Return Assets.(Account has been unlocked)

##### Parameters

1. `String|Number`, from - The address for the account.
2. `Number`, gas - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
3. `Number|String|BN|BigNumber`, gasPrice - (optional) The price of gas for this transaction in wei.
4. `Number`, nonce - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
5. `String|Number`, SwapID - The size of swap.


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
#### fsntx_isAutoBuyTicket

Return `true` if the ticket is purchased automatically.(Account has been unlocked)

##### Parameters

`QUANTITY|TAG`, blockNr - integer of a block number, or the string `"earliest"`, `"latest"` or `"pending"`, as in the [default block parameter](#the-default-block-parameter).

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
#### fsntx_startAutoBuyTicket

Start buying tickets automatically.(Account has been unlocked)

##### Parameters

none

##### Return

`String`, Return null.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_startAutoBuyTicket","params":[],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":null
}

```

***
#### fsntx_stopAutoBuyTicket

Stop buying tickets automatically.(Account has been unlocked)

##### Parameters

none

##### Return

`String`, Return null.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_stopAutoBuyTicket","params":[],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":null
}

```

***
