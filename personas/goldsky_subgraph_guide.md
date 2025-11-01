---
type: "agent_requested"
description: "Example description"
---

# Goldsky Subgraph API Guide

## Overview

This guide shows how to query the Goldsky subgraph API for lottery data using curl commands and how to interact with the lottery contract using viem.

## Environment Setup

```bash
# Set the subgraph URL
export SUBGRAPH_URL="https://api.goldsky.com/api/public/project_cmetslnjdncq801vt17ei7h57/subgraphs/production-lottery-enhanced/2.2.3/gn"

# Contract addresses (from .env)
export LOTTERY_CONTRACT="0x009F422a6e8Be39DE8D39B9232cd0B0e0ca508Fc"
export USDC_CONTRACT="0x1658608c718c834Da375BfaB8A6Df882Ae806BF3"
```

## Common Subgraph Queries

### 1. Get Active Lotteries

```bash
curl -s -X POST "$SUBGRAPH_URL" \
  -H 'Content-Type: application/json' \
  -d '{
    "query": "query($first:Int!,$skip:Int!){
      lotteries(
        first:$first,
        skip:$skip,
        where:{status:ACTIVE},
        orderBy:createdAt,
        orderDirection:desc
      ){
        id
        endTime
        prizeAmount
        ticketPrice
        status
        participantCount
        pickRange
      }
    }",
    "variables": { "first": 5, "skip": 0 }
  }' | jq '.data.lotteries'
```

### 2. Get Specific Lottery by ID

```bash
LOTTERY_ID="124"
curl -s -X POST "$SUBGRAPH_URL" \
  -H 'Content-Type: application/json' \
  -d '{
    "query": "query($id:ID!){
      lottery(id:$id){
        id
        status
        endTime
        prizeAmount
        ticketPrice
        participantCount
        pickRange
        prizeProvider
        prizeToken
      }
    }",
    "variables": { "id": "'$LOTTERY_ID'" }
  }' | jq '.data.lottery'
```

### 3. Get All Lotteries (Any Status)

```bash
curl -s -X POST "$SUBGRAPH_URL" \
  -H 'Content-Type: application/json' \
  -d '{
    "query": "query($first:Int!,$skip:Int!){
      lotteries(
        first:$first,
        skip:$skip,
        orderBy:createdAt,
        orderDirection:desc
      ){
        id
        status
        endTime
        prizeAmount
        ticketPrice
      }
    }",
    "variables": { "first": 10, "skip": 0 }
  }' | jq '.data.lotteries'
```

### 4. Get Tickets for a Lottery

```bash
LOTTERY_ID="124"
curl -s -X POST "$SUBGRAPH_URL" \
  -H 'Content-Type: application/json' \
  -d '{
    "query": "query($lotteryId:String!){
      tickets(where:{lottery:$lotteryId}){
        id
        player
        numbers
        lottery {
          id
        }
      }
    }",
    "variables": { "lotteryId": "'$LOTTERY_ID'" }
  }' | jq '.data.tickets'
```
