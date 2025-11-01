---
type: "agent_requested"
description: "Example description"
---

# Etherscan API Investigation Guide

## Overview

This guide provides comprehensive patterns for using Etherscan API to investigate blockchain transactions, events, and contract interactions on Base Sepolia testnet.

## Environment Setup

### API Key Configuration

Add your Etherscan API key to `.env`:

```bash
# .env
ETHERSCAN_API_KEY=IN1Y5ZYUW97ZWIBXMZFXP46S6Y6561XICJ
```

### Base Sepolia Configuration

- **Chain ID**: 84532
- **API Endpoint**: `https://api.etherscan.io/v2/api`
- **Explorer**: `https://sepolia.basescan.org`

## Common Investigation Patterns

### 1. Transaction Analysis

#### Get Recent Transactions for Address

```bash
# Get last 5 transactions for a specific address
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=account&action=txlist&address=0x83f83aa00e1ec088149a1cf15fab7a91aa4813c7&startblock=0&endblock=99999999&page=1&offset=5&sort=desc&apikey=${ETHERSCAN_API_KEY}" | jq '.result[0:2]'
```

#### Check Transaction Status

```bash
# Check if transaction succeeded or failed
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=transaction&action=getstatus&txhash=0x4f7d21f26b0acdefa1bee3109f098a8e1f105d463373ae04c610672232aeceae&apikey=${ETHERSCAN_API_KEY}" | jq '.'
```

#### Get Transaction Receipt

```bash
# Get detailed transaction receipt with logs
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=proxy&action=eth_getTransactionReceipt&txhash=0x4f7d21f26b0acdefa1bee3109f098a8e1f105d463373ae04c610672232aeceae&apikey=${ETHERSCAN_API_KEY}" | jq '.'
```

### 2. Contract Event Investigation

#### Get All Events from Contract

```bash
# Get all events from lottery contract in recent blocks
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=logs&action=getLogs&address=0xd8580a2d9f462faa03b2abe2dad044c4280ef6bb&fromBlock=31161500&toBlock=latest&apikey=${ETHERSCAN_API_KEY}" | jq '.result[] | {topics, blockNumber, transactionHash}'
```

#### Filter Events by Topic (Event Signature)

```bash
# Get LotteryRequested events (topic0 = event signature)
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=logs&action=getLogs&address=0xd8580a2d9f462faa03b2abe2dad044c4280ef6bb&fromBlock=31161500&toBlock=latest&topic0=0x47105a6d7f6f25c847bcb185cea1dbdcbcf7b89f22370b5fc878607fbd71a1e7&apikey=${ETHERSCAN_API_KEY}" | jq '.result'
```

#### Filter Events by User Address

```bash
# Get LotteryResult events for specific user (topic2 = indexed player address)
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=logs&action=getLogs&address=0xd8580a2d9f462faa03b2abe2dad044c4280ef6bb&fromBlock=31161500&toBlock=latest&topic0=0x84895d1d3741cb053332bfe13f08bde8f4064d3948d949afd5f5228901751808&topic2=0x00000000000000000000000083f83aa00e1ec088149a1cf15fab7a91aa4813c7&apikey=${ETHERSCAN_API_KEY}" | jq '.result'
```

### 3. Token Balance Investigation

#### Check USDC Balance

```bash
# Get USDC balance for address
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=account&action=tokenbalance&contractaddress=0x677728ce4e44973c1b1e69a99abdb53053c3e7f&address=0x83f83aa00e1ec088149a1cf15fab7a91aa4813c7&tag=latest&apikey=${ETHERSCAN_API_KEY}" | jq '.'
```

#### Check ETH Balance

```bash
# Get ETH balance for address
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=account&action=balance&address=0x83f83aa00e1ec088149a1cf15fab7a91aa4813c7&tag=latest&apikey=${ETHERSCAN_API_KEY}" | jq '.'
```

### 4. Contract Interaction Analysis

#### Get Internal Transactions

```bash
# Get internal transactions (contract calls)
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=account&action=txlistinternal&address=0xd8580a2d9f462faa03b2abe2dad044c4280ef6bb&startblock=0&endblock=99999999&page=1&offset=10&sort=desc&apikey=${ETHERSCAN_API_KEY}" | jq '.'
```

#### Get ERC-20 Token Transfers

```bash
# Get USDC transfers for address
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=account&action=tokentx&contractaddress=0x677728ce4e44973c1b1e69a99abdb53053c3e7f&address=0x83f83aa00e1ec088149a1cf15fab7a91aa4813c7&page=1&offset=10&sort=desc&apikey=${ETHERSCAN_API_KEY}" | jq '.'
```

## Event Signature Reference

### Lottery Contract Events

```bash
# LotteryRequested
topic0=0x47105a6d7f6f25c847bcb185cea1dbdcbcf7b89f22370b5fc878607fbd71a1e7

# LotteryResult
topic0=0x84895d1d3741cb053332bfe13f08bde8f4064d3948d949afd5f5228901751808

# ERC-20 Transfer
topic0=0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef
```

## Data Decoding Patterns

### Decode Event Data

```bash
# Get event with data field and decode manually
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=logs&action=getLogs&address=0xd8580a2d9f462faa03b2abe2dad044c4280ef6bb&fromBlock=31161500&toBlock=latest&topic0=0x84895d1d3741cb053332bfe13f08bde8f4064d3948d949afd5f5228901751808&apikey=${ETHERSCAN_API_KEY}" | jq '.result[0]'

# Data field contains:
# - Prize amount (32 bytes)
# - Ticket price (32 bytes)
# - Winning number (32 bytes)
# - Timestamp (32 bytes)
```

### Convert Wei to Human-Readable

```bash
# USDC uses 6 decimals, so divide by 10^6
# Example: 0x989680 = 10000000 wei = 10.0 USDC
echo "scale=6; 10000000 / 1000000" | bc
```

## Investigation Workflows

### VRF Result Investigation

```bash
# 1. Find LotteryRequested events for user
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=logs&action=getLogs&address=0xd8580a2d9f462faa03b2abe2dad044c4280ef6bb&fromBlock=31161500&toBlock=latest&topic0=0x47105a6d7f6f25c847bcb185cea1dbdcbcf7b89f22370b5fc878607fbd71a1e7&topic2=0x00000000000000000000000083f83aa00e1ec088149a1cf15fab7a91aa4813c7&apikey=${ETHERSCAN_API_KEY}" | jq '.result[-2:]'

# 2. Check for corresponding LotteryResult events
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=logs&action=getLogs&address=0xd8580a2d9f462faa03b2abe2dad044c4280ef6bb&fromBlock=31161500&toBlock=latest&topic0=0x84895d1d3741cb053332bfe13f08bde8f4064d3948d949afd5f5228901751808&topic2=0x00000000000000000000000083f83aa00e1ec088149a1cf15fab7a91aa4813c7&apikey=${ETHERSCAN_API_KEY}" | jq '.result'
```

### Transaction Failure Analysis

```bash
# 1. Get transaction receipt
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=proxy&action=eth_getTransactionReceipt&txhash=FAILED_TX_HASH&apikey=${ETHERSCAN_API_KEY}" | jq '.result.status'

# 2. Check transaction details
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=proxy&action=eth_getTransactionByHash&txhash=FAILED_TX_HASH&apikey=${ETHERSCAN_API_KEY}" | jq '.result'
```

## Useful JQ Filters

### Extract Key Information

```bash
# Get transaction hashes and status
jq '.result[] | {hash: .hash, status: .txreceipt_status}'

# Get event topics and block numbers
jq '.result[] | {topics, blockNumber, transactionHash}'

# Get token transfer amounts
jq '.result[] | {from, to, value, tokenSymbol}'

# Filter successful transactions only
jq '.result[] | select(.txreceipt_status == "1")'
```

## Contract Addresses (Base Sepolia)

```bash
# Lottery Contract
LOTTERY_CONTRACT=0xd8580a2d9f462faa03b2abe2dad044c4280ef6bb

# USDC Token Contract
USDC_CONTRACT=0x677728ce4e44973c1b1e69a99abdb53053c3e7f

# User Address (for testing)
USER_ADDRESS=0x83f83aa00e1ec088149a1cf15fab7a91aa4813c7
```

## Rate Limits and Best Practices

- **Rate Limit**: 5 requests per second
- **Use jq**: Always pipe to `jq` for readable output
- **Cache Results**: Save API responses for analysis
- **Batch Requests**: Use block ranges to get multiple events
- **Error Handling**: Check for `"status":"1"` in API responses

## Example Investigation Script

```bash
#!/bin/bash
# investigate-vrf.sh

API_KEY="YOUR_API_KEY"
USER_ADDR="0x83f83aa00e1ec088149a1cf15fab7a91aa4813c7"
LOTTERY_ADDR="0xd8580a2d9f462faa03b2abe2dad044c4280ef6bb"

echo "🔍 Investigating VRF results for $USER_ADDR"

echo "📝 Recent LotteryRequested events:"
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=logs&action=getLogs&address=$LOTTERY_ADDR&fromBlock=31161500&toBlock=latest&topic0=0x47105a6d7f6f25c847bcb185cea1dbdcbcf7b89f22370b5fc878607fbd71a1e7&topic2=0x00000000000000000000000083f83aa00e1ec088149a1cf15fab7a91aa4813c7&apikey=$API_KEY" | jq '.result[-2:] | .[] | {blockNumber, transactionHash}'

echo "🎲 VRF Results:"
curl -s "https://api.etherscan.io/v2/api?chainid=84532&module=logs&action=getLogs&address=$LOTTERY_ADDR&fromBlock=31161500&toBlock=latest&topic0=0x84895d1d3741cb053332bfe13f08bde8f4064d3948d949afd5f5228901751808&topic2=0x00000000000000000000000083f83aa00e1ec088149a1cf15fab7a91aa4813c7&apikey=$API_KEY" | jq '.result | length'
```

This guide provides the essential patterns for investigating blockchain data using Etherscan API on Base Sepolia testnet.
