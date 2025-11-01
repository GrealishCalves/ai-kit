---
type: "always_apply"
---

# Etherscan API Tool

## ⚠️ FIRST: Load .env Configuration

**ALWAYS load environment variables from `.env` before using this tool:**

```bash
# Load .env file (REQUIRED FIRST STEP)
source .env
```

### Required .env Configuration

```bash
# .env - Add to your project root
ETHERSCAN_API_KEY=your_api_key_here

# Contract addresses
CONTRACT_ADDRESS=0xYourContractAddress
TOKEN_ADDRESS=0xYourTokenAddress
USER_ADDRESS=0xYourUserAddress

# Block ranges
START_BLOCK=0
END_BLOCK=99999999
```

---

## Network Configuration

**Base Sepolia Testnet**
- **Chain ID**: `84532`
- **API Endpoint**: `https://api.etherscan.io/v2/api`
- **Explorer**: `https://sepolia.basescan.org`

---

## Account Module

### Get Normal Transactions By Address

**Endpoint:** `module=account&action=txlist`

**Parameters:**
- `chainid` - Chain ID (84532 for Base Sepolia)
- `module` - Set to `account`
- `action` - Set to `txlist`
- `address` - Address to query
- `startblock` - Starting block number (default: 0)
- `endblock` - Ending block number (default: 99999999)
- `page` - Page number for pagination (default: 1)
- `offset` - Number of transactions per page (default: 10)
- `sort` - Sort order: `asc` or `desc` (default: desc)
- `apikey` - Your Etherscan API key

**Example:**
```bash
# Get last 5 transactions for address
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=account&action=txlist&address=${USER_ADDRESS}&startblock=${START_BLOCK}&endblock=${END_BLOCK}&page=1&offset=5&sort=desc&apikey=${ETHERSCAN_API_KEY}" | jaq '.result[0:2]'
```

**Response:**
```json
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "blockNumber": "31161500",
      "timeStamp": "1698765432",
      "hash": "0x...",
      "from": "0x...",
      "to": "0x...",
      "value": "1000000000000000000",
      "gas": "21000",
      "gasPrice": "1000000000",
      "isError": "0",
      "txreceipt_status": "1"
    }
  ]
}
```

---

### Get Internal Transactions by Address

**Endpoint:** `module=account&action=txlistinternal`

**Parameters:**
- `chainid` - Chain ID (84532 for Base Sepolia)
- `module` - Set to `account`
- `action` - Set to `txlistinternal`
- `address` - Address to query
- `startblock` - Starting block number (default: 0)
- `endblock` - Ending block number (default: 99999999)
- `page` - Page number for pagination (default: 1)
- `offset` - Number of transactions per page (default: 10)
- `sort` - Sort order: `asc` or `desc`
- `apikey` - Your Etherscan API key

**Example:**
```bash
# Get internal transactions (contract calls)
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=account&action=txlistinternal&address=${CONTRACT_ADDRESS}&startblock=${START_BLOCK}&endblock=${END_BLOCK}&page=1&offset=10&sort=desc&apikey=${ETHERSCAN_API_KEY}" | jaq '.'
```

**Response:**
```json
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "blockNumber": "31161500",
      "timeStamp": "1698765432",
      "from": "0x...",
      "to": "0x...",
      "value": "1000000000000000000",
      "contractAddress": "",
      "input": "",
      "type": "call",
      "gas": "2300",
      "gasUsed": "0",
      "isError": "0"
    }
  ]
}
```

---

### Get ERC-20 Token Transfers

**Endpoint:** `module=account&action=tokentx`

**Parameters:**
- `chainid` - Chain ID (84532 for Base Sepolia)
- `module` - Set to `account`
- `action` - Set to `tokentx`
- `contractaddress` - Token contract address (optional, filters by token)
- `address` - Address to query
- `page` - Page number for pagination (default: 1)
- `offset` - Number of transfers per page (default: 10)
- `sort` - Sort order: `asc` or `desc`
- `apikey` - Your Etherscan API key

**Example:**
```bash
# Get token transfers for address
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=account&action=tokentx&contractaddress=${TOKEN_ADDRESS}&address=${USER_ADDRESS}&page=1&offset=10&sort=desc&apikey=${ETHERSCAN_API_KEY}" | jaq '.'
```

**Response:**
```json
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "blockNumber": "31161500",
      "timeStamp": "1698765432",
      "hash": "0x...",
      "from": "0x...",
      "to": "0x...",
      "value": "10000000",
      "tokenName": "USD Coin",
      "tokenSymbol": "USDC",
      "tokenDecimal": "6",
      "contractAddress": "0x..."
    }
  ]
}
```

---

### Get Token Balance

**Endpoint:** `module=account&action=tokenbalance`

**Parameters:**
- `chainid` - Chain ID (84532 for Base Sepolia)
- `module` - Set to `account`
- `action` - Set to `tokenbalance`
- `contractaddress` - Token contract address
- `address` - Address to check balance
- `tag` - Block parameter: `latest`, `earliest`, or block number
- `apikey` - Your Etherscan API key

**Example:**
```bash
# Get token balance for address
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=account&action=tokenbalance&contractaddress=${TOKEN_ADDRESS}&address=${USER_ADDRESS}&tag=latest&apikey=${ETHERSCAN_API_KEY}" | jaq '.'
```

**Response:**
```json
{
  "status": "1",
  "message": "OK",
  "result": "10000000"
}
```

---

### Get ETH Balance

**Endpoint:** `module=account&action=balance`

**Parameters:**
- `chainid` - Chain ID (84532 for Base Sepolia)
- `module` - Set to `account`
- `action` - Set to `balance`
- `address` - Address to check balance
- `tag` - Block parameter: `latest`, `earliest`, or block number
- `apikey` - Your Etherscan API key

**Example:**
```bash
# Get ETH balance for address
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=account&action=balance&address=${USER_ADDRESS}&tag=latest&apikey=${ETHERSCAN_API_KEY}" | jaq '.'
```

**Response:**
```json
{
  "status": "1",
  "message": "OK",
  "result": "1000000000000000000"
}
```

---

## Logs Module

### Get Event Logs

**Endpoint:** `module=logs&action=getLogs`

**Parameters:**
- `chainid` - Chain ID (84532 for Base Sepolia)
- `module` - Set to `logs`
- `action` - Set to `getLogs`
- `address` - Contract address to get logs from
- `fromBlock` - Starting block number
- `toBlock` - Ending block number or `latest`
- `topic0` - Event signature hash (optional)
- `topic1` - First indexed parameter (optional)
- `topic2` - Second indexed parameter (optional)
- `topic3` - Third indexed parameter (optional)
- `topic0_1_opr` - Operator for topic0 and topic1 (optional: `and` or `or`)
- `topic0_2_opr` - Operator for topic0 and topic2 (optional: `and` or `or`)
- `topic1_2_opr` - Operator for topic1 and topic2 (optional: `and` or `or`)
- `apikey` - Your Etherscan API key

**Example 1: Get All Events**
```bash
# Get all events from contract
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=logs&action=getLogs&address=${CONTRACT_ADDRESS}&fromBlock=${START_BLOCK}&toBlock=latest&apikey=${ETHERSCAN_API_KEY}" | jaq '.result[] | {topics, blockNumber, transactionHash}'
```

**Example 2: Filter by Event Signature**
```bash
# Define event signature (e.g., Transfer event)
EVENT_SIGNATURE=0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef

# Get specific events
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=logs&action=getLogs&address=${CONTRACT_ADDRESS}&fromBlock=${START_BLOCK}&toBlock=latest&topic0=${EVENT_SIGNATURE}&apikey=${ETHERSCAN_API_KEY}" | jaq '.result'
```

**Example 3: Filter by User Address**
```bash
# Pad user address to 32 bytes for topic filtering
USER_TOPIC=0x000000000000000000000000${USER_ADDRESS#0x}

# Get events for specific user
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=logs&action=getLogs&address=${CONTRACT_ADDRESS}&fromBlock=${START_BLOCK}&toBlock=latest&topic0=${EVENT_SIGNATURE}&topic2=${USER_TOPIC}&apikey=${ETHERSCAN_API_KEY}" | jaq '.result'
```

**Response:**
```json
{
  "status": "1",
  "message": "OK",
  "result": [
    {
      "address": "0x...",
      "topics": [
        "0x47105a6d7f6f25c847bcb185cea1dbdcbcf7b89f22370b5fc878607fbd71a1e7",
        "0x0000000000000000000000000000000000000000000000000000000000000001",
        "0x00000000000000000000000083f83aa00e1ec088149a1cf15fab7a91aa4813c7"
      ],
      "data": "0x00000000000000000000000000000000000000000000000000000000009896800000000000000000000000000000000000000000000000000000000000989680",
      "blockNumber": "0x1db5c8c",
      "timeStamp": "0x6543210a",
      "gasPrice": "0x3b9aca00",
      "gasUsed": "0x5208",
      "logIndex": "0x0",
      "transactionHash": "0x...",
      "transactionIndex": "0x1"
    }
  ]
}
```

---

## Transaction Module

### Get Transaction Status

**Endpoint:** `module=transaction&action=getstatus`

**Parameters:**
- `chainid` - Chain ID (84532 for Base Sepolia)
- `module` - Set to `transaction`
- `action` - Set to `getstatus`
- `txhash` - Transaction hash to check
- `apikey` - Your Etherscan API key

**Example:**
```bash
# Check transaction status
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=transaction&action=getstatus&txhash=${TX_HASH}&apikey=${ETHERSCAN_API_KEY}" | jaq '.'
```

**Response:**
```json
{
  "status": "1",
  "message": "OK",
  "result": {
    "isError": "0",
    "errDescription": ""
  }
}
```

---

## Proxy Module

### Get Transaction Receipt

**Endpoint:** `module=proxy&action=eth_getTransactionReceipt`

**Parameters:**
- `chainid` - Chain ID (84532 for Base Sepolia)
- `module` - Set to `proxy`
- `action` - Set to `eth_getTransactionReceipt`
- `txhash` - Transaction hash
- `apikey` - Your Etherscan API key

**Example:**
```bash
# Get transaction receipt with logs
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=proxy&action=eth_getTransactionReceipt&txhash=${TX_HASH}&apikey=${ETHERSCAN_API_KEY}" | jaq '.'
```

**Response:**
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "blockHash": "0x...",
    "blockNumber": "0x1db5c8c",
    "contractAddress": null,
    "cumulativeGasUsed": "0x5208",
    "from": "0x...",
    "gasUsed": "0x5208",
    "logs": [],
    "logsBloom": "0x...",
    "status": "0x1",
    "to": "0x...",
    "transactionHash": "0x...",
    "transactionIndex": "0x1"
  }
}
```

---

### Get Transaction by Hash

**Endpoint:** `module=proxy&action=eth_getTransactionByHash`

**Parameters:**
- `chainid` - Chain ID (84532 for Base Sepolia)
- `module` - Set to `proxy`
- `action` - Set to `eth_getTransactionByHash`
- `txhash` - Transaction hash
- `apikey` - Your Etherscan API key

**Example:**
```bash
# Get transaction details
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=proxy&action=eth_getTransactionByHash&txhash=${TX_HASH}&apikey=${ETHERSCAN_API_KEY}" | jaq '.result'
```

**Response:**
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "blockHash": "0x...",
    "blockNumber": "0x1db5c8c",
    "from": "0x...",
    "gas": "0x5208",
    "gasPrice": "0x3b9aca00",
    "hash": "0x...",
    "input": "0x",
    "nonce": "0x1",
    "to": "0x...",
    "transactionIndex": "0x1",
    "value": "0xde0b6b3a7640000",
    "v": "0x1b",
    "r": "0x...",
    "s": "0x..."
  }
}
```

---

## Reference Data

### Common Event Signatures

**ERC-20 Transfer Event**
```bash
# Transfer(address indexed from, address indexed to, uint256 value)
TRANSFER_TOPIC=0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef
```

**ERC-721 Transfer Event**
```bash
# Transfer(address indexed from, address indexed to, uint256 indexed tokenId)
NFT_TRANSFER_TOPIC=0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef
```

**ERC-20 Approval Event**
```bash
# Approval(address indexed owner, address indexed spender, uint256 value)
APPROVAL_TOPIC=0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925
```

### Topic Encoding

**Address to Topic (32 bytes, left-padded)**
```bash
# Convert address to topic format
USER_TOPIC=0x000000000000000000000000${USER_ADDRESS#0x}
```

**Number to Topic (32 bytes, left-padded hex)**
```bash
# Example: tokenId 123 = 0x7b
TOKEN_ID_TOPIC=0x000000000000000000000000000000000000000000000000000000000000007b
```

### Data Decoding

**Wei to Human-Readable**
```bash
# ETH uses 18 decimals
# 1 ETH = 1000000000000000000 wei
echo "scale=18; 1000000000000000000 / 1000000000000000000" | bc
# Output: 1.000000000000000000

# USDC uses 6 decimals
# 10 USDC = 10000000 (6 decimals)
echo "scale=6; 10000000 / 1000000" | bc
# Output: 10.000000
```

**Hex to Decimal**
```bash
# Convert hex value to decimal
echo $((16#989680))
# Output: 10000000
```

**Extract Data from Event Log**
```bash
# Event data is hex-encoded, 32 bytes per parameter
# Example data: 0x00000000000000000000000000000000000000000000000000000000009896800000000000000000000000000000000000000000000000000000000000989680

# First 32 bytes (64 hex chars after 0x)
PARAM1=$(echo "0x00000000000000000000000000000000000000000000000000000000009896800000000000000000000000000000000000000000000000000000000000989680" | cut -c3-66)
echo $((16#${PARAM1}))
# Output: 10000000

# Second 32 bytes
PARAM2=$(echo "0x00000000000000000000000000000000000000000000000000000000009896800000000000000000000000000000000000000000000000000000000000989680" | cut -c67-130)
echo $((16#${PARAM2}))
# Output: 10000000
```

---

## Common Workflows

### Workflow 1: Check Transaction Status

```bash
# Step 1: Check if transaction succeeded
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=proxy&action=eth_getTransactionReceipt&txhash=${TX_HASH}&apikey=${ETHERSCAN_API_KEY}" | jaq '.result.status'
# "0x1" = success, "0x0" = failure

# Step 2: Get transaction details
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=proxy&action=eth_getTransactionByHash&txhash=${TX_HASH}&apikey=${ETHERSCAN_API_KEY}" | jaq '.result'

# Step 3: Get error description
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=transaction&action=getstatus&txhash=${TX_HASH}&apikey=${ETHERSCAN_API_KEY}" | jaq '.result'
```

---

### Workflow 2: Check User Balances

```bash
# Step 1: Get token transfers
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=account&action=tokentx&address=${USER_ADDRESS}&page=1&offset=10&sort=desc&apikey=${ETHERSCAN_API_KEY}" | jaq '.result[] | {hash, from, to, value, tokenSymbol}'

# Step 2: Get token balance
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=account&action=tokenbalance&contractaddress=${TOKEN_ADDRESS}&address=${USER_ADDRESS}&tag=latest&apikey=${ETHERSCAN_API_KEY}" | jaq '.result'

# Step 3: Get ETH balance
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=account&action=balance&address=${USER_ADDRESS}&tag=latest&apikey=${ETHERSCAN_API_KEY}" | jaq '.result'
```

---

### Workflow 3: Monitor Contract Events

```bash
# Step 1: Get recent events (Transfer events example)
EVENT_SIGNATURE=0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=logs&action=getLogs&address=${CONTRACT_ADDRESS}&fromBlock=${START_BLOCK}&toBlock=latest&topic0=${EVENT_SIGNATURE}&apikey=${ETHERSCAN_API_KEY}" | jaq '.result[-10:]'

# Step 2: Filter events by user
USER_TOPIC=0x000000000000000000000000${USER_ADDRESS#0x}
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=logs&action=getLogs&address=${CONTRACT_ADDRESS}&fromBlock=${START_BLOCK}&toBlock=latest&topic0=${EVENT_SIGNATURE}&topic1=${USER_TOPIC}&apikey=${ETHERSCAN_API_KEY}" | jaq '.result'

# Step 3: Get event data
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=logs&action=getLogs&address=${CONTRACT_ADDRESS}&fromBlock=${START_BLOCK}&toBlock=latest&topic0=${EVENT_SIGNATURE}&apikey=${ETHERSCAN_API_KEY}" | jaq '.result[0] | {data, topics, blockNumber}'
```

---

## JAQ Filters (Common Patterns)

```bash
# Extract specific fields
jaq '.result[] | {hash, from, to, value}'

# Filter successful transactions
jaq '.result[] | select(.txreceipt_status == "1")'

# Get last 5 results
jaq '.result[-5:]'

# Count results
jaq '.result | length'

# Get first result
jaq '.result[0]'

# Format as table
jaq -r '.result[] | [.blockNumber, .hash] | @tsv'
```

---

## Best Practices

### Rate Limits
- Free tier: 5 requests/second
- Use block ranges to limit query scope
- Cache responses when possible

### Error Handling
```bash
# Check response status
RESPONSE=$(curl -s "https://api.etherscan.io/v2/api?...")
STATUS=$(echo "$RESPONSE" | jaq -r '.status')

if [ "$STATUS" = "1" ]; then
  echo "$RESPONSE" | jaq '.result'
else
  echo "Error: $(echo "$RESPONSE" | jaq -r '.message')"
fi
```

### Security
- **ALWAYS** load API key from `.env` file
- **NEVER** commit `.env` to git (add to `.gitignore`)
- API keys are read-only by default

---

## Quick Reference

### Base Sepolia Network
```bash
CHAIN_ID=84532
API_BASE=https://api.etherscan.io/v2/api
EXPLORER=https://sepolia.basescan.org
```

### Common Modules
- `account` - Account-related queries (transactions, balances)
- `logs` - Event logs and contract events
- `transaction` - Transaction status and details
- `proxy` - Direct Ethereum JSON-RPC calls
- `contract` - Contract source code and ABI
- `stats` - Network statistics

### Common Actions
- `txlist` - Get normal transactions
- `txlistinternal` - Get internal transactions
- `tokentx` - Get ERC-20 token transfers
- `tokenbalance` - Get token balance
- `balance` - Get ETH balance
- `getLogs` - Get event logs
- `getstatus` - Get transaction status
- `eth_getTransactionReceipt` - Get transaction receipt
- `eth_getTransactionByHash` - Get transaction details

---

## Additional Resources

- **Official Docs**: https://docs.etherscan.io
- **Base Sepolia Explorer**: https://sepolia.basescan.org
- **Supported Chains**: https://docs.etherscan.io/supported-chains
- **Rate Limits**: https://docs.etherscan.io/resources/rate-limits
- **API Plans**: https://etherscan.io/apis
