# Etherscan API Documentation

_Scraped from docs.etherscan.io_

_Total pages: 97_

---

# Get Address ERC20 Token Holding

**Source:** https://docs.etherscan.io/api-reference/endpoint/addresstokenbalance

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

This [historical endpoint](/resources/rate-limits) is throttled to 2 calls/s

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint.

[​

](#param-action)

action

string

default:"addresstokenbalance"

Set to `addresstokenbalance` for this endpoint.

[​

](#param-address)

address

string

default:"0x983e3660c0bE01991785F80f266A84B911ab59b0"

Address to check for token holdings.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"100"

Number of records per page.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=account&action=addresstokenbalance&address=0x983e3660c0bE01991785F80f266A84B911ab59b0&page=1&offset=100&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "TokenAddress": "0xffffffff2ba8f66d4e51811c5190992176930278",
      "TokenName": "Furucombo",
      "TokenSymbol": "COMBO",
      "TokenQuantity": "1861606940000000000",
      "TokenDivisor": "18",
      "TokenPriceUSD": "0.000891470000000000"
    },
    {
      "TokenAddress": "0x53a1e9912323b8016424d6287286e3b6de263f76",
      "TokenName": "PUTIN Token",
      "TokenSymbol": "PTT",
      "TokenQuantity": "3500000000000000000000",
      "TokenDivisor": "18",
      "TokenPriceUSD": "0.000000000000000000"
    }
  ]
}

```

---

# Get Address ERC721 Token Holding

**Source:** https://docs.etherscan.io/api-reference/endpoint/addresstokennftbalance

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

This [historical endpoint](/resources/rate-limits) is throttled to 2 calls/s

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint.

[​

](#param-action)

action

string

default:"addresstokennftbalance"

Set to `addresstokennftbalance` for this endpoint.

[​

](#param-address)

address

string

default:"0x6b52e83941eb10f9c613c395a834457559a80114"

Address to check for NFT holdings.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"100"

Number of records per page.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=account&action=addresstokennftbalance&address=0x6b52e83941eb10f9c613c395a834457559a80114&page=1&offset=100&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "0",
  "message": "NOTOK",
  "result": "Maximum rate limit reached"
}

```

---

# Get Address ERC721 Token Inventory by Contract

**Source:** https://docs.etherscan.io/api-reference/endpoint/addresstokennftinventory

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

This [historical endpoint](/resources/rate-limits) is throttled to 2 calls/s

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint.

[​

](#param-action)

action

string

default:"addresstokennftinventory"

Set to `addresstokennftinventory` for this endpoint.

[​

](#param-address)

address

string

default:"0x123432244443b54409430979df8333f9308a6040"

Address to check for inventory.

[​

](#param-contractaddress)

contractaddress

string

default:"0xed5af388653567af2f388e6224dc7c4b3241c544"

ERC-721 token contract address to filter by.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"100"

Number of records per page. Limited to 1000 records per query; use the `page` parameter for subsequent records.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=account&action=addresstokennftinventory&address=0x123432244443b54409430979df8333f9308a6040&contractaddress=0xed5af388653567af2f388e6224dc7c4b3241c544&page=1&offset=100&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "TokenAddress": "0xed5af388653567af2f388e6224dc7c4b3241c544",
      "TokenId": "453"
    },
    {
      "TokenAddress": "0xed5af388653567af2f388e6224dc7c4b3241c544",
      "TokenId": "8160"
    }
  ]
}

```

---

# Get Native Balance for an Address

**Source:** https://docs.etherscan.io/api-reference/endpoint/balance

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint.

[​

](#param-action)

action

string

default:"balance"

Set to `balance` for this endpoint.

[​

](#param-address)

address

string

default:"0xde0b295669a9fd93d5f28d9ec85e40f4cb697bae"

The address to query, like `0xfefefefefefefefefefefefefefefefefefefefe`. Up to 20 addresses can be queried, separated by commas.

[​

](#param-tag)

tag

string

default:"latest"

Use `latest` for the last block number of the chain. Also accepts a specific block number in hex format, like `0x10d4f` up to the last 128 blocks. For historical balances, use the [Historical Balance](/api-reference/endpoint/balancehistory) endpoint.

Response

Copy

Ask AI

```
{
   "status":"1",
   "message":"OK",
   "result":"172774397764084972158218"
}

```

---

# Get Historical Native Balance for an Address

**Source:** https://docs.etherscan.io/api-reference/endpoint/balancehistory

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

This [historical endpoint](/resources/rate-limits) is throttled to 2 calls/s

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint.

[​

](#param-action)

action

string

default:"balancehistory"

Set to `balancehistory` for this endpoint.

[​

](#param-address)

address

string

default:"0xde0b295669a9fd93d5f28d9ec85e40f4cb697bae"

The address to query, like `0xfefefefefefefefefefefefefefefefefefefefe`.

[​

](#param-blockno)

blockno

integer

default:"8000000"

Block number to check balance at, all the way up to the genesis block `0`.

Response

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": "610538078574759898951277"
}

```

---

# Chainlist

**Source:** https://docs.etherscan.io/api-reference/endpoint/chainlist

---

###

[​

](#query-parameters)
Query Parameters

No parameters required.

Response

Copy

Ask AI

```
{
  "comments": "List of API endpoints maintained by Etherscan EAAS. Available Status codes are (0)=Offline, (1)=Ok, (2)=Degraded",
  "totalcount": 67,
  "result": [
    {
      "chainname": "Ethereum Mainnet",
      "chainid": "1",
      "blockexplorer": "https://etherscan.io/",
      "apiurl": "https://api.etherscan.io/v2/api?chainid=1",
      "status": 1,
      "comment": ""
    },
    {
      "chainname": "Sepolia Testnet",
      "chainid": "11155111",
      "blockexplorer": "https://sepolia.etherscan.io/",
      "apiurl": "https://api.etherscan.io/v2/api?chainid=11155111",
      "status": 1,
      "comment": ""
    },
    {
      "chainname": "Base Mainnet",
      "chainid": "8453",
      "blockexplorer": "https://basescan.org/",
      "apiurl": "https://api.etherscan.io/v2/api?chainid=8453",
      "status": 1,
      "comment": ""
    },
    {
      "chainname": "Polygon Mainnet",
      "chainid": "137",
      "blockexplorer": "https://polygonscan.com/",
      "apiurl": "https://api.etherscan.io/v2/api?chainid=137",
      "status": 1,
      "comment": ""
    },
    {
      "chainname": "Arbitrum One Mainnet",
      "chainid": "42161",
      "blockexplorer": "https://arbiscan.io/",
      "apiurl": "https://api.etherscan.io/v2/api?chainid=42161",
      "status": 1,
      "comment": ""
    }
  ]
}

```

---

# Get Ethereum Nodes Size

**Source:** https://docs.etherscan.io/api-reference/endpoint/chainsize

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"chainsize"

Set to `chainsize` for this endpoint.

[​

](#param-startdate)

startdate

string

default:"2019-02-01"

Starting date in `yyyy-MM-dd` format.

[​

](#param-enddate)

enddate

string

default:"2019-02-28"

Ending date in `yyyy-MM-dd` format.

[​

](#param-clienttype)

clienttype

string

default:"geth"

Node client to query, either `geth` or `parity`.

[​

](#param-syncmode)

syncmode

string

default:"default"

Node type to run, either `default` or `archive`.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest results first or `asc` for the oldest results first.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=chainsize&startdate=2019-02-01&enddate=2019-02-28&clienttype=geth&syncmode=default&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "blockNumber": "7156164",
      "chainTimeStamp": "2019-02-01",
      "chainSize": "184726421279",
      "clientType": "Geth",
      "syncMode": "Default"
    },
    {
      "blockNumber": "7161012",
      "chainTimeStamp": "2019-02-02",
      "chainSize": "184726654448",
      "clientType": "Geth",
      "syncMode": "Default"
    }
  ]
}

```

---

# Check Proxy Contract Verification Status

**Source:** https://docs.etherscan.io/api-reference/endpoint/checkproxyverification

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"contract"

Set to `contract` for this endpoint.

[​

](#param-action)

action

string

default:"checkproxyverification"

Set to `checkproxyverification` for this endpoint.

[​

](#param-guid)

guid

string

The GUID received from the proxy verification request.

Response

Copy

Ask AI

```
{
  "status": "0",
  "message": "NOTOK",
  "result": "The proxy contract at 0xcbdcd3815b5f975e1a2c944a9b2cd1c985a1cb7f does not seem to be verified. Please verify and publish the contract source before proceeding with this proxy verification."
}

```

---

# Check Source Code Verification Status

**Source:** https://docs.etherscan.io/api-reference/endpoint/checkverifystatus

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"contract"

Set to `contract` for this endpoint.

[​

](#param-action)

action

string

default:"checkverifystatus"

Set to `checkverifystatus` for this endpoint.

[​

](#param-guid)

guid

string

The GUID received from the verification request.

Response

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": "Pass - Verified"
}

```

---

# Get Daily Average Block Size

**Source:** https://docs.etherscan.io/api-reference/endpoint/dailyavgblocksize

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID you query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"dailyavgblocksize"

Set to `dailyavgblocksize` for this endpoint.

[​

](#param-startdate)

startdate

string

default:"2019-02-01"

Starting date in `yyyy-MM-dd` format.

[​

](#param-enddate)

enddate

string

default:"2019-02-28"

Ending date in `yyyy-MM-dd` format.

[​

](#param-sort)

sort

string

default:"desc"

Sort order, either `asc` or `desc`.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=dailyavgblocksize&startdate=2019-02-01&enddate=2019-02-28&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "UTCDate": "2019-02-01",
      "unixTimeStamp": "1548979200",
      "blockSize_bytes": 20373
    },
    {
      "UTCDate": "2019-02-02",
      "unixTimeStamp": "1549065600",
      "blockSize_bytes": 17499
    }
  ]
}

```

---

# Get Daily Average Block Time

**Source:** https://docs.etherscan.io/api-reference/endpoint/dailyavgblocktime

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID you query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"dailyavgblocktime"

Set to `dailyavgblocktime` for this endpoint.

[​

](#param-startdate)

startdate

string

default:"2019-02-01"

Starting date in `yyyy-MM-dd` format.

[​

](#param-enddate)

enddate

string

default:"2019-02-28"

Ending date in `yyyy-MM-dd` format.

[​

](#param-sort)

sort

string

default:"desc"

Sort order, either `asc` or `desc`.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=dailyavgblocktime&startdate=2019-02-01&enddate=2019-02-28&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "UTCDate": "2019-02-01",
      "unixTimeStamp": "1548979200",
      "blockTime_sec": "17.67"
    },
    {
      "UTCDate": "2019-02-02",
      "unixTimeStamp": "1549065600",
      "blockTime_sec": "17.41"
    }
  ]
}

```

---

# Get Daily Average Gas Limit

**Source:** https://docs.etherscan.io/api-reference/endpoint/dailyavggaslimit

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"dailyavggaslimit"

Set to `dailyavggaslimit` for this endpoint.

[​

](#param-startdate)

startdate

string

default:"2019-02-01"

Starting date in `yyyy-MM-dd` format.

[​

](#param-enddate)

enddate

string

default:"2019-02-28"

Ending date in `yyyy-MM-dd` format.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest results first or `asc` for the oldest results first.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=dailyavggaslimit&startdate=2019-02-01&enddate=2019-02-28&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "UTCDate": "2019-02-01",
      "unixTimeStamp": "1548979200",
      "gasLimit": "8001360"
    },
    {
      "UTCDate": "2019-02-02",
      "unixTimeStamp": "1549065600",
      "gasLimit": "8001269"
    }
  ]
}

```

---

# Get Daily Average Gas Price

**Source:** https://docs.etherscan.io/api-reference/endpoint/dailyavggasprice

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"dailyavggasprice"

Set to `dailyavggasprice` for this endpoint.

[​

](#param-startdate)

startdate

string

default:"2019-02-01"

Starting date in `yyyy-MM-dd` format.

[​

](#param-enddate)

enddate

string

default:"2019-02-28"

Ending date in `yyyy-MM-dd` format.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest results first or `asc` for the oldest results first.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=dailyavggasprice&startdate=2019-02-01&enddate=2019-02-28&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "UTCDate": "2019-02-01",
      "unixTimeStamp": "1548979200",
      "maxGasPrice_Wei": "60814303896257",
      "minGasPrice_Wei": "432495",
      "avgGasPrice_Wei": "13234562600"
    },
    {
      "UTCDate": "2019-02-02",
      "unixTimeStamp": "1549065600",
      "maxGasPrice_Wei": "20000000000000",
      "minGasPrice_Wei": "2352",
      "avgGasPrice_Wei": "12000569516"
    }
  ]
}

```

---

# Get Daily Average Network Hash Rate

**Source:** https://docs.etherscan.io/api-reference/endpoint/dailyavghashrate

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"dailyavghashrate"

Set to `dailyavghashrate` for this endpoint.

[​

](#param-startdate)

startdate

string

default:"2019-02-01"

Starting date in `yyyy-MM-dd` format.

[​

](#param-enddate)

enddate

string

default:"2019-02-28"

Ending date in `yyyy-MM-dd` format.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest results first or `asc` for the oldest results first.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=dailyavghashrate&startdate=2019-02-01&enddate=2019-02-28&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "UTCDate": "2019-02-01",
      "unixTimeStamp": "1548979200",
      "networkHashRate": "143116.0140"
    },
    {
      "UTCDate": "2019-02-02",
      "unixTimeStamp": "1549065600",
      "networkHashRate": "143036.2313"
    }
  ]
}

```

---

# Get Daily Average Network Difficulty

**Source:** https://docs.etherscan.io/api-reference/endpoint/dailyavgnetdifficulty

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"dailyavgnetdifficulty"

Set to `dailyavgnetdifficulty` for this endpoint.

[​

](#param-startdate)

startdate

string

default:"2019-02-01"

Starting date in `yyyy-MM-dd` format.

[​

](#param-enddate)

enddate

string

default:"2019-02-28"

Ending date in `yyyy-MM-dd` format.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest results first or `asc` for the oldest results first.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=dailyavgnetdifficulty&startdate=2019-02-01&enddate=2019-02-28&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "UTCDate": "2019-02-01",
      "unixTimeStamp": "1548979200",
      "networkDifficulty": "2,408.028"
    },
    {
      "UTCDate": "2019-02-02",
      "unixTimeStamp": "1549065600",
      "networkDifficulty": "2,358.910"
    }
  ]
}

```

---

# Get Daily Block Count and Rewards

**Source:** https://docs.etherscan.io/api-reference/endpoint/dailyblkcount

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID you query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"dailyblkcount"

Set to `dailyblkcount` for this endpoint.

[​

](#param-startdate)

startdate

string

default:"2019-02-01"

Starting date in `yyyy-MM-dd` format.

[​

](#param-enddate)

enddate

string

default:"2019-02-28"

Ending date in `yyyy-MM-dd` format.

[​

](#param-sort)

sort

string

default:"desc"

Sort order, either `asc` or `desc`.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=dailyblkcount&startdate=2019-02-01&enddate=2019-02-28&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "UTCDate": "2019-02-01",
      "unixTimeStamp": "1548979200",
      "blockCount": 4848,
      "blockRewards_Eth": "14929.464690870590355682"
    },
    {
      "UTCDate": "2019-02-02",
      "unixTimeStamp": "1549065600",
      "blockCount": 4935,
      "blockRewards_Eth": "15120.386084685869906669"
    }
  ]
}

```

---

# Get Daily Block Rewards

**Source:** https://docs.etherscan.io/api-reference/endpoint/dailyblockrewards

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID you query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"dailyblockrewards"

Set to `dailyblockrewards` for this endpoint.

[​

](#param-startdate)

startdate

string

default:"2019-02-01"

Starting date in `yyyy-MM-dd` format.

[​

](#param-enddate)

enddate

string

default:"2019-02-28"

Ending date in `yyyy-MM-dd` format.

[​

](#param-sort)

sort

string

default:"desc"

Sort order, either `asc` or `desc`.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=dailyblockrewards&startdate=2019-02-01&enddate=2019-02-28&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "UTCDate": "2019-02-01",
      "unixTimeStamp": "1548979200",
      "blockRewards_Eth": "15300.65625"
    },
    {
      "UTCDate": "2019-02-02",
      "unixTimeStamp": "1549065600",
      "blockRewards_Eth": "15611.625"
    }
  ]
}

```

---

# Get Ethereum Daily Total Gas Used

**Source:** https://docs.etherscan.io/api-reference/endpoint/dailygasused

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"dailygasused"

Set to `dailygasused` for this endpoint.

[​

](#param-startdate)

startdate

string

default:"2019-02-01"

Starting date in `yyyy-MM-dd` format.

[​

](#param-enddate)

enddate

string

default:"2019-02-28"

Ending date in `yyyy-MM-dd` format.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest results first or `asc` for the oldest results first.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=dailygasused&startdate=2019-02-01&enddate=2019-02-28&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "UTCDate": "2019-02-01",
      "unixTimeStamp": "1548979200",
      "gasUsed": "32761450415"
    },
    {
      "UTCDate": "2019-02-02",
      "unixTimeStamp": "1549065600",
      "gasUsed": "30168904532"
    }
  ]
}

```

---

# Get Daily Network Utilization

**Source:** https://docs.etherscan.io/api-reference/endpoint/dailynetutilization

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"dailynetutilization"

Set to `dailynetutilization` for this endpoint.

[​

](#param-startdate)

startdate

string

default:"2019-02-01"

Starting date in `yyyy-MM-dd` format.

[​

](#param-enddate)

enddate

string

default:"2019-02-28"

Ending date in `yyyy-MM-dd` format.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest results first or `asc` for the oldest results first.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=dailynetutilization&startdate=2019-02-01&enddate=2019-02-28&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "UTCDate": "2019-02-01",
      "unixTimeStamp": "1548979200",
      "networkUtilization": "0.8464"
    },
    {
      "UTCDate": "2019-02-02",
      "unixTimeStamp": "1549065600",
      "networkUtilization": "0.7687"
    }
  ]
}

```

---

# Get Daily New Address Count

**Source:** https://docs.etherscan.io/api-reference/endpoint/dailynewaddress

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"dailynewaddress"

Set to `dailynewaddress` for this endpoint.

[​

](#param-startdate)

startdate

string

default:"2019-02-01"

Starting date in `yyyy-MM-dd` format.

[​

](#param-enddate)

enddate

string

default:"2019-02-28"

Ending date in `yyyy-MM-dd` format.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest results first or `asc` for the oldest results first.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=dailynewaddress&startdate=2019-02-01&enddate=2019-02-28&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "UTCDate": "2019-02-01",
      "unixTimeStamp": "1548979200",
      "newAddressCount": 54081
    },
    {
      "UTCDate": "2019-02-02",
      "unixTimeStamp": "1549065600",
      "newAddressCount": 65152
    }
  ]
}

```

---

# Get Daily Transaction Count

**Source:** https://docs.etherscan.io/api-reference/endpoint/dailytx

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"dailytx"

Set to `dailytx` for this endpoint.

[​

](#param-startdate)

startdate

string

default:"2019-02-01"

Starting date in `yyyy-MM-dd` format.

[​

](#param-enddate)

enddate

string

default:"2019-02-28"

Ending date in `yyyy-MM-dd` format.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest results first or `asc` for the oldest results first.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=dailytx&startdate=2019-02-01&enddate=2019-02-28&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "UTCDate": "2019-02-01",
      "unixTimeStamp": "1548979200",
      "transactionCount": 498856
    },
    {
      "UTCDate": "2019-02-02",
      "unixTimeStamp": "1549065600",
      "transactionCount": 450314
    }
  ]
}

```

---

# Get Daily Network Transaction Fee

**Source:** https://docs.etherscan.io/api-reference/endpoint/dailytxnfee

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"dailytxnfee"

Set to `dailytxnfee` for this endpoint.

[​

](#param-startdate)

startdate

string

default:"2019-02-01"

Starting date in `yyyy-MM-dd` format.

[​

](#param-enddate)

enddate

string

default:"2019-02-28"

Ending date in `yyyy-MM-dd` format.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest results first or `asc` for the oldest results first.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=dailytxnfee&startdate=2019-02-01&enddate=2019-02-28&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "UTCDate": "2019-02-01",
      "unixTimeStamp": "1548979200",
      "transactionFee_Eth": "358.558440870590355682"
    },
    {
      "UTCDate": "2019-02-02",
      "unixTimeStamp": "1549065600",
      "transactionFee_Eth": "286.886084685869906669"
    }
  ]
}

```

---

# Get Daily Uncle Block Count and Rewards

**Source:** https://docs.etherscan.io/api-reference/endpoint/dailyuncleblkcount

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID you query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"dailyuncleblkcount"

Set to `dailyuncleblkcount` for this endpoint.

[​

](#param-startdate)

startdate

string

default:"2019-02-01"

Starting date in `yyyy-MM-dd` format.

[​

](#param-enddate)

enddate

string

default:"2019-02-28"

Ending date in `yyyy-MM-dd` format.

[​

](#param-sort)

sort

string

default:"desc"

Sort order, either `asc` or `desc`.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=dailyuncleblkcount&startdate=2019-02-01&enddate=2019-02-28&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "UTCDate": "2019-02-01",
      "unixTimeStamp": "1548979200",
      "uncleBlockCount": 287,
      "uncleBlockRewards_Eth": "729.75"
    },
    {
      "UTCDate": "2019-02-02",
      "unixTimeStamp": "1549065600",
      "uncleBlockCount": 304,
      "uncleBlockRewards_Eth": "778.125"
    }
  ]
}

```

---

# eth_blockNumber

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethblocknumber

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"proxy"

Set to `proxy` for this endpoint.

[​

](#param-action)

action

string

default:"eth_blockNumber"

Set to `eth_blockNumber` for this endpoint.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=proxy&action=eth_blockNumber&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "jsonrpc": "2.0",
  "id": 83,
  "result": "0x1661760"
}

```

---

# eth_call

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethcall

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"proxy"

Set to `proxy` for this endpoint.

[​

](#param-action)

action

string

default:"eth_call"

Set to `eth_call` for this endpoint.

[​

](#param-to)

to

string

default:"0xAEEF46DB4855E25702F8237E8f403FddcaF931C0"

The address to interact with.

[​

](#param-data)

data

string

Hash of the method signature and encoded parameters.

[​

](#param-tag)

tag

string

default:"latest"

Use `latest`, `earliest`, `pending`, or a block number in hex.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=proxy&action=eth_call&to=0xAEEF46DB4855E25702F8237E8f403FddcaF931C0&data=0x70a08231000000000000000000000000e16359506c028e51f16be38986ec5746251e9724&tag=latest&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x00000000000000000000000000000000000000000000000000601d8888141c00"
}

```

---

# Get Ether Historical Price

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethdailyprice

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"ethdailyprice"

Set to `ethdailyprice` for this endpoint.

[​

](#param-startdate)

startdate

string

default:"2019-02-01"

Starting date in `yyyy-MM-dd` format.

[​

](#param-enddate)

enddate

string

default:"2019-02-28"

Ending date in `yyyy-MM-dd` format.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest results first or `asc` for the oldest results first.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=ethdailyprice&startdate=2019-02-01&enddate=2019-02-28&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "UTCDate": "2019-02-01",
      "unixTimeStamp": "1548979200",
      "value": "107.03"
    },
    {
      "UTCDate": "2019-02-02",
      "unixTimeStamp": "1549065600",
      "value": "111.00"
    }
  ]
}

```

---

# eth_estimateGas

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethestimategas

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"proxy"

Set to `proxy` for this endpoint.

[​

](#param-action)

action

string

default:"eth_estimateGas"

Set to `eth_estimateGas` for this endpoint.

[​

](#param-data)

data

string

default:"0x4e71d92d"

Hash of the method signature and encoded parameters.

[​

](#param-to)

to

string

default:"0xf0160428a8552ac9bb7e050d90eeade4ddd52843"

The address to interact with.

[​

](#param-value)

value

string

default:"0xff22"

Value sent in this transaction, in hex.

[​

](#param-gas-price)

gasPrice

string

default:"0x51da038cc"

Gas price in wei.

[​

](#param-gas)

gas

string

default:"0x5f5e0ff"

Gas limit provided for the transaction, in hex.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=proxy&action=eth_estimateGas&data=0x4e71d92d&to=0xf0160428a8552ac9bb7e050d90eeade4ddd52843&value=0xff22&gasPrice=0x51da038cc&gas=0x5f5e0ff&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x66ac"
}

```

---

# eth_gasPrice

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethgasprice

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"proxy"

Set to `proxy` for this endpoint.

[​

](#param-action)

action

string

default:"eth_gasPrice"

Set to `eth_gasPrice` for this endpoint.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=proxy&action=eth_gasPrice&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "jsonrpc": "2.0",
  "id": 73,
  "result": "0x1d9d2db2"
}

```

---

# eth_getBlockByNumber

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethgetblockbynumber

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"proxy"

Set to `proxy` for this endpoint.

[​

](#param-action)

action

string

default:"eth_getBlockByNumber"

Set to `eth_getBlockByNumber` for this endpoint.

[​

](#param-tag)

tag

string

default:"0x10d4f"

Block number in hex format.

[​

](#param-boolean)

boolean

string

default:"true"

Set to `true` to return full transaction objects or `false` for transaction hashes only.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=proxy&action=eth_getBlockByNumber&tag=0x10d4f&boolean=true&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "difficulty": "0x1d95715bd14",
    "extraData": "0x",
    "gasLimit": "0x2fefd8",
    "gasUsed": "0x5208",
    "hash": "0x7eb7c23a5ac2f2d70aa1ba4e5c56d89de5ac993590e5f6e79c394e290d998ba8",
    "logsBloom": "0x000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
    "miner": "0xf927a40c8b7f6e07c5af7fa2155b4864a4112b13",
    "mixHash": "0x13dd2c8aec729f75aebcd79a916ecb0f7edc6493efcc6a4da8d7b0ab3ee88444",
    "nonce": "0xc60a782e2e69ce22",
    "number": "0x10d4f",
    "parentHash": "0xf8d01370e6e274f8188954fbee435b40c35b2ad3d4ab671f6d086cd559e48f04",
    "receiptsRoot": "0x0c44b7ed0fefb613ec256341aa0ffdb643e869e3a0ebc8f58e36b4e47efedd33",
    "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
    "size": "0x275",
    "stateRoot": "0xd64a0f63e2c7f541e6e6f8548a10a5c4e49fda7ac1aa80f9dddef648c7b9e25f",
    "timestamp": "0x55c9ea07",
    "transactions": [
      {
        "blockHash": "0x7eb7c23a5ac2f2d70aa1ba4e5c56d89de5ac993590e5f6e79c394e290d998ba8",
        "blockNumber": "0x10d4f",
        "from": "0x4458f86353b4740fe9e09071c23a7437640063c9",
        "gas": "0x5208",
        "gasPrice": "0xba43b7400",
        "hash": "0xa442249820de6be754da81eafbd44a865773e4b23d7c0522d31fd03977823008",
        "input": "0x",
        "nonce": "0x1",
        "to": "0xbf3403210f9802205f426759947a80a9fda71b1e",
        "transactionIndex": "0x0",
        "value": "0xaa9f075c200000",
        "type": "0x0",
        "v": "0x1b",
        "r": "0x2c2789c6704ba2606e200e1ba4fd17ba4f0e0f94abe32a12733708c3d3442616",
        "s": "0x2946f47e3ece580b5b5ecb0f8c52604fa5f60aeb4103fc73adcbf6d620f9872b"
      }
    ],
    "transactionsRoot": "0x4a5b78c13d11559c9541576834b5172fe8b18507c0f9f76454fcdddedd8dff7a",
    "uncles": []
  }
}

```

---

# eth_getBlockTransactionCountByNumber

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethgetblocktransactioncountbynumber

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"proxy"

Set to `proxy` for this endpoint.

[​

](#param-action)

action

string

default:"eth_getBlockTransactionCountByNumber"

Set to `eth_getBlockTransactionCountByNumber` for this endpoint.

[​

](#param-tag)

tag

string

default:"0x10FB78"

Block number in hex format.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=proxy&action=eth_getBlockTransactionCountByNumber&tag=0x10FB78&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x3"
}

```

---

# eth_getCode

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethgetcode

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"proxy"

Set to `proxy` for this endpoint.

[​

](#param-action)

action

string

default:"eth_getCode"

Set to `eth_getCode` for this endpoint.

[​

](#param-address)

address

string

default:"0xf75e354c5edc8efed9b59ee9f67a80845ade7d0c"

The address to query.

[​

](#param-tag)

tag

string

default:"latest"

Use `latest`, `earliest`, `pending`, or a block number in hex.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=proxy&action=eth_getCode&address=0xf75e354c5edc8efed9b59ee9f67a80845ade7d0c&tag=latest&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x3660008037602060003660003473273930d21e01ee25e4c219b63259d214872220a261235a5a03f21560015760206000f3"
}

```

---

# eth_getStorageAt

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethgetstorageat

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"proxy"

Set to `proxy` for this endpoint.

[​

](#param-action)

action

string

default:"eth_getStorageAt"

Set to `eth_getStorageAt` for this endpoint.

[​

](#param-address)

address

string

default:"0x6e03d9cce9d60f3e9f2597e13cd4c54c55330cfd"

The address to query.

[​

](#param-position)

position

string

default:"0x0"

Storage position in hex.

[​

](#param-tag)

tag

string

default:"latest"

Use `latest`, `earliest`, `pending`, or a block number in hex.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=proxy&action=eth_getStorageAt&address=0x6e03d9cce9d60f3e9f2597e13cd4c54c55330cfd&position=0x0&tag=latest&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x0000000000000000000000000000000000000000000000000000000000000000"
}

```

---

# eth_getTransactionByBlockNumberAndIndex

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethgettransactionbyblocknumberandindex

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"proxy"

Set to `proxy` for this endpoint.

[​

](#param-action)

action

string

default:"eth_getTransactionByBlockNumberAndIndex"

Set to `eth_getTransactionByBlockNumberAndIndex` for this endpoint.

[​

](#param-tag)

tag

string

default:"0xC6331D"

Block number in hex format.

[​

](#param-index)

index

string

default:"0x11A"

Transaction index position in the block, in hex.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=proxy&action=eth_getTransactionByBlockNumberAndIndex&tag=0xC6331D&index=0x11A&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "blockHash": "0x13c5311a78b9a8790c58f1d0d0660226352a498cc98b06e1861f1967d3a9d1b4",
    "blockNumber": "0xc6331d",
    "from": "0xd1af036d589b6ebfbaa184b339a30d32ef611708",
    "gas": "0x11170",
    "gasPrice": "0x5b7e259a7",
    "maxFeePerGas": "0x5b7e259a7",
    "maxPriorityFeePerGas": "0x5b7e259a7",
    "hash": "0xc7ef51f0bfe85eefbb1d4d88f5a39e82fbfc94987d8cbcb515f74d80b6e44902",
    "input": "0x2d2da806000000000000000000000000d1af036d589b6ebfbaa184b339a30d32ef611708",
    "nonce": "0x0",
    "to": "0xabea9132b05a70803a4e85094fd0e1800777fbef",
    "transactionIndex": "0x11a",
    "value": "0x57930b5db6a000",
    "type": "0x2",
    "accessList": [],
    "chainId": "0x1",
    "v": "0x0",
    "r": "0x595619be17755f7427d07626275e4c89b8f9d5a0a668515165ea000d30f26325",
    "s": "0x33d4b22c7c779fc9f04c81788cb4268d2c3f8dc9e219245570bdd384c6ca6690",
    "yParity": "0x0"
  }
}

```

---

# eth_getTransactionByHash

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethgettransactionbyhash

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"proxy"

Set to `proxy` for this endpoint.

[​

](#param-action)

action

string

default:"eth_getTransactionByHash"

Set to `eth_getTransactionByHash` for this endpoint.

[​

](#param-txhash)

txhash

string

The transaction hash to query.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=proxy&action=eth_getTransactionByHash&txhash=0xbc78ab8a9e9a0bca7d0321a27b2c03addeae08ba81ea98b03cd3dd237eabed44&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "blockHash": "0xf850331061196b8f2b67e1f43aaa9e69504c059d3d3fb9547b04f9ed4d141ab7",
    "blockNumber": "0xcf2420",
    "from": "0x00192fb10df37c9fb26829eb2cc623cd1bf599e8",
    "gas": "0x5208",
    "gasPrice": "0x19f017ef49",
    "maxFeePerGas": "0x1f6ea08600",
    "maxPriorityFeePerGas": "0x3b9aca00",
    "hash": "0xbc78ab8a9e9a0bca7d0321a27b2c03addeae08ba81ea98b03cd3dd237eabed44",
    "input": "0x",
    "nonce": "0x33b79d",
    "to": "0xc67f4e626ee4d3f272c2fb31bad60761ab55ed9f",
    "transactionIndex": "0x5b",
    "value": "0x19755d4ce12c00",
    "type": "0x2",
    "accessList": [],
    "chainId": "0x1",
    "v": "0x0",
    "r": "0xa681faea68ff81d191169010888bbbe90ec3eb903e31b0572cd34f13dae281b9",
    "s": "0x3f59b0fa5ce6cf38aff2cfeb68e7a503ceda2a72b4442c7e2844d63544383e3",
    "yParity": "0x0"
  }
}

```

---

# eth_getTransactionCount

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethgettransactioncount

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"proxy"

Set to `proxy` for this endpoint.

[​

](#param-action)

action

string

default:"eth_getTransactionCount"

Set to `eth_getTransactionCount` for this endpoint.

[​

](#param-address)

address

string

default:"0x4bd5900Cb274ef15b153066D736bf3e83A9ba44e"

The address to query.

[​

](#param-tag)

tag

string

default:"latest"

Use `latest`, `earliest`, `pending`, or a block number in hex.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=proxy&action=eth_getTransactionCount&address=0x4bd5900Cb274ef15b153066D736bf3e83A9ba44e&tag=latest&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x73"
}

```

---

# eth_getTransactionReceipt

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethgettransactionreceipt

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"proxy"

Set to `proxy` for this endpoint.

[​

](#param-action)

action

string

default:"eth_getTransactionReceipt"

Set to `eth_getTransactionReceipt` for this endpoint.

[​

](#param-txhash)

txhash

string

The transaction hash to query.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=proxy&action=eth_getTransactionReceipt&txhash=0xadb8aec59e80db99811ac4a0235efa3e45da32928bcff557998552250fa672eb&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "blockHash": "0x07c17710dbb7514e92341c9f83b4aab700c5dba7c4fb98caadd7926a32e47799",
    "blockNumber": "0xcf2427",
    "contractAddress": null,
    "cumulativeGasUsed": "0xeb67d5",
    "effectiveGasPrice": "0x1a96b24c26",
    "from": "0x292f04a44506c2fd49bac032e1ca148c35a478c8",
    "gasUsed": "0xb41d",
    "logs": [
      {
        "address": "0xdac17f958d2ee523a2206206994597c13d831ec7",
        "topics": [
          "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
          "0x000000000000000000000000292f04a44506c2fd49bac032e1ca148c35a478c8",
          "0x000000000000000000000000ab6960a6511ff18ed8b8c012cb91c7f637947fc0"
        ],
        "data": "0x00000000000000000000000000000000000000000000000000000000013f81a6",
        "blockNumber": "0xcf2427",
        "transactionHash": "0xadb8aec59e80db99811ac4a0235efa3e45da32928bcff557998552250fa672eb",
        "transactionIndex": "0x122",
        "blockHash": "0x07c17710dbb7514e92341c9f83b4aab700c5dba7c4fb98caadd7926a32e47799",
        "logIndex": "0xdb",
        "removed": false
      }
    ],
    "logsBloom": "0x0000000000000000000000000000000000000000000000000000000000000400000000000400000000000000000001000000000000000000000000000000000000000000000000000000000800000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000001000000000110000000000000000000000000000000000000000000000000000000200100000000000000000000000000080000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
    "status": "0x1",
    "to": "0xdac17f958d2ee523a2206206994597c13d831ec7",
    "transactionHash": "0xadb8aec59e80db99811ac4a0235efa3e45da32928bcff557998552250fa672eb",
    "transactionIndex": "0x122",
    "type": "0x2"
  }
}

```

---

# eth_getUncleByBlockNumberAndIndex

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethgetunclebyblocknumberandindex

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"proxy"

Set to `proxy` for this endpoint.

[​

](#param-action)

action

string

default:"eth_getUncleByBlockNumberAndIndex"

Set to `eth_getUncleByBlockNumberAndIndex` for this endpoint.

[​

](#param-tag)

tag

string

default:"0xC63276"

Block number in hex format.

[​

](#param-index)

index

string

default:"0x0"

Position of the uncle in the block, in hex.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=proxy&action=eth_getUncleByBlockNumberAndIndex&tag=0xC63276&index=0x0&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "baseFeePerGas": "0x65a42b13c",
    "difficulty": "0x1b1457a8247bbb",
    "extraData": "0x486976656f6e2063612d68656176792059476f6e",
    "gasLimit": "0x1ca359a",
    "gasUsed": "0xb48fe1",
    "hash": "0x1da88e3581315d009f1cb600bf06f509cd27a68cb3d6437bda8698d04089f14a",
    "logsBloom": "0xf1a360ca505cdda510d810c1c81a03b51a8a508ed601811084833072945290235c8721e012182e40d57df552cf00f1f01bc498018da19e008681832b43762a30c26e11709948a9b96883a42ad02568e3fcc3000004ee12813e4296498261619992c40e22e60bd95107c5bd8462fcca570a0095d52a4c24720b00f13a2c3d62aca81e852017470c109643b15041fd69742406083d67654fc841a18b405ab380e06a8c14c0138b6602ea8f48b2cd90ac88c3478212011136802900264718a085047810221225080dfb2c214010091a6f233883bb0084fa1c197330a10bb0006686e678b80e50e4328000041c218d1458880181281765d28d51066058f3f80a7822",
    "miner": "0x1ad91ee08f21be3de0ba2ba6918e714da6b45836",
    "mixHash": "0xa8e1dbbf073614c7ed05f44b9e92fbdb3e1d52575ed8167fa57f934210bbb0a2",
    "nonce": "0x28cc3e5b7bee9866",
    "number": "0xc63274",
    "parentHash": "0x496dae3e722efdd9ee1eb69499bdc7ed0dca54e13cd1157a42811c442f01941f",
    "receiptsRoot": "0x9c9a7a99b4af7607691a7f2a50d474290385c0a6f39c391131ea0c67307213f4",
    "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
    "size": "0x224",
    "stateRoot": "0xde9a11f0ee321390c1a7843cab7b9ffd3779d438bc8f77de4361dfe2807d7dee",
    "timestamp": "0x6110bd1a",
    "transactionsRoot": "0xa04a79e531db3ec373cb63e9ebfbc9c95525de6347958918a273675d4f221575",
    "uncles": []
  }
}

```

---

# Get Ether Last Price

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethprice

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"ethprice"

Set to `ethprice` for this endpoint.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=ethprice&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": {
    "ethbtc": "0.0368322132423383",
    "ethbtc_timestamp": "1759139509",
    "ethusd": "4129.30651044757",
    "ethusd_timestamp": "1759139509"
  }
}

```

---

# eth_sendRawTransaction

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethsendrawtransaction

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"proxy"

Set to `proxy` for this endpoint.

[​

](#param-action)

action

string

default:"eth_sendRawTransaction"

Set to `eth_sendRawTransaction` for this endpoint.

[​

](#param-hex)

hex

string

default:"0xf904808000831cfde080"

The signed raw transaction data to broadcast.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=proxy&action=eth_sendRawTransaction&hex=0xf904808000831cfde080&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": -32000,
    "message": "rlp: value size exceeds available input length"
  }
}

```

---

# Get Total Supply of Ether

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethsupply

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"ethsupply"

Set to `ethsupply` for this endpoint.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=ethsupply&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": "122373866217800000000000000"
}

```

---

# Get Total Supply of Ether 2

**Source:** https://docs.etherscan.io/api-reference/endpoint/ethsupply2

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"ethsupply2"

Set to `ethsupply2` for this endpoint.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=ethsupply2&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": {
    "EthSupply": "122373866217800000000000000",
    "Eth2Staking": "2940327167090033000000000",
    "BurntFees": "4610942856337430575173814",
    "WithdrawnTotal": "7618584348954597000000000"
  }
}

```

---

# Export Address Tags

**Source:** https://docs.etherscan.io/api-reference/endpoint/exportaddresstags

---

This is a PRO endpoint, available to the [Enterprise tier](https://etherscan.io/apis)

This endpoint is throttled to 2 calls/s and 100 calls/day

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-module)

module

string

default:"nametag"

Set to `nametag` for this endpoint.

[​

](#param-action)

action

string

default:"exportaddresstags"

Set to `exportaddresstags` for this endpoint.

[​

](#param-label)

label

string

default:"ofac-sanctioned"

Use the [Label Master List](/api-reference/endpoint/getlabelmasterlist) to discover categeories like `phish-hack`, or pass `all` to export every label.

[​

](#param-format)

format

string

default:"csv"

The response format, currently only in `.csv`.

Response

Copy

Ask AI

```
"address";"nametag";"internal_nametag";"url";"shortdescription";"notes_1";"notes_2";"labels";"labels_slug";"reputation";"other_attributes";"lastupdatedtimestamp"
"0x0000000000000000000000000000000000001111";"Fake_Phishing589039";"";"";"";"There are reports that this address was used in a Phishing scam. Please exercise caution when interacting with it. Reported by HashDit.";"";"Phish / Hack";"phish-hack";"2";"";"1728292621"
"0x00000000001adc2c0b202d0f72ad9d50f0675296";"Fake_Phishing1334182";"";"";"";"There are reports that this address was used in a Phishing scam. Please exercise caution when interacting with it. Reported by BlockSec.";"";"Phish / Hack";"phish-hack";"2";"";"1757057505"
"0x00000000006a2b6820feb8d5ac00cb6800795d8f";"Fake_Phishing326507";"";"";"";"There are reports that this address was used in a Phishing scam. Please exercise caution when interacting with it.";"";"Phish / Hack";"phish-hack";"2";"";"1711418316"
"0x0000000000d67e5d5f991a40f04ed40fa3b150df";"Fake_Phishing326508";"";"";"";"There are reports that this address was used in a Phishing scam. Please exercise caution when interacting with it.";"";"Phish / Hack";"phish-hack";"2";"";"1711418362"
"0x0000000000e7f7e18e6b7890ea1227ff088e6653";"Fake_Phishing686515";"";"";"";"There are reports that this address was used in a Phishing scam. Please exercise caution when interacting with it. Reported by ScamSniffer.";"";"Phish / Hack";"phish-hack";"2";"";"1734599538"
"0x00000000020e203eb8fc5d0724e7c93e6b002c71";"Fake_Phishing119146";"";"";"";"Warning! This address may be attempting to impersonate a similar looking address. Please proceed with caution.";"";"Phish / Hack";"phish-hack";"2";"";"1738570688"

```

---

# Get Estimation of Confirmation Time

**Source:** https://docs.etherscan.io/api-reference/endpoint/gasestimate

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"gastracker"

Set to `gastracker` for this endpoint.

[​

](#param-action)

action

string

default:"gasestimate"

Set to `gasestimate` for this endpoint.

[​

](#param-gasprice)

gasprice

string

default:"2000000000"

Gas price paid per unit of gas, in wei.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=gastracker&action=gasestimate&gasprice=2000000000&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": "45"
}

```

---

# Get Gas Oracle

**Source:** https://docs.etherscan.io/api-reference/endpoint/gasoracle

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"gastracker"

Set to `gastracker` for this endpoint.

[​

](#param-action)

action

string

default:"gasoracle"

Set to `gasoracle` for this endpoint.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=gastracker&action=gasoracle&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": {
    "LastBlock": "23467872",
    "SafeGasPrice": "0.496839934",
    "ProposeGasPrice": "0.496840168",
    "FastGasPrice": "0.55411917",
    "suggestBaseFee": "0.496839934",
    "gasUsedRatio": "0.405942555555556,0.784013777777778,0.502624148542588,0.719479644444444,0.45673524947105"
  }
}

```

---

# Get Contract ABI

**Source:** https://docs.etherscan.io/api-reference/endpoint/getabi

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"contract"

Set to `contract` for this endpoint.

[​

](#param-action)

action

string

default:"getabi"

Set to `getabi` for this endpoint.

[​

](#param-address)

address

string

default:"0xBB9bc244D798123fDe783fCc1C72d3Bb8C189413"

The contract address to query.

Response

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": "[{\"constant\":false,\"inputs\":[{\"name\":\"_c\",\"type\":\"string\"}],\"name\":\"enterValue\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"test\",\"outputs\":[{\"name\":\"\",\"type\":\"string\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"}]"
}

```

---

# Get Nametag for an Address

**Source:** https://docs.etherscan.io/api-reference/endpoint/getaddresstag

---

This is a PRO endpoint, available to the [Pro Plus tier](https://etherscan.io/apis)

This [historical endpoint](/resources/rate-limits) is throttled to 2 calls/s

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"nametag"

Set to `nametag` for this endpoint.

[​

](#param-action)

action

string

default:"getaddresstag"

Set to `getaddresstag` for this endpoint.

[​

](#param-address)

address

string

default:"0x0000db5C8B030AE20308AC975898E09741E70000"

The address to query, like `0x0000db5C8B030AE20308AC975898E09741E70000`.

Response

Copy

Ask AI

```
{
   "status":"1",
   "message":"OK",
   "result":[
      {
         "address":"0xa9d1e08c7793af67e9d92fe308d5697fb81d3e43",
         "nametag":"Coinbase 10",
         "internal_nametag":"",
         "url":"https://coinbase.com",
         "shortdescription":"",
         "notes_1":"",
         "notes_2":"",
         "labels":[
            "Coinbase",
            "Exchange"
         ],
         "labels_slug":[
            "coinbase",
            "exchange"
         ],
         "reputation":0,
         "other_attributes":[

         ],
         "lastupdatedtimestamp":1721899658
      }
   ]
}

```

---

# Get Estimated Block Countdown by Block Number

**Source:** https://docs.etherscan.io/api-reference/endpoint/getblockcountdown

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID you query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"block"

Set to `block` for this endpoint.

[​

](#param-action)

action

string

default:"getblockcountdown"

Set to `getblockcountdown` for this endpoint.

[​

](#param-blockno)

blockno

integer

default:"16701588"

Block number to estimate time remaining for.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=block&action=getblockcountdown&blockno=16701588&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "0",
  "message": "NOTOK",
  "result": "Error! Block number already pass"
}

```

---

# Get Block Number by Timestamp

**Source:** https://docs.etherscan.io/api-reference/endpoint/getblocknobytime

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID you query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"block"

Set to `block` for this endpoint.

[​

](#param-action)

action

string

default:"getblocknobytime"

Set to `getblocknobytime` for this endpoint.

[​

](#param-timestamp)

timestamp

integer

default:"1578638524"

Unix timestamp in seconds.

[​

](#param-closest)

closest

string

default:"before"

Closest available block to the provided timestamp, either `before` or `after`.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=block&action=getblocknobytime&timestamp=1578638524&closest=before&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": "9251482"
}

```

---

# Get Block and Uncle Rewards by Block Number

**Source:** https://docs.etherscan.io/api-reference/endpoint/getblockreward

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID you query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"block"

Set to `block` for this endpoint.

[​

](#param-action)

action

string

default:"getblockreward"

Set to `getblockreward` for this endpoint.

[​

](#param-blockno)

blockno

integer

default:"2165403"

Block number to check rewards for.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=block&action=getblockreward&blockno=2165403&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": {
    "blockNumber": "2165403",
    "timeStamp": "1472533979",
    "blockMiner": "0x13a06d3dfe21e0db5c016c03ea7d2509f7f8d1e3",
    "blockReward": "5314181600000000000",
    "uncles": [
      {
        "miner": "0xbcdfc35b86bedf72f0cda046a3c16829a2ef41d1",
        "unclePosition": "0",
        "blockreward": "3750000000000000000"
      },
      {
        "miner": "0x0d0c9855c722ff0c78f21e43aa275a5b8ea60dce",
        "unclePosition": "1",
        "blockreward": "3750000000000000000"
      }
    ],
    "uncleInclusionReward": "312500000000000000"
  }
}

```

---

# Get Contract Creator and Creation Tx Hash

**Source:** https://docs.etherscan.io/api-reference/endpoint/getcontractcreation

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"contract"

Set to `contract` for this endpoint.

[​

](#param-action)

action

string

default:"getcontractcreation"

Set to `getcontractcreation` for this endpoint.

[​

](#param-contractaddresses)

contractaddresses

string

Up to 5 contract addresses, separated by commas.

Response

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "contractAddress": "0xcbdcd3815b5f975e1a2c944a9b2cd1c985a1cb7f",
      "contractCreator": "0x3d080421c9dd5fb387d6e3124f7e1c241ade9568",
      "txHash": "0xdce495a9261c4a2a5d4e879cfb55c060b4616a846d3425c441a9e31aa34c956f",
      "blockNumber": "10720863",
      "timestamp": "1598242563",
      "contractFactory": "",
      "creationBytecode": "0x602d3d8160093d39f3363d3d373d3d3d363d73c6cf0f044ba8ea402bfedf9e87b88bf1c008d1625af43d82803e903d91602b57fd5bf3"
    }
  ]
}

```

---

# Get Deposit Transactions by Address

**Source:** https://docs.etherscan.io/api-reference/endpoint/getdeposittxs

---

This endpoint is only available for the Arbitrum and Optimism stack chains.

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"10"

Chain ID to query, eg `10` for Optimism or `42161` for Arbitrum from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint.

[​

](#param-action)

action

string

default:"getdeposittxs"

Set to `getdeposittxs` for this endpoint.

[​

](#param-address)

address

string

default:"0x80f3950a4d371c43360f292a4170624abd9eed03"

Address to check for cross-chain deposits from Ethereum to L2.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"10"

Number of records per page.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest transactions first or `asc` for the oldest transactions first.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=10&module=account&action=getdeposittxs&address=0x80f3950a4d371c43360f292a4170624abd9eed03&page=1&offset=10&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "blockNumber": "132992375",
      "timeStamp": "1741583527",
      "blockHash": "0xef2ff158c8b12be842429f4a8cde58bfa6a389c5274b46f8a1dd2ee7f958ca4d",
      "hash": "0x64ccd0cfa9f333578b36227492f3bc7f5f3ec4bfa82cdc46f82884db680d8e5b",
      "nonce": "520502",
      "from": "0x36bde71c97b33cc4729cf772ae268934f7ab70b2",
      "to": "0x4200000000000000000000000000000000000007",
      "value": "598200000000000",
      "gas": "490798",
      "gasPrice": "0",
      "input": "0xd764ad0b000100000000000000000000000000000000000000000000000000000002802a00000000000000000000000099c9fc46f92e8a1c0dec1b1747d010903e884be10000000000000000000000004200000000000000000000000000000000000001000000000000000000000000000000000000000000000000000002200f4a81300000000000000000000000000000000000000000000000000000000000000030d4000000000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000000a41635f5fd0000000000000000000000001231deb6f5749ef6ce6943a275a1d3e7486f4eae00000000000000000000000080f3950a4d371c43360f292a4170624abd9eed03000000000000000000000000000000000000000000000000000002200f4a813000000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
      "cumulativeGasUsed": "169497",
      "gasUsed": "117234",
      "isError": "0",
      "errDescription": "",
      "txreceipt_status": "1",
      "queueIndex": "999999",
      "L1transactionhash": "0x303bd05c47e62e1243a33210e535ebc70a7567e53a9972fbdef52ee5bcda5acb",
      "L1TxOrigin": "0x36bde71c97b33cc4729cf772ae268934f7ab70b2",
      "tokenAddress": "ETH",
      "tokenSentFrom": "",
      "tokenSentTo": "0x80f3950a4d371c43360f292a4170624abd9eed03",
      "tokenValue": "598200000000000"
    },
    {
      "blockNumber": "134878032",
      "timeStamp": "1745354841",
      "blockHash": "0xd1a47b46e3aaed25c594507cf9df59c9ba7a25542fabed7e2b646efe8b6944f9",
      "hash": "0xb7cbf6eb529f60b28c6707a52e17c84e75bbe0a968e668a29e5a7550bfb00e92",
      "nonce": "527247",
      "from": "0x36bde71c97b33cc4729cf772ae268934f7ab70b2",
      "to": "0x4200000000000000000000000000000000000007",
      "value": "1195403000000000",
      "gas": "490798",
      "gasPrice": "0",
      "input": "0xd764ad0b0001000000000000000000000000000000000000000000000000000000029a8300000000000000000000000099c9fc46f92e8a1c0dec1b1747d010903e884be100000000000000000000000042000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000043f36732dae00000000000000000000000000000000000000000000000000000000000000030d4000000000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000000a41635f5fd0000000000000000000000001231deb6f5749ef6ce6943a275a1d3e7486f4eae00000000000000000000000080f3950a4d371c43360f292a4170624abd9eed030000000000000000000000000000000000000000000000000000043f36732dae000000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
      "cumulativeGasUsed": "147297",
      "gasUsed": "92234",
      "isError": "0",
      "errDescription": "",
      "txreceipt_status": "1",
      "queueIndex": "999999",
      "L1transactionhash": "0xdca5992b97fcb3d2aa4d06f8c47c0786c0007009d15445712931ee0998f94caa",
      "L1TxOrigin": "0x36bde71c97b33cc4729cf772ae268934f7ab70b2",
      "tokenAddress": "ETH",
      "tokenSentFrom": "",
      "tokenSentTo": "0x80f3950a4d371c43360f292a4170624abd9eed03",
      "tokenValue": "1195403000000000"
    }
  ]
}

```

---

# Get Label Master List

**Source:** https://docs.etherscan.io/api-reference/endpoint/getlabelmasterlist

---

This is a PRO endpoint, available to the [Enterprise tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-module)

module

string

default:"nametag"

Set to `nametag` for this endpoint.

[​

](#param-action)

action

string

default:"getlabelmasterlist"

Set to `getlabelmasterlist` for this endpoint.

[​

](#param-format)

format

string

default:"zip"

The format of the response. Can be either `zip` or `csv`. Note that the labels `binance`, `deposit-address`, and `all` must use `zip` format.

Response

Copy

Ask AI

```
{
   "status":"1",
   "message":"OK",
   "result":[
      {
         "labelname":"Axelar",
         "labelslug":"axelar",
         "shortdescription":"Axelar delivers secure cross-chain communication for Web3. Its infrastructure enables dApp users to interact with any asset or application, on any chain, with one click. Axelar is an overlay network, delivering Turing-complete message passing via proof-of-stake and permissionless protocols.",
         "notes":"",
         "lastupdatedtimestamp":1712897117
      },
      {
         "labelname":"Binance",
         "labelslug":"binance",
         "shortdescription":"Binance is one of the world’s leading blockchain ecosystems, with a product suite that includes the largest digital asset exchange.",
         "notes":"",
         "lastupdatedtimestamp":1759922816
      }
   ]
}

```

---

# Get Event Logs by Address

**Source:** https://docs.etherscan.io/api-reference/endpoint/getlogs

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"logs"

Set to `logs` for this endpoint.

[​

](#param-action)

action

string

default:"getLogs"

Set to `getLogs` for this endpoint.

[​

](#param-address)

address

string

default:"0xbd3531da5cf5857e7cfaa92426877b022e612cf8"

Address to check for logs.

[​

](#param-from-block)

fromBlock

integer

default:"12878196"

Starting block number to search from.

[​

](#param-to-block)

toBlock

integer

default:"12878196"

Ending block number to search to.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"1000"

Number of records per page. Limited to 1000 records per query; use the `page` parameter for subsequent records.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=logs&action=getLogs&address=0xbd3531da5cf5857e7cfaa92426877b022e612cf8&fromBlock=12878196&toBlock=12878196&page=1&offset=1000&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "address": "0xbd3531da5cf5857e7cfaa92426877b022e612cf8",
      "topics": [
        "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
        "0x0000000000000000000000000000000000000000000000000000000000000000",
        "0x000000000000000000000000c45a4b3b698f21f88687548e7f5a80df8b99d93d",
        "0x00000000000000000000000000000000000000000000000000000000000000b5"
      ],
      "data": "0x",
      "blockNumber": "0xc48174",
      "blockHash": "0x837e109ab8b1b40ec7d1032bff82397325d85e719b97d900fa0d9aa9745b2c27",
      "timeStamp": "0x60f9ce56",
      "gasPrice": "0x2e90edd000",
      "gasUsed": "0x247205",
      "logIndex": "0x",
      "transactionHash": "0x4ffd22d986913d33927a392fe4319bcd2b62f3afe1c15a2c59f77fc2cc4c20a9",
      "transactionIndex": "0x"
    },
    {
      "address": "0xbd3531da5cf5857e7cfaa92426877b022e612cf8",
      "topics": [
        "0x645f26e653c951cec836533f8fe0616d301c20a17153debc17d7c3dbe4f32b28",
        "0x00000000000000000000000000000000000000000000000000000000000000b5"
      ],
      "data": "0x",
      "blockNumber": "0xc48174",
      "blockHash": "0x837e109ab8b1b40ec7d1032bff82397325d85e719b97d900fa0d9aa9745b2c27",
      "timeStamp": "0x60f9ce56",
      "gasPrice": "0x2e90edd000",
      "gasUsed": "0x247205",
      "logIndex": "0x1",
      "transactionHash": "0x4ffd22d986913d33927a392fe4319bcd2b62f3afe1c15a2c59f77fc2cc4c20a9",
      "transactionIndex": "0x"
    }
  ]
}

```

---

# Get Event Logs by Address and Topics

**Source:** https://docs.etherscan.io/api-reference/endpoint/getlogs-address-topics

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"logs"

Set to `logs` for this endpoint.

[​

](#param-action)

action

string

default:"getLogs"

Set to `getLogs` for this endpoint.

[​

](#param-from-block)

fromBlock

integer

default:"15073139"

Starting block number to search from.

[​

](#param-to-block)

toBlock

integer

default:"15074139"

Ending block number to search to.

[​

](#param-address)

address

string

default:"0x59728544b08ab483533076417fbbb2fd0b17ce3a"

Address to check for logs.

[​

](#param-topic0)

topic0

string

First topic to filter by, such as an event signature.

[​

](#param-topic0-1-opr)

topic0_1_opr

string

default:"and"

Topic operator between `topic0` and `topic1`, either `and` or `or`.

[​

](#param-topic1)

topic1

string

Second topic to filter by.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"1000"

Number of records per page. Limited to 1000 records per query; use the `page` parameter for subsequent records.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=logs&action=getLogs&fromBlock=15073139&toBlock=15074139&address=0x59728544b08ab483533076417fbbb2fd0b17ce3a&topic0=0x27c4f0403323142b599832f26acd21c74a9e5b809f2215726e244a4ac588cd7d&topic0_1_opr=and&topic1=0x00000000000000000000000023581767a106ae21c074b2276d25e5c3e136a68b&page=1&offset=1000&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "address": "0x59728544b08ab483533076417fbbb2fd0b17ce3a",
      "topics": [
        "0x27c4f0403323142b599832f26acd21c74a9e5b809f2215726e244a4ac588cd7d",
        "0x00000000000000000000000023581767a106ae21c074b2276d25e5c3e136a68b",
        "0x000000000000000000000000000000000000000000000000000000000000236d",
        "0x000000000000000000000000c8a5592031f93debea5d9e67a396944ee01bb2ca"
      ],
      "data": "0x000000000000000000000000c02aaa39b223fe8d0a0e5c4f27ead9083c756cc20000000000000000000000000000000000000000000000000f207539952d0000",
      "blockNumber": "0xe60262",
      "blockHash": "0xb40d77b4ffba5ae2a38cbc87a65a6c9b56f9af5d8bf320aa1f1b6af00b850778",
      "timeStamp": "0x62c26caf",
      "gasPrice": "0x5e2d742c9",
      "gasUsed": "0xfb7f8",
      "logIndex": "0x4b",
      "transactionHash": "0x26fe1a0a403fd44ef11ee72f3b4ceff590b6ea533684cb279cb4242be463304c",
      "transactionIndex": "0x39"
    },
    {
      "address": "0x59728544b08ab483533076417fbbb2fd0b17ce3a",
      "topics": [
        "0x27c4f0403323142b599832f26acd21c74a9e5b809f2215726e244a4ac588cd7d",
        "0x00000000000000000000000023581767a106ae21c074b2276d25e5c3e136a68b",
        "0x0000000000000000000000000000000000000000000000000000000000002261",
        "0x000000000000000000000000c8a5592031f93debea5d9e67a396944ee01bb2ca"
      ],
      "data": "0x000000000000000000000000c02aaa39b223fe8d0a0e5c4f27ead9083c756cc20000000000000000000000000000000000000000000000000de0b6b3a7640000",
      "blockNumber": "0xe6035b",
      "blockHash": "0x5a46aeca5eaf8af1fbf56439b12dfea8fb27d18ca31020cc723271e119cffc04",
      "timeStamp": "0x62c27ab1",
      "gasPrice": "0x27e523173",
      "gasUsed": "0x3b86e",
      "logIndex": "0x1d7",
      "transactionHash": "0x3a299413cf2c91e376e542efcf3fc308c562da79af6e992401217cc6208c7f74",
      "transactionIndex": "0x92"
    }
  ]
}

```

---

# Get Event Logs by Topics

**Source:** https://docs.etherscan.io/api-reference/endpoint/getlogs-topics

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"logs"

Set to `logs` for this endpoint.

[​

](#param-action)

action

string

default:"getLogs"

Set to `getLogs` for this endpoint.

[​

](#param-from-block)

fromBlock

integer

default:"12878196"

Starting block number to search from.

[​

](#param-to-block)

toBlock

integer

default:"12879196"

Ending block number to search to.

[​

](#param-topic0)

topic0

string

First topic to filter by, such as an event signature.

[​

](#param-topic0-1-opr)

topic0_1_opr

string

default:"and"

Topic operator between `topic0` and `topic1`, either `and` or `or`.

[​

](#param-topic1)

topic1

string

Second topic to filter by.

[​

](#param-topic1-2-opr)

topic1_2_opr

string

default:"and"

Topic operator between `topic1` and `topic2`, either `and` or `or`.

[​

](#param-topic2)

topic2

string

Third topic to filter by.

[​

](#param-topic2-3-opr)

topic2_3_opr

string

default:"and"

Topic operator between `topic2` and `topic3`, either `and` or `or`.

[​

](#param-topic3)

topic3

string

Fourth topic to filter by.

[​

](#param-topic0-2-opr)

topic0_2_opr

string

default:"and"

Topic operator between `topic0` and `topic2`, either `and` or `or`.

[​

](#param-topic0-3-opr)

topic0_3_opr

string

default:"and"

Topic operator between `topic0` and `topic3`, either `and` or `or`.

[​

](#param-topic1-3-opr)

topic1_3_opr

string

default:"and"

Topic operator between `topic1` and `topic3`, either `and` or `or`.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"1000"

Number of records per page. Limited to 1000 records per query; use the `page` parameter for subsequent records.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=logs&action=getLogs&fromBlock=12878196&toBlock=12879196&topic0=0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef&topic0_1_opr=and&topic1=0x0000000000000000000000000000000000000000000000000000000000000000&topic1_2_opr=and&topic2=0x000000000000000000000000c45a4b3b698f21f88687548e7f5a80df8b99d93d&topic2_3_opr=and&topic3=0x00000000000000000000000000000000000000000000000000000000000000b5&topic0_2_opr=and&topic0_3_opr=and&topic1_3_opr=and&page=1&offset=1000&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "address": "0xbd3531da5cf5857e7cfaa92426877b022e612cf8",
      "topics": [
        "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
        "0x0000000000000000000000000000000000000000000000000000000000000000",
        "0x000000000000000000000000c45a4b3b698f21f88687548e7f5a80df8b99d93d",
        "0x00000000000000000000000000000000000000000000000000000000000000b5"
      ],
      "data": "0x",
      "blockNumber": "0xc48174",
      "blockHash": "0x837e109ab8b1b40ec7d1032bff82397325d85e719b97d900fa0d9aa9745b2c27",
      "timeStamp": "0x60f9ce56",
      "gasPrice": "0x2e90edd000",
      "gasUsed": "0x247205",
      "logIndex": "0x",
      "transactionHash": "0x4ffd22d986913d33927a392fe4319bcd2b62f3afe1c15a2c59f77fc2cc4c20a9",
      "transactionIndex": "0x"
    }
  ]
}

```

---

# Get Blocks Validated by Address

**Source:** https://docs.etherscan.io/api-reference/endpoint/getminedblocks

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint.

[​

](#param-action)

action

string

default:"getminedblocks"

Set to `getminedblocks` for this endpoint.

[​

](#param-address)

address

string

default:"0x9dd134d14d1e65f84b706d6f205cd5b1cd03a46b"

Validator The address to query, like `0xfefefefefefefefefefefefefefefefefefefefe`.

[​

](#param-blocktype)

blocktype

string

default:"blocks"

Use `blocks` for full blocks, optionally `uncles` for [pre-Merge](https://ethereum.org/en/roadmap/merge/) blocks.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"1"

Number of records per page.

Response

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "blockNumber": "20226140",
      "timeStamp": "1720011623",
      "blockReward": "33424103001400554"
    }
  ]
}

```

---

# Get Contract Source Code

**Source:** https://docs.etherscan.io/api-reference/endpoint/getsourcecode

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"contract"

Set to `contract` for this endpoint.

[​

](#param-action)

action

string

default:"getsourcecode"

Set to `getsourcecode` for this endpoint.

[​

](#param-address)

address

string

default:"0xBB9bc244D798123fDe783fCc1C72d3Bb8C189413"

The contract address to query.

Response

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "SourceCode": "pragma solidity 0.4.26;\r\n\r\ncontract Test12345 {\r\n    string public test;\r\n    \r\n    function enterValue(string _c) {\r\n        test = _c;\r\n    }\r\n}",
      "ABI": "[{\"constant\":false,\"inputs\":[{\"name\":\"_c\",\"type\":\"string\"}],\"name\":\"enterValue\",\"outputs\":[],\"payable\":false,\"stateMutability\":\"nonpayable\",\"type\":\"function\"},{\"constant\":true,\"inputs\":[],\"name\":\"test\",\"outputs\":[{\"name\":\"\",\"type\":\"string\"}],\"payable\":false,\"stateMutability\":\"view\",\"type\":\"function\"}]",
      "ContractName": "Test12345",
      "CompilerVersion": "v0.4.26+commit.4563c3fc",
      "CompilerType": "solc",
      "OptimizationUsed": "1",
      "Runs": "200",
      "ConstructorArguments": "",
      "EVMVersion": "Default",
      "Library": "",
      "ContractFileName": "",
      "LicenseType": "None",
      "Proxy": "0",
      "Implementation": "",
      "SwarmSource": "bzzr://f6b932198e10e83a6c872406a4252cf5eea48f37bac9a33085eba887820370cf",
      "SimilarMatch": "0x60810f4d8a618edb533a168e790ab6c09b0e7707"
    }
  ]
}

```

---

# Check Contract Execution Status

**Source:** https://docs.etherscan.io/api-reference/endpoint/getstatus

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"transaction"

Set to `transaction` for this endpoint.

[​

](#param-action)

action

string

default:"getstatus"

Set to `getstatus` for this endpoint.

[​

](#param-txhash)

txhash

string

Transaction hash to check the execution status.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=transaction&action=getstatus&txhash=0x15f8e5ea1079d9a0bb04a4c58ae5fe7654b5b2b4463375ff7ffb490aa0032f3a&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": {
    "isError": "1",
    "errDescription": "Bad jump destination"
  }
}

```

---

# Check Transaction Receipt Status

**Source:** https://docs.etherscan.io/api-reference/endpoint/gettxreceiptstatus

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"transaction"

Set to `transaction` for this endpoint.

[​

](#param-action)

action

string

default:"gettxreceiptstatus"

Set to `gettxreceiptstatus` for this endpoint.

[​

](#param-txhash)

txhash

string

Transaction hash to check the receipt status.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=transaction&action=gettxreceiptstatus&txhash=0x513c1ba0bebf66436b5fed86ab668452b7805593c05073eb2d51d3a52f480a76&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": {
    "status": "1"
  }
}

```

---

# Get Withdrawal Transactions by Address

**Source:** https://docs.etherscan.io/api-reference/endpoint/getwithdrawaltxs

---

###

[​

](#query-parameters)
Query Parameters

This endpoint is only available for the Arbitrum and Optimism stack chains.

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"10"

Chain ID to query, eg `10` for Optimism or `42161` for Arbitrum from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint.

[​

](#param-action)

action

string

default:"getwithdrawaltxs"

Set to `getwithdrawaltxs` for this endpoint.

[​

](#param-address)

address

string

default:"0x80f3950a4d371c43360f292a4170624abd9eed03"

Address to check for cross-chain withdrawals from L2 to Ethereum.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"10"

Number of records per page.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest transactions first or `asc` for the oldest transactions first.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=10&module=account&action=getwithdrawaltxs&address=0x80f3950a4d371c43360f292a4170624abd9eed03&page=1&offset=10&sort=desc&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "0",
  "message": "No transactions found",
  "result": []
}

```

---

# Get Total Nodes Count

**Source:** https://docs.etherscan.io/api-reference/endpoint/nodecount

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"nodecount"

Set to `nodecount` for this endpoint.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=nodecount&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": {
    "UTCDate": "2025-09-25",
    "TotalNodeCount": "11786"
  }
}

```

---

# Get ERC1155 Token Transfers by Address

**Source:** https://docs.etherscan.io/api-reference/endpoint/token1155tx

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint.

[​

](#param-action)

action

string

default:"token1155tx"

Set to `token1155tx` for this endpoint.

[​

](#param-contractaddress)

contractaddress

string

default:"0x76be3b62873462d2142405439777e971754e8e77"

The ERC1155 token contract address to filter transfers by, eg `0x495f947276749ce646f68ac8c248420045cb7b5e` for [Opensea Shared Storefront](https://etherscan.io/token/0x495f947276749ce646f68ac8c248420045cb7b5e).

[​

](#param-address)

address

string

default:"0x83f564d180b58ad9a02a449105568189ee7de8cb"

The address to query, like `0xfefefefefefefefefefefefefefefefefefefefe`

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"1"

Number of transactions per page.

[​

](#param-startblock)

startblock

integer

default:"0"

Starting block number to search from.

[​

](#param-endblock)

endblock

integer

default:"9999999999"

Ending block number to search to.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest transactions first or `asc` for the oldest transactions first.

Response

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "blockNumber": "13472395",
      "timeStamp": "1634973285",
      "hash": "0x643b15f3ffaad5d38e33e5872b4ebaa7a643eda8b50ffd5331f682934ee65d4d",
      "nonce": "41",
      "blockHash": "0xa5da536dfbe8125eb146114e2ee0d0bdef2b20483aacbf30fed6b60f092059e6",
      "transactionIndex": "100",
      "gas": "140000",
      "gasPrice": "52898577246",
      "gasUsed": "105030",
      "cumulativeGasUsed": "11739203",
      "input": "deprecated",
      "methodId": "0x3e6b214b",
      "functionName": "",
      "contractAddress": "0x76be3b62873462d2142405439777e971754e8e77",
      "from": "0x1e63326a84d2fa207bdfa856da9278a93deba418",
      "to": "0x83f564d180b58ad9a02a449105568189ee7de8cb",
      "tokenID": "10371",
      "tokenValue": "1",
      "tokenName": "parallel",
      "tokenSymbol": "LL",
      "confirmations": "9995266"
    }
  ]
}

```

---

# Get ERC20-Token Account Balance for TokenContractAddress

**Source:** https://docs.etherscan.io/api-reference/endpoint/tokenbalance

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint.

[​

](#param-action)

action

string

default:"tokenbalance"

Set to `tokenbalance` for this endpoint.

[​

](#param-contractaddress)

contractaddress

string

default:"0x57d90b64a1a57749b0f932f1a3395792e12e7055"

Contract address of the ERC-20 token.

[​

](#param-address)

address

string

default:"0xe04f27eb70e025b78871a2ad7eabe85e61212761"

Address to check for token balance.

[​

](#param-tag)

tag

string

default:"latest"

Use `latest` for the last block number of the chain.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=account&action=tokenbalance&contractaddress=0x57d90b64a1a57749b0f932f1a3395792e12e7055&address=0xe04f27eb70e025b78871a2ad7eabe85e61212761&tag=latest&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": "135499"
}

```

---

# Get Historical ERC20-Token Account Balance by BlockNo

**Source:** https://docs.etherscan.io/api-reference/endpoint/tokenbalancehistory

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

This [historical endpoint](/resources/rate-limits) is throttled to 2 calls/s

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint.

[​

](#param-action)

action

string

default:"tokenbalancehistory"

Set to `tokenbalancehistory` for this endpoint.

[​

](#param-contractaddress)

contractaddress

string

default:"0x57d90b64a1a57749b0f932f1a3395792e12e7055"

Contract address of the ERC-20 token.

[​

](#param-address)

address

string

default:"0xe04f27eb70e025b78871a2ad7eabe85e61212761"

Address to check for balance.

[​

](#param-blockno)

blockno

integer

default:"8000000"

Block number to check balance for.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=account&action=tokenbalancehistory&contractaddress=0x57d90b64a1a57749b0f932f1a3395792e12e7055&address=0xe04f27eb70e025b78871a2ad7eabe85e61212761&blockno=8000000&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": "135499"
}

```

---

# Get Token Holder Count by Contract Address

**Source:** https://docs.etherscan.io/api-reference/endpoint/tokenholdercount

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"token"

Set to `token` for this endpoint.

[​

](#param-action)

action

string

default:"tokenholdercount"

Set to `tokenholdercount` for this endpoint.

[​

](#param-contractaddress)

contractaddress

string

default:"0xaaaebe6fe48e54f431b0c390cfaf0b017d09d42d"

Contract address of the ERC-20 token.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=token&action=tokenholdercount&contractaddress=0xaaaebe6fe48e54f431b0c390cfaf0b017d09d42d&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": "30506"
}

```

---

# Get Token Holder List by Contract Address

**Source:** https://docs.etherscan.io/api-reference/endpoint/tokenholderlist

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"token"

Set to `token` for this endpoint.

[​

](#param-action)

action

string

default:"tokenholderlist"

Set to `tokenholderlist` for this endpoint.

[​

](#param-contractaddress)

contractaddress

string

default:"0xaaaebe6fe48e54f431b0c390cfaf0b017d09d42d"

Contract address of the ERC-20 token.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"10"

Number of records per page.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=token&action=tokenholderlist&contractaddress=0xaaaebe6fe48e54f431b0c390cfaf0b017d09d42d&page=1&offset=10&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "TokenHolderAddress": "0xa5b7d615c99f011a22f16f5809890ca6900200a3",
      "TokenHolderQuantity": "2"
    },
    {
      "TokenHolderAddress": "0x0412a1d25fbdcabc536603198330021ccb13240b",
      "TokenHolderQuantity": "3385700"
    }
  ]
}

```

---

# Get Token Info by ContractAddress

**Source:** https://docs.etherscan.io/api-reference/endpoint/tokeninfo

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

This [historical endpoint](/resources/rate-limits) is throttled to 2 calls/s

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"token"

Set to `token` for this endpoint.

[​

](#param-action)

action

string

default:"tokeninfo"

Set to `tokeninfo` for this endpoint.

[​

](#param-contractaddress)

contractaddress

string

default:"0x0e3a2a1f2146d86a604adc220b4967a898d7fe07"

Contract address of the token to retrieve info for.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=token&action=tokeninfo&contractaddress=0x0e3a2a1f2146d86a604adc220b4967a898d7fe07&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "contractAddress": "0x0e3a2a1f2146d86a604adc220b4967a898d7fe07",
      "tokenName": "Gods Unchained Cards",
      "symbol": "CARD",
      "divisor": "0",
      "tokenType": "ERC721",
      "totalSupply": "6972003",
      "blueCheckmark": "true",
      "description": "A TCG on the Ethereum blockchain that uses NFT's to bring real ownership to in-game assets.",
      "website": "https://godsunchained.com/",
      "email": "",
      "blog": "https://medium.com/@fuelgames",
      "reddit": "https://www.reddit.com/r/GodsUnchained/",
      "slack": "",
      "facebook": "https://www.facebook.com/godsunchained/",
      "twitter": "https://twitter.com/godsunchained",
      "bitcointalk": "",
      "github": "",
      "telegram": "",
      "wechat": "",
      "linkedin": "",
      "discord": "https://discordapp.com/invite/DKGr2pW",
      "whitepaper": "",
      "tokenPriceUSD": "0.000000000000000000",
      "image": ""
    }
  ]
}

```

---

# Get ERC721 Token Transfers by Address

**Source:** https://docs.etherscan.io/api-reference/endpoint/tokennfttx

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint.

[​

](#param-action)

action

string

default:"tokennfttx"

Set to `tokennfttx` for this endpoint.

[​

](#param-contractaddress)

contractaddress

string

default:"0x06012c8cf97bead5deae237070f9587f8e7a266d"

The ERC721 token contract address to filter transfers by, eg `0xbd3531da5cf5857e7cfaa92426877b022e612cf8` for [Pudgy Penguins](https://etherscan.io/token/0xbd3531da5cf5857e7cfaa92426877b022e612cf8).

[​

](#param-address)

address

string

default:"0x6975be450864c02b4613023c2152ee0743572325"

Address to filter token transfers by.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"1"

Number of transactions per page.

[​

](#param-startblock)

startblock

integer

default:"0"

Starting block number to search from.

[​

](#param-endblock)

endblock

integer

default:"9999999999"

Ending block number to search to.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest transactions first or `asc` for the oldest transactions first.

Response

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "blockNumber": "4708120",
      "timeStamp": "1512907118",
      "hash": "0x031e6968a8de362e4328d60dcc7f72f0d6fc84284c452f63176632177146de66",
      "nonce": "0",
      "blockHash": "0x4be19c278bfaead5cb0bc9476fa632e2447f6e6259e0303af210302d22779a24",
      "from": "0xb1690c08e213a35ed9bab7b318de14420fb57d8c",
      "contractAddress": "0x06012c8cf97bead5deae237070f9587f8e7a266d",
      "to": "0x6975be450864c02b4613023c2152ee0743572325",
      "tokenID": "202106",
      "tokenName": "CryptoKitties",
      "tokenSymbol": "CK",
      "tokenDecimal": "0",
      "transactionIndex": "81",
      "gas": "158820",
      "gasPrice": "40000000000",
      "gasUsed": "60508",
      "cumulativeGasUsed": "4880352",
      "input": "deprecated",
      "methodId": "0x454a2ab3",
      "functionName": "bid(uint256 _tokenId)",
      "confirmations": "18759540"
    }
  ]
}

```

---

# Get ERC20-Token TotalSupply by ContractAddress

**Source:** https://docs.etherscan.io/api-reference/endpoint/tokensupply

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"tokensupply"

Set to `tokensupply` for this endpoint.

[​

](#param-contractaddress)

contractaddress

string

default:"0x57d90b64a1a57749b0f932f1a3395792e12e7055"

Contract address of the ERC-20 token.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=tokensupply&contractaddress=0x57d90b64a1a57749b0f932f1a3395792e12e7055&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": "21265524714464"
}

```

---

# Get Historical ERC20-Token TotalSupply by ContractAddress & BlockNo

**Source:** https://docs.etherscan.io/api-reference/endpoint/tokensupplyhistory

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

This [historical endpoint](/resources/rate-limits) is throttled to 2 calls/s

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"stats"

Set to `stats` for this endpoint.

[​

](#param-action)

action

string

default:"tokensupplyhistory"

Set to `tokensupplyhistory` for this endpoint.

[​

](#param-contractaddress)

contractaddress

string

default:"0x57d90b64a1a57749b0f932f1a3395792e12e7055"

Contract address of the ERC-20 token.

[​

](#param-blockno)

blockno

integer

default:"8000000"

Block number to check total supply for.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=stats&action=tokensupplyhistory&contractaddress=0x57d90b64a1a57749b0f932f1a3395792e12e7055&blockno=8000000&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": "21265524714464"
}

```

---

# Get ERC20 Token Transfers by Address

**Source:** https://docs.etherscan.io/api-reference/endpoint/tokentx

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint

[​

](#param-action)

action

string

default:"tokentx"

Set to `tokentx` for this endpoint

[​

](#param-contractaddress)

contractaddress

string

default:"0x9f8f72aa9304c8b593d555f12ef6589cc3a579a2"

The ERC20 token contract address to filter transfers by, eg `0xdac17f958d2ee523a2206206994597c13d831ec7` for USDT.

[​

](#param-address)

address

string

default:"0x4e83362442b8d1bec281594cea3050c8eb01311c"

The address to query, like `0xfefefefefefefefefefefefefefefefefefefefe`

[​

](#param-startblock)

startblock

integer

default:"0"

Starting block number to search from.

[​

](#param-endblock)

endblock

integer

default:"9999999999"

Ending block number to search to.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"1"

Number of transactions per page.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest transactions first or `asc` for the oldest transactions first.

Response

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "blockNumber": "4730207",
      "timeStamp": "1513240363",
      "hash": "0xe8c208398bd5ae8e4c237658580db56a2a94dfa0ca382c99b776fa6e7d31d5b4",
      "nonce": "406",
      "blockHash": "0x022c5e6a3d2487a8ccf8946a2ffb74938bf8e5c8a3f6d91b41c56378a96b5c37",
      "from": "0x642ae78fafbb8032da552d619ad43f1d81e4dd7c",
      "contractAddress": "0x9f8f72aa9304c8b593d555f12ef6589cc3a579a2",
      "to": "0x4e83362442b8d1bec281594cea3050c8eb01311c",
      "value": "5901522149285533025181",
      "tokenName": "Maker",
      "tokenSymbol": "MKR",
      "tokenDecimal": "18",
      "transactionIndex": "81",
      "gas": "940000",
      "gasPrice": "32010000000",
      "gasUsed": "77759",
      "cumulativeGasUsed": "2523379",
      "input": "deprecated",
      "methodId": "0xbe040fb0",
      "functionName": "redeem()",
      "confirmations": "18737452"
    }
  ]
}

```

---

# Get Top Token Holders

**Source:** https://docs.etherscan.io/api-reference/endpoint/toptokenholders

---

This is a PRO endpoint, available to any [paid tier](https://etherscan.io/apis)

This [historical endpoint](/resources/rate-limits) is throttled to 2 calls/s

HyperEVM (999) is not supported yet

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"token"

Set to `token` for this endpoint.

[​

](#param-action)

action

string

default:"topholders"

Set to `topholders` for this endpoint.

[​

](#param-contractaddress)

contractaddress

string

default:"0x7fc66500c84a76ad7e9c93437bfc5ac33e2ddae9"

Contract address of the ERC-20 token.

[​

](#param-offset)

offset

integer

default:"100"

Number of top holders to return, up to 1000.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=1&module=token&action=topholders&contractaddress=0x7fc66500c84a76ad7e9c93437bfc5ac33e2ddae9&offset=100&apikey=YourApiKeyToken"

```

Response

Copy

Ask AI

```
{
  "status": "1",
  "message": "Ok",
  "result": [
    {
      "TokenHolderAddress": "0x4da27a545c0c5b758a6ba100e3a049001de870f5",
      "TokenHolderQuantity": "2696124.3026660371030000",
      "TokenHolderAddressType": "C"
    },
    {
      "TokenHolderAddress": "0xa700b4eb416be35b2911fd5dee80678ff64ff6c9",
      "TokenHolderQuantity": "1650828.8050095955930000",
      "TokenHolderAddressType": "C"
    }
  ]
}

```

---

# Get Normal Transactions By Address

**Source:** https://docs.etherscan.io/api-reference/endpoint/txlist

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint

[​

](#param-action)

action

string

default:"txlist"

Set to `txlist` for this endpoint

[​

](#param-address)

address

string

default:"0xc5102fE9359FD9a28f877a67E36B0F050d81a3CC"

The address to query, like `0xfefefefefefefefefefefefefefefefefefefefe`.

[​

](#param-startblock)

startblock

integer

default:"0"

Starting block number to search from.

[​

](#param-endblock)

endblock

integer

default:"9999999999"

Ending block number to search to.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"1"

Number of transactions per page.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest transactions first or `asc` for the oldest transactions first.

Response

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "blockNumber": "23467053",
      "blockHash": "0xf5646226f819fbdd6b2f1cb99de6e5d17da3d876ec166e69f4e9736c8ebf7ab4",
      "timeStamp": "1759129619",
      "hash": "0xf9db905d77704596d3600816bc70201586cfeec13bcf576320e2f38d6ca851a0",
      "nonce": "8",
      "transactionIndex": "184",
      "from": "0x2449ecef5012f0a0e153b278ef4fcc9625bc4c78",
      "to": "0xc5102fe9359fd9a28f877a67e36b0f050d81a3cc",
      "value": "0",
      "gas": "73271",
      "gasPrice": "238744402",
      "input": "0x5c19a95c0000000000000000000000002449ecef5012f0a0e153b278ef4fcc9625bc4c78",
      "methodId": "0x5c19a95c",
      "functionName": "delegate(address to)",
      "contractAddress": "",
      "cumulativeGasUsed": "22498564",
      "txreceipt_status": "1",
      "gasUsed": "48847",
      "confirmations": "589",
      "isError": "0"
    }
  ]
}

```

---

# Get Internal Transactions by Address

**Source:** https://docs.etherscan.io/api-reference/endpoint/txlistinternal

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint

[​

](#param-action)

action

string

default:"txlistinternal"

Set to `txlistinternal` for this endpoint.

[​

](#param-address)

address

string

default:"0x2c1ba59d6f58433fb1eaee7d20b26ed83bda51a3"

The address to query, like `0xfefefefefefefefefefefefefefefefefefefefe`.

[​

](#param-startblock)

startblock

integer

default:"0"

Starting block number to search from.

[​

](#param-endblock)

endblock

integer

default:"9999999999"

Ending block number to search to.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"1"

Number of transactions per page.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest transactions first or `asc` for the oldest transactions first.

Response

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "blockNumber": "2535368",
      "timeStamp": "1477837690",
      "hash": "0x8a1a9989bda84f80143181a68bc137ecefa64d0d4ebde45dd94fc0cf49e70cb6",
      "from": "0x20d42f2e99a421147acf198d775395cac2e8b03d",
      "to": "",
      "value": "0",
      "contractAddress": "0x2c1ba59d6f58433fb1eaee7d20b26ed83bda51a3",
      "input": "",
      "type": "create",
      "gas": "254791",
      "gasUsed": "46750",
      "traceId": "0",
      "isError": "0",
      "errCode": ""
    }
  ]
}

```

---

# Get Internal Transactions by Block Range

**Source:** https://docs.etherscan.io/api-reference/endpoint/txlistinternal-blockrange

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint.

[​

](#param-action)

action

string

default:"txlistinternal"

Set to `txlistinternal` for this endpoint.

[​

](#param-startblock)

startblock

integer

default:"13481773"

Starting block number to search from.

[​

](#param-endblock)

endblock

integer

default:"9999999999"

Ending block number to search to.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"1"

Number of transactions per page.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest transactions first or `asc` for the oldest transactions first.

Response

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "blockNumber": "13481773",
      "timeStamp": "1635100060",
      "hash": "0x8b440f5ec0e986589517b131c5b75e921f2768b9912f028d2cdb009d759036ab",
      "from": "0x3909336de913344701c6f096502d26208210b39f",
      "to": "0xff62dfadca3b5643d0b283571fe154d886580c0c",
      "value": "1159078546481168231",
      "contractAddress": "",
      "input": "",
      "type": "call",
      "gas": "101300",
      "gasUsed": "13898",
      "traceId": "3",
      "isError": "0",
      "errCode": ""
    }
  ]
}

```

---

# Get Internal Transactions by Transaction Hash

**Source:** https://docs.etherscan.io/api-reference/endpoint/txlistinternal-txhash

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint

[​

](#param-action)

action

string

default:"txlistinternal"

Set to `txlistinternal` for this endpoint.

[​

](#param-txhash)

txhash

string

Transaction hash to check for internal transactions, like `0x36dc7f05085d0e4f9c3e4116345a2a487ac8f23f7e71bcc0ae20e27abbfa931d`. Only non-zero value internal transactions are returned.

Response

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "blockNumber": "1743059",
      "timeStamp": "1466489498",
      "from": "0x2cac6e4b11d6b58f6d3c1c9d5fe8faa89f60e5a2",
      "to": "0x66a1c3eaf0f1ffc28d209c0763ed0ca614f3b002",
      "value": "7106740000000000",
      "contractAddress": "",
      "input": "",
      "type": "call",
      "gas": "2300",
      "gasUsed": "0",
      "isError": "0",
      "errCode": ""
    }
  ]
}

```

---

# Get Plasma Deposits by Address

**Source:** https://docs.etherscan.io/api-reference/endpoint/txnbridge

---

This endpoint is only available for Polygon (137), Xdai (100) and BTTC(199)

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"137"

Chain ID to query, eg `137` for Polygon from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint.

[​

](#param-action)

action

string

default:"txnbridge"

Set to `txnbridge` for this endpoint.

[​

](#param-address)

address

string

default:"0x4880bd4695a8e59dc527d124085749744b6c988e"

Address to check for Plasma deposits.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"10"

Number of records per page.

Copy

Ask AI

```
curl "https://api.etherscan.io/v2/api?chainid=137&module=account&action=txnbridge&address=0x4880bd4695a8e59dc527d124085749744b6c988e&page=1&offset=10&apikey=YourApiKeyToken"

```

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "hash": "0xf645deb2b6fbb8b76ccbcf4bde782e28d3520e8a30e9a568b9b8c526e2fd8434",
      "blockNumber": "51844560",
      "timeStamp": "1704181285",
      "from": "0x0000000000000000000000000000000000000000",
      "address": "0x4880bd4695a8e59dc527d124085749744b6c988e",
      "amount": "2341706540000000000",
      "tokenName": "Polygon Token",
      "symbol": "POL",
      "contractAddress": "0x0000000000000000000000000000000000001010",
      "divisor": "18"
    }
  ]
}

```

---

# Get Beacon Chain Withdrawals by Address

**Source:** https://docs.etherscan.io/api-reference/endpoint/txsbeaconwithdrawal

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"account"

Set to `account` for this endpoint.

[​

](#param-action)

action

string

default:"txsBeaconWithdrawal"

Set to `txsBeaconWithdrawal` for this endpoint.

[​

](#param-address)

address

string

default:"0xB9D7934878B5FB9610B3fE8A5e441e8fad7E293f"

Address to check for beacon withdrawals.

[​

](#param-startblock)

startblock

integer

default:"0"

Starting block number to search from.

[​

](#param-endblock)

endblock

integer

default:"9999999999"

Ending block number to search to.

[​

](#param-page)

page

integer

default:"1"

Page number for pagination.

[​

](#param-offset)

offset

integer

default:"1"

Number of records per page.

[​

](#param-sort)

sort

string

default:"desc"

Sort order either `desc` for the latest transactions first or `asc` for the oldest transactions first.

Response

Copy

Ask AI

```
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "withdrawalIndex": "13",
      "validatorIndex": "117823",
      "address": "0xb9d7934878b5fb9610b3fe8a5e441e8fad7e293f",
      "amount": "3402931175",
      "blockNumber": "17034877",
      "timestamp": "1681338599"
    }
  ]
}

```

---

# Verify Proxy Contract

**Source:** https://docs.etherscan.io/api-reference/endpoint/verifyproxycontract

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"contract"

Set to `contract` for this endpoint.

[​

](#param-action)

action

string

default:"verifyproxycontract"

Set to `verifyproxycontract` for this endpoint.

[​

](#param-address)

address

string

default:"0xcbdcd3815b5f975e1a2c944a9b2cd1c985a1cb7f"

The proxy contract address to verify.

[​

](#param-expectedimplementation)

expectedimplementation

string

default:"0xB0F24CEB2616F6Bb608B00875Db306167c0f2E8C"

Optional implementation address to enforce during verification.

Response

Copy

Ask AI

```
{
    "status": "1",
    "message": "OK",
    "result": "a7lpxkm9kpcpicx7daftmjifrfhiuhf5vqqnawhkfhzfrcpnxj"
}

```

---

# Verify Solidity Source Code

**Source:** https://docs.etherscan.io/api-reference/endpoint/verifysourcecode

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"contract"

Set to `contract` for this endpoint.

[​

](#param-action)

action

string

default:"verifysourcecode"

Set to `verifysourcecode` for this endpoint.

[​

](#param-contractaddress)

contractaddress

string

default:"0xBB9bc244D798123fDe783fCc1C72d3Bb8C189413"

The address where the contract is deployed.

[​

](#param-source-code)

sourceCode

string

The Solidity source code to verify.

[​

](#param-codeformat)

codeformat

string

default:"solidity-standard-json-input"

Use `solidity-single-file` for a single file or `solidity-standard-json-input` for JSON input.

[​

](#param-contractname)

contractname

string

default:"contracts/Verified.sol:Verified"

The contract name, including path if applicable. If `codeformat=solidity-standard-json-input`, then enter contractname as `erc20.sol:erc20`.

[​

](#param-compilerversion)

compilerversion

string

default:"v0.8.24+commit.e11b9ed9"

Compiler version used for compilation.

[​

](#param-optimization-used)

optimizationUsed

string

default:"0"

Use `1` if optimization was used or `0` if disabled, specify runs below.

[​

](#param-runs)

runs

string

default:"200"

Number of optimization runs.

[​

](#param-constructor-arguments)

constructorArguments

string

Optional constructor arguments used in contract deployment.

[​

](#param-evm-version)

evmVersion

string

default:"default"

Use compiler `default` or specify an EVM version such as `byzantium`, `shanghai`.

[​

](#param-license-type)

licenseType

string

default:"1"

The [open source license](https://etherscan.io/contract-license-types) to associate with the verified source code, e.g `3` for MIT.

Response

Copy

Ask AI

```
{
    "status": "1",
    "message": "OK",
    "result": "a7lpxkm9kpcpicx7daftmjifrfhiuhf5vqqnawhkfhzfrcpnxj"
}

```

---

# Verify Stylus Source Code

**Source:** https://docs.etherscan.io/api-reference/endpoint/verifystylus

---

This endpoint is only available for the Arbitrum stack chains.

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"42161"

Chain ID to query, eg `42161` for Arbitrum One from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"contract"

Set to `contract` for this endpoint.

[​

](#param-action)

action

string

default:"verifysourcecode"

Set to `verifysourcecode` for this endpoint.

[​

](#param-codeformat)

codeformat

string

default:"stylus"

Use `stylus` for Stylus projects.

[​

](#param-source-code)

sourceCode

string

Public Git repository that hosts the Stylus source code.

[​

](#param-contractaddress)

contractaddress

string

default:"0x915f0B2f34F5B5b84D1F066b398D7F0E3C2F8f83"

The address where the contract is deployed.

[​

](#param-contractname)

contractname

string

default:"stylus_hello_world"

The contract name that matches your Stylus deployment.

[​

](#param-compilerversion)

compilerversion

string

default:"stylus:0.5.3"

Stylus compiler version used for compilation.

[​

](#param-license-type)

licenseType

integer

default:"3"

License identifier from the [open source license options](https://arbiscan.io/contract-license-types).

Response

Copy

Ask AI

```
{
    "status": "1",
    "message": "OK",
    "result": "a7lpxkm9kpcpicx7daftmjifrfhiuhf5vqqnawhkfhzfrcpnxj"
}

```

---

# Verify Vyper Source Code

**Source:** https://docs.etherscan.io/api-reference/endpoint/verifyvyper

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"1"

Chain ID to query, eg `1` for Ethereum, `8453` for Base from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"contract"

Set to `contract` for this endpoint.

[​

](#param-action)

action

string

default:"verifysourcecode"

Set to `verifysourcecode` for this endpoint.

[​

](#param-codeformat)

codeformat

string

default:"vyper-json"

Use `vyper-json` for Vyper contracts.

[​

](#param-source-code)

sourceCode

string

The Vyper source code in JSON format.

[​

](#param-constructor-arguments)

constructorArguments

string

Optional constructor arguments used in contract deployment.

[​

](#param-contractaddress)

contractaddress

string

default:"0xBB9bc244D798123fDe783fCc1C72d3Bb8C189413"

The address where the contract is deployed.

[​

](#param-contractname)

contractname

string

default:"contracts/Verified.vy:Verified"

The contract name, including path if applicable.

[​

](#param-compilerversion)

compilerversion

string

default:"vyper:0.4.0"

Compiler version used for compilation.

[​

](#param-optimization-used)

optimizationUsed

string

default:"0"

Use `1` if compiler optimizations were enabled, otherwise `0`.

Response

Copy

Ask AI

```
{
    "status": "1",
    "message": "OK",
    "result": "a7lpxkm9kpcpicx7daftmjifrfhiuhf5vqqnawhkfhzfrcpnxj"
}

```

---

# Verify Source Code on zkSync

**Source:** https://docs.etherscan.io/api-reference/endpoint/verifyzksyncsourcecode

---

###

[​

](#query-parameters)
Query Parameters

[​

](#param-apikey)

apikey

string

default:"YourApiKeyToken"

Your Etherscan API key.

[​

](#param-chainid)

chainid

string

default:"324"

Chain ID to query, eg `324` for zkSync Era from our [supported chains](/supported-chains).

[​

](#param-module)

module

string

default:"contract"

Set to `contract` for this endpoint.

[​

](#param-action)

action

string

default:"verifysourcecode"

Set to `verifysourcecode` for this endpoint.

[​

](#param-codeformat)

codeformat

string

default:"solidity-standard-json-input"

Use `solidity-single-file` for a single file or `solidity-standard-json-input` for JSON input.

[​

](#param-source-code)

sourceCode

string

The Solidity source code to verify.

[​

](#param-constructor-arguments)

constructorArguments

string

Optional constructor arguments used in contract deployment.

[​

](#param-contractaddress)

contractaddress

string

default:"0xf66f984e0b73453193b452f84c8fff0ed19f6d81"

The address where the contract is deployed.

[​

](#param-contractname)

contractname

string

default:"contracts/Verified.sol:Verified"

The contract name, including path if applicable.

[​

](#param-compilerversion)

compilerversion

string

default:"v0.8.24+commit.e11b9ed9"

Compiler version used for compilation.

[​

](#param-zksolc-version)

zksolcVersion

string

default:"1.3.13"

zkSync compiler version used for compilation.

[​

](#param-compilermode)

compilermode

string

default:"zksync"

zkSync compiler mode to process the build artifacts.

---

# Common Verification Errors

**Source:** https://docs.etherscan.io/contract-verification/common-verification-errors

---

###

[​

](#contract-doesn%E2%80%99t-match)
Contract Doesn’t Match

“Compiled contract deployment bytecode does NOT match the transaction deployment bytecode”

The submitted source code does not match the contract code deployed on chain.
Common causes include using a different compiler version or enabling optimisation runs.
For an exact match to be found, both **source code** and **compiler settings** specified have to exactly match the deployment conditions, for the same bytecode to be reproduced.

###

[​

](#solidity-compilation-error)
Solidity Compilation Error

“Solidity Compilation Error: Identifier not found or not unique”

A compilation issue occured due to syntax errors in your Solidity source code.
Consider debugging your contract with any compiler such as [**Remix**](https://remix.ethereum.org/) or [**Hardhat**](https://hardhat.org/) and reference the error from Solidity’s [**official documentation**](https://docs.soliditylang.org).

###

[​

](#contract-not-deployed)
Contract Not Deployed

“Unable to locate ContractCode at 0x539a277b12a3f6723f4c1769edb11b0be7c214da”

The contract has not been deployed at the specific address at the specific chain.
Check the contract address you’ve deployed, if your contract deployment transaction has succeeded or if the [**chainId**](/supported-chains) specified is correct.

###

[​

](#missing-or-invalid-library-names)
Missing or Invalid Library Names

“Library was required but suitable match not found”

A [**library**](https://solidity-by-example.org/library/) was used in your contract deployment, but was not specified, misspelt or using the wrong library address.
Double check on your library names ( **case sensitive** such as “PRBMath” ) or ensure that a matching library name and library address is provided.

###

[​

](#missing-contract-name)
Missing Contract Name

“Unable to locate ContractName , did you specify the correct Contract Name ?”

A match was not found with the name of the contract specified when multiple files are provided.
Ensure that you have provided the correct contract name to be matched against, and making sure you submit the **main contract** name not its dependencies.

###

[​

](#no-deployment-bytecode-match-found)
No Deployment Bytecode Match Found

“Compiled contract deployment bytecode does NOT match the transaction deployment bytecode”

The compilation of your submitted source code does not match the deployment bytecode, ie the constructor arguments plus general initialisation code and runtime bytecode.
Similar solution as above, do take into account constructor arguments as well below.

###

[​

](#missing%2Finvalid-constructor-arguments)
Missing/Invalid Constructor Arguments

“Please check if the correct constructor argument was entered”

if your contract utilized the `constructor` keyword, you should provide it in hex format. Otherwise, leave this field empty as it is.
You may reference your original deployment’s constructor arguments or determine it from the [**end of your compiled bytecode**](https://info.etherscan.com/contract-verification-constructor-arguments/).

###

[​

](#mismatched-bytecode-metadata-hash)
Mismatched bytecode metadata hash

“Please check if the correct bytecodehash was specified via standard-json verification.”

The [**metadata hash**](https://docs.soliditylang.org/en/v0.8.17/metadata.html#encoding-of-the-metadata-hash-in-the-bytecode) settings of your submitted source code differs from the settings of your original contract deployment, such as being set to `ipfs` or `none`.
Submit your contract verification using the solc json input format, and [**specify the settings**](https://github.com/PaulRBerg/hardhat-template/blob/f6406c4e7c9e23d5169b39fb11d528a975b678e6/hardhat.config.ts#L104) accordingly there.
Other submission formats such as single file or multifile **do not support** changing this setting, and will use the compiler defaults.

###

[​

](#similar-match-found)
Similar Match Found

“This contract already Similar Matches the deployed ByteCode at 0x4200000000000000000000000000000000000042”

This error indicates that the contract has already been verified via [**Similar Match**](https://info.etherscan.com/types-of-contract-verification/) to another contract.
Kindly [**reach out**](https://info.etherscan.com/update-on-similar-match-contract-verification/) to us at this point of time to have this updated to Full Match if required.

###

[​

](#similar-match-api-responses)
Similar Match API Responses

Both [`getsourcecode`](/api-reference/endpoint/getsourcecode) and [`getabi`](/api-reference/endpoint/getabi) return data for contracts verified via Similar Match.

If a verified contract does not return data from these endpoints, report the issue to us.

###

[​

](#unsupported-solc-version)
Unsupported Solc Version

“Invalid or not supported solc version, see [https://etherscan.io/solcversions](https://etherscan.io/solcversions) for list”

This error is thrown when you specify to use an invalid or unsupported version of the Solidity Compiler ie. below `v0.4.11-nightly.2017.3.15+`.
Do [**let us know**](/resources/contact-us) if you need to verify a contract below this supported version such as to prove you deployed the first NFT.

###

[​

](#source-code-already-verified)
Source Code Already Verified

“Source code already verified”

An [**Exact Match**](https://info.etherscan.com/types-of-contract-verification/) has been obtained, get back to having your [**coffee**](https://media.giphy.com/media/11ISwbgCxEzMyY/giphy.gif).
If you think this might be a mistake, do check if you’ve submitted verification to the right **explorer/chain**, a contract that is verified on Etherscan is **not automatically verified** on other explorers.

###

[​

](#unsupported-file-import-callback)
Unsupported File Import Callback

“Source “@openzeppelin/contracts/ERC20.sol” not found: File import callback not supported”

This error is thrown when contracts reference imports from external sources, such as [**OpenZeppelin**](https://docs.openzeppelin.com/) libraries or Github links.
Consider [**flattening**](https://hardhat.org/hardhat-runner/docs/advanced/flattening#flattening-your-contracts) your source code into a single file, or use the Solidity Standard Json Input format that comes with tools such as [**Hardhat**](https://hardhat.org/hardhat-runner/docs/guides/verifying#verifying-your-contracts) to resolve these external imports.

###

[​

](#invalid-chainid)
Invalid chainId

The chain you’ve specified does not have an Etherscan-like explorer.
Check the chainId used against our [**supported list**](/supported-chains).

###

[​

](#temporary-error)
Temporary Error

“This could be a temporary error, please retry or contact us (Error Code 10001/10002/10003)”

Something went wrong on our end, which could include downtime or [**maintenance**](https://etherscan.freshstatus.io/) windows.
Please retry this in a while or [**ping us**](/resources/contact-us) if this continues to persist.

---

# Verify with Foundry

**Source:** https://docs.etherscan.io/contract-verification/verify-with-foundry

---

You must update to the nightly build of Foundry to use this at the moment, via `foundryup --install nightly`

[**Foundry**](https://book.getfoundry.sh/) is a tool that helps take the heat off smart contract development steps, including compiling, deploying and finally submitting your contract for verification.

##

[​

](#deploy-and-verify)
Deploy and Verify

Copy

Ask AI

```
forge create --broadcast --rpc-url https://rpc.sepolia.ethpandaops.io --private-key YourPrivateKey src/ContractFile.sol:ContractName --verify --verifier etherscan --etherscan-api-key YourApiKeyToken

```

##

[​

](#verify-an-existing-contract)
Verify an Existing Contract

Copy

Ask AI

```
forge verify-contract --watch --chain sepolia 0x324eca20b358b18e48f2611f7452560ce3b3c1bb src/ContractFile.sol:ContractName --verifier etherscan --etherscan-api-key YourApiKeyToken

```

##

[​

](#migrating-from-v1)
Migrating from V1

API keys from any other explorer (such as BscScan/Basescan/Arbiscan) have been deprecated

The easiest way to migrate is to simply pass in your [**Etherscan API key**](https://etherscan.io/apidashboard) via `--etherscan-api-key`
For deployment scripts, in `foundry.toml` you can shorten your settings to this, without needing a different key for each chain.

Copy

Ask AI

```
etherscan_api_key = "VZFDUWB3YGQ1YCDKTCU1D6DDSS6EWI62KV"

[etherscan]
mainnet   = { key = "${ETHERSCAN_API_KEY}" }

```

_This open source integration was shipped by_ [_@iainnash_](https://github.com/iainnash) _and the Foundry team_

---

# Verify with Hardhat

**Source:** https://docs.etherscan.io/contract-verification/verify-with-hardhat

---

Make sure you’ve updated to [**@nomicfoundation/hardhat-verify@^2.0.14**](https://www.npmjs.com/package/@nomicfoundation/hardhat-verify), see migrating notes if you have an existing project.

[**Hardhat**](https://hardhat.org/) is a smart contracts development tool, perfect if you’re familiar with Javascript/Typescript.
If you’ve yet to setup Hardhat, here’s the [**official guide**](https://hardhat.org/tutorial/creating-a-new-hardhat-project)**,** the following steps are to setup the verification plugin.

##

[​

](#install)
Install

Via npm.

Copy

Ask AI

```
npm i @nomicfoundation/hardhat-verify

```

##

[​

](#config)
Config

In `hardhat.config.ts`, import the plugin.

Copy

Ask AI

```
import "@nomicfoundation/hardhat-verify";

```

Add your Etherscan key.

Copy

Ask AI

```
config: HardhatUserConfig = {
  solidity: "0.8.28",
  networks: {
    sepolia: {
      url: `https://1rpc.io/sepolia`,
      accounts: [SEPOLIA_PRIVATE_KEY],
    },
  },
   etherscan: {
    apiKey: "YourEtherscanApiKey"
  }
};

```

##

[​

](#deploy-and-verify)
Deploy and Verify

Copy

Ask AI

```
npx hardhat ignition deploy ./ignition/modules/Lock.ts --network sepolia --verify

```

##

[​

](#verify-an-existing-contract)
Verify an Existing Contract

Copy

Ask AI

```
npx hardhat verify --network sepolia 0xdCBdBAA8404554502B433106e62728B659aCfE3b

```

##

[​

](#custom-chains)
Custom Chains

For super new Etherscan based explorers that are not supported by Hardhat yet, you can add them as a `customChain` in `hardhat.config.ts`.

Copy

Ask AI

```
etherscan: {
    apiKey: "YourEtherscanApiKey",
    customChains: [
    {
      network: "sonic",
      chainId: 146,
      urls: {
        apiURL: "https://api.etherscan.io/v2/api",
        browserURL: "https://sonicscan.org"
      }
    },
    {
      network: "katana",
      chainId: 146,
      urls: {
        apiURL: "https://api.etherscan.io/v2/api",
        browserURL: "https://sonicscan.org"
      }
    }
  ]
}

```

##

[​

](#migrating-from-v1)
Migrating from V1

API keys from any other explorer (such as BscScan/Basescan/Arbiscan) have been deprecated.

This change largely affects the `hardhat.config.ts` file — good riddance to the long `customChains` entries for each explorer.
Update your config to a single Etherscan `apiKey` entry as per above.

_This open source integration was shipped by_ [**_@antico5_**](https://github.com/antico5) _and the Hardhat team_.

---

# Verify with Remix

**Source:** https://docs.etherscan.io/contract-verification/verify-with-remix

---

[**Remix**](https://remix.ethereum.org/) is a no-frills, just works in your browser Solidity development tool.

##

[​

](#activate)
Activate

Once you’ve written and deployed your contract, click on the **Plugin Manager** and select **Contract Verification**.

##

[​

](#add-an-api-key)
Add an API Key

The Etherscan verification plugin requires an API key because grumpy bots yell at us on a daily basis.
Click the “Enable” link and add your key, leave the rest as default.

##

[​

](#verify)
Verify

Fill in the chain, contract address, and contract name that you deployed.
Now that the Etherscan checkbox is enabled, click Verify. Without this, the contract won’t show as verified on Etherscan’s explorer.

Once verified, you will see a happy green ✅.

_This open source integration was shipped by_ [**@manuelwedler**](https://github.com/manuelwedler) _and the Remix team_.

---

# Getting an API Key

**Source:** https://docs.etherscan.io/getting-an-api-key

---

1

Create an Etherscan Account

[Sign up](https://etherscan.io/register) for a Free account.

2

Navigate to the API Dashboard

Under your [API Dashboard](https://etherscan.io/apidashboard), click “Add +” to create a new API key. This key can be used to access all [supported chains](/supported-chains) under API V2.

3

Make your First Request

Use your key to start testing requests in the [API Playground](/api-reference) within this docs 🚀.

---

# Introduction

**Source:** https://docs.etherscan.io/introduction

---

Etherscan is the leading blockchain explorer, search, API, and analytics platform for Ethereum and other EVM-compatible chains.
With Etherscan API V2, we’ve unified all 60+ [supported chains](/supported-chains) under a single account and API key system. Your app becomes multichain 🌈 simply by updating the `chainid` parameter to support BNB Smart Chain (BSC), Base, Arbitrum, HyperEVM, and more.

##

[​

](#start-building)
Start Building

[

## Get Stablecoin Transfers to an Address

Check for USDC/USDT/PYUSD token transfers to an address.

Token Transfers Endpoint

](/api-reference/endpoint/tokentx)[

## Get Top Token Holders

Analyze the largest holders of YieldBasis(Ethereum), Aster(BNB) and other newly launched tokens.

Top Holders Endpoint

](/api-reference/endpoint/toptokenholders)[

## Get Address Portfolio

List all token balances for an address, across chains.

Address Portfolio Endpoint

](/api-reference/endpoint/addresstokenbalance)[

## Get Address Name Tag

Get name tags and labels associated with an address, such as exchange deposits “Coinbase 10”.

Name Tag Endpoint

](/api-reference/endpoint/getaddresstag)

---

# Introduction

**Source:** https://docs.etherscan.io/mcp-docs/introduction

---

Etherscan’s MCP [(Model Context Protocol)](https://modelcontextprotocol.io) allows you to connect Etherscan data to your AI models such as ChatGPT, Claude.
On top of providing reliable access, by avoiding hallucination or getting blocked via web search, it opens up the possiblity of building AI agents to do stuff for you.

##

[​

](#etherscan-mcp-server)
Etherscan MCP Server

We host a Streamable HTTP MCP server at

Copy

Ask AI

```
https://mcp.etherscan.io/mcp

```

Reach out for [early access](mailto:apisupport@etherscan.io) to a bearer token.

---

# Introduction

**Source:** https://docs.etherscan.io/metadata/introduction

---

The Metadata CSV export is an Enterprise feature that allows dataset exports of [labels](https://etherscan.io/labelcloud/), name tags, and other metadata related to an address.
This export currently supports Ethereum only, with other chains to follow.
[Contact us](mailto:apisupport@etherscan.io) to for more info and to book a demo.

---

# Common Error Messages

**Source:** https://docs.etherscan.io/resources/common-error-messages

---

An API call that encounters an error will return 0 as its `status code` and display the cause of the error under the `result` field.

Copy

Ask AI

```
{
  "status": "0",
  "message": "NOTOK",
  "result": "Max rate limit reached, please use API Key for higher rate limit"
}

```

###

[​

](#missing-or-unsupported-chain)
Missing or Unsupported Chain

“Missing or unsupported chainid parameter (required for v2 api), please see chainlist for the list of supported chainids”

The chain you’ve specified is not supported by us yet. It could also be that you’ve sent multiple chains at the same time like `420,10`, you can only send **one** at a time.

###

[​

](#invalid-api-key)
Invalid API Key

“Invalid API Key”

This error occurs when you specify an invalid API Key.
Ensure you are using your **Etherscan API Key**, keys from other chains like Polygonscan/Arbiscan are not valid for V2.
Keys do take a few minutes to activate, anything longer than should be alarming.

###

[​

](#max-rate-limit)
Max rate limit

“Max rate limit reached, please use API Key for higher rate limit”

This error occurs when you **exceed the rate limit** assigned to your specific API key.
To resolve, adhere to the [rate limits](/resources/rate-limits) of your available plan.
If you are using a script or application, **apply throttling** like a token bucket to limit the frequency of calls.

###

[​

](#missing-or-invalid-action)
Missing or invalid action

“Error! Missing Or invalid Action name”

This error occurs when you **do not specify**, or specify an **invalid** `module` and `action` name.
To resolve, **double check** your API query to use a valid module and action name.
If you require some help getting started, try copying the sample queries provided in the API Playground and pasting them into your browser.

###

[​

](#endpoint-specific-errors)
Endpoint-specific errors

“Error! Block number already pass”“Error! Invalid address format”“Contract source code not verified”

These error messages returned are specific to certain endpoints and their **related parameters.**
To resolve, kindly refer to the specific endpoint’s documentation, and check for the **correct format** or **values** to be specified as **parameters.**

###

[​

](#query-timeout)
Query Timeout

“Query Timeout occured. Please select a smaller result dataset”“Unexpected err, timeout occurred or server too busy. Please try again later”

This error occurs when you have sent a particularly large query that did not manage to be completed in time.
To resolve, consider selecting a smaller date/block range, though you may [**ping us**](/resources/contact-us) if you think the issue may be performance related.

###

[​

](#migrate-from-v1)
Migrate from V1

“Max calls per sec rate limit reached (2/sec). Please switch to API V2”

Please migrate to using Etherscan V2, with your Etherscan API key, here’s our [**migration guide**](/v2-migration).
Legacy V1 endpoints from other explorers are accessible by passing in the chain ID, eg 8453 for Base.

###

[​

](#free-tier-throttled)
Free Tier Throttled

“Free API access is temporarily unavailable due to unusually high network activity”

During periods of very high load, Free tier requests may be throttled. For uninterrupted access, using any [**Paid tier**](https://etherscan.io/apis) will have your requests prioritized.

---

# Contact Us

**Source:** https://docs.etherscan.io/resources/contact-us

---

Beware of **phishing attempts** and emails **impersonating the team** at Etherscan, we only communicate from the channels below.

###

[​

](#support-tickets)
Support Tickets

Our emails always come from the domain **@etherscan.io**.
Keep in mind that as a block explorer service, we **cannot cancel, refund or reverse transactions** as we do not process them.
If your issues are related to transactions, you may find helpful articles over at the [**Etherscan Information Center.**](https://info.etherscan.com/)

Reach out to us via a [**support ticket.**](https://etherscan.io/contactus)

###

[​

](#twitter)
Twitter

For general updates, new feature releases and community support, keep in touch with us via Twitter.

Follow us on [**Twitter.**](https://twitter.com/etherscan)

###

[​

](#freshstatus)
Freshstatus

Announcements for ongoing and scheduled maintenance works that may affect certain services used.

Check Etherscan’s [**network status.**](https://etherscan.freshstatus.io/)

---

# PRO Endpoints

**Source:** https://docs.etherscan.io/resources/pro-endpoints

---

A complete list of PRO endpoints available with a [paid subscription.](https://etherscan.io/apis)

##

[​

](#account)
Account

- [Get Historical Native Balance for an Address](/api-reference/endpoint/balancehistory)

##

[​

](#tokens)
Tokens

- [Get Historical ERC20-Token Account Balance by BlockNo](/api-reference/endpoint/tokenbalancehistory)

- [Get Historical ERC20-Token TotalSupply by ContractAddress & BlockNo](/api-reference/endpoint/tokensupplyhistory)

- [Get Top Token Holders](/api-reference/endpoint/toptokenholders)

- [Get Token Holder List by Contract Address](/api-reference/endpoint/tokenholderlist)

- [Get Token Holder Count by Contract Address](/api-reference/endpoint/tokenholdercount)

- [Get Address ERC20 Token Holding](/api-reference/endpoint/addresstokenbalance)

- [Get Address ERC721 Token Holding](/api-reference/endpoint/addresstokennftbalance)

- [Get Address ERC721 Token Inventory by Contract](/api-reference/endpoint/addresstokennftinventory)

##

[​

](#blocks)
Blocks

- [Get Daily Average Block Size](/api-reference/endpoint/dailyavgblocksize)

- [Get Daily Block Count and Rewards](/api-reference/endpoint/dailyblkcount)

- [Get Daily Block Rewards](/api-reference/endpoint/dailyblockrewards)

- [Get Daily Average Block Time](/api-reference/endpoint/dailyavgblocktime)

- [Get Daily Uncle Block Count and Rewards](/api-reference/endpoint/dailyuncleblkcount)

##

[​

](#stats)
Stats

- [Get Daily Network Transaction Fee](/api-reference/endpoint/dailytxnfee)

- [Get Daily New Address Count](/api-reference/endpoint/dailynewaddress)

- [Get Daily Network Utilization](/api-reference/endpoint/dailynetutilization)

- [Get Daily Average Network Hash Rate](/api-reference/endpoint/dailyavghashrate)

- [Get Daily Transaction Count](/api-reference/endpoint/dailytx)

- [Get Daily Average Network Difficulty](/api-reference/endpoint/dailyavgnetdifficulty)

- [Get Ether Historical Price](/api-reference/endpoint/ethdailyprice)

##

[​

](#gas-tracker)
Gas Tracker

- [Get Daily Average Gas Limit](/api-reference/endpoint/dailyavggaslimit)

- [Get Ethereum Daily Total Gas Used](/api-reference/endpoint/dailygasused)

- [Get Daily Average Gas Price](/api-reference/endpoint/dailyavggasprice)

##

[​

](#nametags)
Nametags

- [Get Nametag for an Address](/api-reference/endpoint/getaddresstag) (Pro Plus tier)

---

# Rate Limits

**Source:** https://docs.etherscan.io/resources/rate-limits

---

Historical endpoints have a rate limit of **2 calls/s** regardless of API PRO tier.

API TierRate LimitFree5 calls/second, up to 100,000 calls/day, may experience [usage caps](/resources/common-error-messages#free-tier-throttled) when network is busyStandard10 calls/second, up to 200,000 calls/dayAdvanced20 calls/second, up to 500,000 calls/dayProfessional30 calls/second, up to 1,000,000 calls/dayPro Plus30 calls/second, up to 1,500,000 calls/dayDedicated/Custom[**Contact Us**](mailto:apisupport@etherscan.io)

---

# Supported Chains

**Source:** https://docs.etherscan.io/supported-chains

---

For Solana(SOL) data, we’re also working on the [**Solscan APIs**](https://solscan.io/apis), available separately.

Sophon Mainnet (50104) and Sophon Sepolia Testnet (531050104) will be deprecating on November 22nd.

Chain NameChain IDEthereum Mainnet1Sepolia Testnet11155111Holesky Testnet17000Hoodi Testnet560048Abstract Mainnet2741Abstract Sepolia Testnet11124ApeChain Curtis Testnet33111ApeChain Mainnet33139Arbitrum Nova Mainnet42170Arbitrum One Mainnet42161Arbitrum Sepolia Testnet421614Avalanche C-Chain43114Avalanche Fuji Testnet43113Base Mainnet8453Base Sepolia Testnet84532Berachain Bepolia Testnet80069Berachain Mainnet80094BitTorrent Chain Mainnet199BitTorrent Chain Testnet1029Blast Mainnet81457Blast Sepolia Testnet168587773BNB Smart Chain Mainnet56BNB Smart Chain Testnet97Celo Mainnet42220Celo Sepolia Testnet11142220Fraxtal Mainnet252Fraxtal Testnet2522Gnosis100HyperEVM Mainnet999Katana Bokuto737373Katana Mainnet747474Linea Mainnet59144Linea Sepolia Testnet59141Mantle Mainnet5000Mantle Sepolia Testnet5003Memecore Testnet43521Monad Testnet10143Moonbase Alpha Testnet1287Moonbeam Mainnet1284Moonriver Mainnet1285OP Mainnet10OP Sepolia Testnet11155420opBNB Mainnet204opBNB Testnet5611Polygon Amoy Testnet80002Polygon Mainnet137Scroll Mainnet534352Scroll Sepolia Testnet534351Sei Mainnet1329Sei Testnet1328Sonic Mainnet146Sonic Testnet14601Sophon Mainnet50104Sophon Sepolia Testnet531050104Swellchain Mainnet1923Swellchain Testnet1924Taiko Hoodi167012Taiko Mainnet167000Unichain Mainnet130Unichain Sepolia Testnet1301World Mainnet480World Sepolia Testnet4801XDC Apothem Testnet51XDC Mainnet50zkSync Mainnet324zkSync Sepolia Testnet300

---

# V2 Migration

**Source:** https://docs.etherscan.io/v2-migration

---

[Contract verification](/contract-verification/verify-with-foundry) using Hardhat/Remix/Foundry also support using a single Etherscan API key for all chains

As of **August 15th, 2025**, the legacy **Etherscan API V1 endpoints have been deprecated** in favor of the new **Etherscan API V2**, which introduces a unified multichain experience across 60+ supported networks 🌈.
You’ll see a deprecation error message like this if you’re still using V1:

Copy

Ask AI

```
{
   "status":"0",
   "message":"NOTOK",
   "result":"You are using a deprecated V1 endpoint, switch to Etherscan API V2."
}

```

All existing endpoints remain compatible once you update them to the **V2 format**.

###

[​

](#how-to-migrate)
How to Migrate

1

Create an Etherscan account

[**Sign up**](https://etherscan.io/register) if you don’t have an account or if you’re using other explorers like BaseScan, BscScan, Polygonscan, etc.

2

Create an Etherscan API Key

Under your Etherscan [**API dashboard**](https://etherscan.io/apidashboard), create a new key. This key can be used to access all [supported chains](/supported-chains) under API V2.

3

Migrating Endpoints from Etherscan API V1

Use `https://api.etherscan.io/v2/api` as your **base path**, and include a `chainid` for your target network (e.g., 1 for Ethereum).Before (V1):

Copy

Ask AI

```
https://api.etherscan.io/api?&action=balance&apikey=YourEtherscanApiKey

```

After (V2):

Copy

Ask AI

```
https://api.etherscan.io/v2/api?chainid=1&action=balance&apikey=YourEtherscanApiKey

```

4

Migrating Endpoints from Other Explorers

Use the same base path (`https://api.etherscan.io/v2/api`) and include a `chainid` for the relevant chain from [this list](/supported-chains), in this case `137` for Polygon.Pass in your **Etherscan API key** instead of the old explorer-specific one.Before (PolygonScan V1):

Copy

Ask AI

```
https://api.polygonscan.com/api?&action=balance&apikey=YourPolygonscanApiKey

```

After (V2):

Copy

Ask AI

```
https://api.etherscan.io/v2/api?chainid=137&action=balance&apikey=YourEtherscanApiKey

```

---
