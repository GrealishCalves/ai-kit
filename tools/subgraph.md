## GraphQL Subgraph Querying

### **What It Does**

- Query indexed blockchain data via GraphQL APIs (Goldsky, The Graph, etc.)
- Filter, sort, and paginate blockchain events without scanning blocks
- Aggregate on-chain data (revenue, user stats, transaction history)
- Real-time monitoring of smart contract state changes

### **Why Use It**

- **Instant queries:** No block scanning - data is pre-indexed
- **Powerful filtering:** Complex where clauses, comparison operators, nested relationships
- **Efficient pagination:** Handle large datasets with first/skip parameters
- **Type-safe:** GraphQL schema provides structure and validation
- **Composable:** Combine with jaq for advanced data transformation
- **Universal:** Works with any GraphQL endpoint (Goldsky, The Graph, custom subgraphs)

### **Essential Commands**

```bash
# Set subgraph endpoint
export SUBGRAPH_URL="https://api.goldsky.com/api/public/project_xxx/subgraphs/my-subgraph/1.0.0/gn"

# Basic query
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 10) { id field1 field2 } }"}' | jaq '.data.entities'

# Query with variables
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{
    "query": "query($first: Int!, $skip: Int!) { entities(first: $first, skip: $skip) { id } }",
    "variables": {"first": 10, "skip": 0}
  }' | jaq '.data.entities'

# Filter by field
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(where: {status: ACTIVE}) { id status } }"}' | jaq '.data.entities'

# Get single entity by ID
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query($id: ID!) { entity(id: $id) { id field1 field2 } }", "variables": {"id": "1"}}' | jaq '.data.entity'
```

### **Query Structure**

GraphQL queries follow this pattern:

```graphql
query QueryName($variable: Type!) {
  entityName(
    where: { field: value }
    orderBy: fieldName
    orderDirection: asc
    first: 10
    skip: 0
  ) {
    field1
    field2
    nestedEntity {
      nestedField
    }
  }
}
```

**Key Components:**
- `where`: Filter conditions
- `orderBy`: Sort field
- `orderDirection`: `asc` or `desc`
- `first`: Limit results (max usually 1000)
- `skip`: Offset for pagination
- Nested fields: Query related entities

### **Filtering Patterns**

```bash
# Exact match
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(where: {status: ACTIVE}) { id } }"}' | jaq '.'

# Comparison operators (_gt, _gte, _lt, _lte)
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(where: {amount_gte: \"1000\"}) { id amount } }"}' | jaq '.'

# Multiple conditions (AND)
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(where: {status: ACTIVE, amount_gte: \"1000\"}) { id } }"}' | jaq '.'

# Filter by address (use lowercase)
export USER_ADDRESS="0xabcd..."
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{
    "query": "query($user: String!) { entities(where: {user: $user}) { id } }",
    "variables": {"user": "'${USER_ADDRESS}'"}
  }' | jaq '.'

# Filter by timestamp (last 24 hours)
export TIMESTAMP_24H_AGO=$(($(date +%s) - 86400))
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{
    "query": "query($timestamp: BigInt!) { entities(where: {createdAt_gte: $timestamp}) { id } }",
    "variables": {"timestamp": "'${TIMESTAMP_24H_AGO}'"}
  }' | jaq '.'
```

### **Pagination Patterns**

```bash
# Basic pagination
export FIRST="100"
export SKIP="0"

curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{
    "query": "query($first: Int!, $skip: Int!) { entities(first: $first, skip: $skip) { id } }",
    "variables": {"first": '${FIRST}', "skip": '${SKIP}'}
  }' | jaq '.'

# Paginate through all results
export PAGE_SIZE="100"
for page in {0..9}; do
  SKIP=$((page * PAGE_SIZE))
  curl -s -X POST "${SUBGRAPH_URL}" \
    -H 'Content-Type: application/json' \
    -d '{
      "query": "query($first: Int!, $skip: Int!) { entities(first: $first, skip: $skip, orderBy: id) { id } }",
      "variables": {"first": '${PAGE_SIZE}', "skip": '${SKIP}'}
    }' | jaq '.data.entities[]'
  sleep 0.1
done
```

### **Sorting & Ordering**

```bash
# Sort ascending
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(orderBy: createdAt, orderDirection: asc) { id createdAt } }"}' | jaq '.'

# Sort descending (most recent first)
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(orderBy: createdAt, orderDirection: desc) { id createdAt } }"}' | jaq '.'

# Sort by numeric field
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(orderBy: amount, orderDirection: desc) { id amount } }"}' | jaq '.'
```

### **Nested Queries**

```bash
# Query with relationships
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 10) { id field1 relatedEntity { id name } } }"}' | jaq '.'

# Deep nesting
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 5) { id parent { id child { id value } } } }"}' | jaq '.'

# Filter on nested fields
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(where: {parent: \"123\"}) { id parent { id } } }"}' | jaq '.'
```

### **Combining with JAQ**

```bash
# Extract specific fields
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 100) { id amount status } }"}' | jaq '.data.entities[] | {id, amount}'

# Filter client-side
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 100) { id amount } }"}' | jaq '.data.entities[] | select((.amount | tonumber) > 1000)'

# Aggregate data
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 1000) { amount } }"}' | jaq '[.data.entities[].amount | tonumber] | {total: add, count: length, avg: (add/length)}'

# Group by field
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 1000) { status } }"}' | jaq '.data.entities | group_by(.status) | map({status: .[0].status, count: length})'

# Convert BigInt to human-readable (6 decimals for USDC)
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 10) { id amount } }"}' | jaq '.data.entities[] | {id, amountUSDC: ((.amount | tonumber) / 1000000)}'

# Format as CSV
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 100) { id status amount } }"}' | jaq -r '.data.entities[] | "\(.id),\(.status),\(.amount)"'
```

### **Error Handling**

```bash
# Check for errors
RESPONSE=$(curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 10) { id } }"}')

if echo "$RESPONSE" | jaq -e '.errors' > /dev/null; then
  echo "Error: $(echo $RESPONSE | jaq '.errors')"
  exit 1
else
  echo "$RESPONSE" | jaq '.data.entities'
fi

# Handle empty results
RESULT=$(curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(where: {id: \"999\"}) { id } }"}' | jaq '.data.entities')

if [ "$RESULT" = "[]" ] || [ "$RESULT" = "null" ]; then
  echo "No results found"
else
  echo "$RESULT"
fi
```

### **Monitoring & Health**

```bash
# Check subgraph metadata
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { _meta { block { number hash timestamp } } }"}' | jaq '.data._meta'

# Get latest indexed block
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { _meta { block { number } } }"}' | jaq -r '.data._meta.block.number'

# Check indexing lag
SUBGRAPH_BLOCK=$(curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { _meta { block { number } } }"}' | jaq -r '.data._meta.block.number')

echo "Subgraph at block: ${SUBGRAPH_BLOCK}"
```

### **Best Practices**

```bash
# ✅ Good: Filter on server
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(where: {status: ACTIVE}) { id } }"}' | jaq '.'

# ❌ Bad: Fetch all and filter client-side
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 1000) { id status } }"}' | jaq '.data.entities[] | select(.status == "ACTIVE")'

# ✅ Good: Request only needed fields
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 10) { id status } }"}' | jaq '.'

# ❌ Bad: Request all fields when you only need a few
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 10) { id field1 field2 field3 field4 field5 } }"}' | jaq '.data.entities[] | {id, status}'

# ✅ Good: Use variables for dynamic queries
export ENTITY_ID="123"
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{
    "query": "query($id: ID!) { entity(id: $id) { id } }",
    "variables": {"id": "'${ENTITY_ID}'"}
  }' | jaq '.'

# ❌ Bad: String concatenation in query
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entity(id: \"'${ENTITY_ID}'\") { id } }"}' | jaq '.'
```

### **Caching & Performance**

```bash
# Cache responses for analysis
CACHE_DIR="./subgraph_cache"
mkdir -p ${CACHE_DIR}

TIMESTAMP=$(date +%s)
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 100) { id status } }"}' > "${CACHE_DIR}/entities_${TIMESTAMP}.json"

# Reuse cached data
jaq '.data.entities[] | select(.status == "ACTIVE")' "${CACHE_DIR}/entities_${TIMESTAMP}.json"

# Batch multiple queries
cat <<EOF | curl -s -X POST "${SUBGRAPH_URL}" -H 'Content-Type: application/json' -d @- | jaq '.'
{
  "query": "query { activeEntities: entities(where: {status: ACTIVE}) { id } inactiveEntities: entities(where: {status: INACTIVE}) { id } }"
}
EOF
```

### **Common Workflows**

```bash
# Get total count
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 1000) { id } }"}' | jaq '.data.entities | length'

# Get latest N entities
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 10, orderBy: createdAt, orderDirection: desc) { id createdAt } }"}' | jaq '.'

# Find entity by field
export SEARCH_VALUE="target_value"
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{
    "query": "query($value: String!) { entities(where: {field: $value}) { id } }",
    "variables": {"value": "'${SEARCH_VALUE}'"}
  }' | jaq '.'

# Export to CSV
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 1000) { id status amount createdAt } }"}' | jaq -r '.data.entities[] | "\(.id),\(.status),\(.amount),\(.createdAt)"' > export.csv
```

### **Integration with Other Tools**

```bash
# Combine with ripgrep for local analysis
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 1000) { id } }"}' | jaq -r '.data.entities[].id' | rg "^123"

# Pipe to file for further processing
curl -s -X POST "${SUBGRAPH_URL}" \
  -H 'Content-Type: application/json' \
  -d '{"query": "query { entities(first: 1000) { id status } }"}' | jaq '.data.entities' > entities.json

# Use with watch for monitoring
watch -n 10 'curl -s -X POST "${SUBGRAPH_URL}" -H "Content-Type: application/json" -d "{\"query\": \"query { entities(where: {status: ACTIVE}) { id } }\"}" | jaq ".data.entities | length"'
```

