---
type: "agent_requested"
description: "Example description"
---

Your task is to open a new browser window using MCP Playwright, ensure the client app is running and served, locate the local port, and navigate to the app to perform manual testing. If the app is not running, start it first. Then, using the tools provided below, test the app for performance, memory leaks, caching, and other relevant metrics. For actual transactions, we will perform contract interactions and wait for approvals and signatures as needed, by waiting for manual confirmation and approving those transactions from the wallet.

## Available Testing Tools

### Browser-Based Tools

#### Browser DevTools

- **Chrome DevTools Network Tab** - RPC call analysis
- **Chrome DevTools Performance Tab** - Rendering bottlenecks
- **Chrome DevTools Memory Tab** - Memory leak detection
- **Chrome DevTools Console** - Real-time debugging
- **Chrome Eval Script** - Runtime code evaluation
- **Chrome Logs** - Debug logging
- **TanStack Query DevTools** - Cache inspection
- **React DevTools** - Component re-render tracking

#### External Tools

- **Playwright MCP** - Automated network monitoring

#### Blockchain & API Tools

- **Goldsky Explorer** - Query optimization and monitoring
- **Etherscan/Basescan** - Transaction verification
- **Pyth Network Entropy Explorer** - VRF transaction verification and entropy monitoring

### API Endpoints

- **Goldsky Subgraph API**: `https://api.goldsky.com/api/public/project_cmetslnjdncq801vt17ei7h57/subgraphs/production-lottery-enhanced/2.2.0/gn`
- **Etherscan/Basescan API Key**: `https://api.etherscan.io/v2/api?chainid=1&action=balance&apikey=YourEtherscanApiKeyToken`

### VRF Confirmation via Pyth Network Entropy Explorer

**Tool**: MCP Playwright → Navigate to `https://entropy.pyth.network/`

**VRF Verification Steps**:

1. Search by transaction hash from lottery draw
2. Verify entropy fulfillment status shows "Fulfilled"
3. Confirm random value matches lottery winning number
4. Check provider address matches expected VRF provider

## 🎯 What to Test & Which Tool to Use

### RPC Efficiency Testing

**Primary Tool**: Chrome DevTools Network Tab

- **Filter by**: `alchemy.com` requests
- **Look for**: Duplicate `eth_call`, `eth_getBalance`, `eth_getLogs`
- **Measure**: Request frequency, response times, 429 errors
- **Test scenarios**: Lottery creation, ticket purchase, result checking
- **RPC WebSocket**: `wss://base-sepolia.g.alchemy.com/v2/{API_KEY}`

### Cache Performance Testing

**Primary Tool**: TanStack Query DevTools

- **Monitor**: Query states (fresh, stale, loading, error)
- **Inspect**: Cache keys, invalidation patterns, stale times
- **Test scenarios**: Rapid navigation, data refresh, component unmounting

**Secondary Tool**: Chrome DevTools Memory Tab

- **Check**: Memory growth over time, garbage collection patterns
- **Look for**: Unbounded cache growth, memory leaks

### Goldsky Query Analysis

**Primary Tool**: Chrome DevTools Network Tab

- **Filter by**: `api.goldsky.com` requests
- **Monitor endpoint**: `https://api.goldsky.com/api/public/project_cmetslnjdncq801vt17ei7h57/subgraphs/production-lottery-enhanced/2.2.0/gn`
- **Measure**: Query response times, payload sizes
- **Identify**: Over-fetching, N+1 query patterns

**Secondary Tool**: Goldsky Dashboard

- **Access**: Visit Goldsky dashboard for your project
- **Test endpoint**: `https://api.goldsky.com/api/public/project_cmetslnjdncq801vt17ei7h57/subgraphs/production-lottery-enhanced/2.2.0/gn`
- **Use**: Query playground for optimization
- **Test**: Different query structures, indexing efficiency
- **Verify**: Schema alignment with frontend needs
- **Monitor**: Indexing lag, sync status

### Hook Coordination Testing

**Primary Tool**: React DevTools

- **Monitor**: Component re-renders, prop changes
- **Track**: Hook dependency arrays, effect triggers
- **Identify**: Cascade re-renders, infinite loops

**Secondary Tool**: Chrome DevTools Console

- **Add**: Custom debug logs for hook lifecycle
- **Track**: State changes, effect executions
- **Monitor**: Cleanup function calls

### WebSocket Connection Testing

**Primary Tool**: Chrome DevTools Network Tab (WS filter)

- **Monitor**: Connection establishment, drops, reconnections
- **Check**: Message frequency, subscription management
- **Verify**: Clean disconnection on component unmount

### Performance Bottleneck Detection

**Primary Tool**: Chrome DevTools Performance Tab

- **Record**: User interactions (lottery creation, ticket purchase)
- **Analyze**: Main thread blocking, layout thrashing
- **Identify**: JavaScript execution time, render delays

**Secondary Tool**: Chrome DevTools Lighthouse

- **Run**: Performance audits on key pages
- **Focus**: Core Web Vitals, bundle size optimization

---

## 📊 Testing Scenarios by Tool

### Network Tab Testing Flows

**Scenario 1: Lottery Creation**

- Clear network log
- Create new lottery
- Count RPC calls (target: < 5)
- Check for duplicate contract reads
- Verify gas estimation efficiency

**Scenario 2: Ticket Purchase**

- Monitor transaction preparation calls
- Track balance updates
- Check approval transactions
- Verify confirmation polling

**Scenario 3: Result Checking**

- **Monitor**: `api.goldsky.com` requests to production-lottery-enhanced subgraph
- **Check**: Query frequency vs RPC polling
- **Compare**: Goldsky data vs Etherscan API data using key `IN1Y5ZYUW97ZWIBXMZFXP46S6Y6561XICJ`
- **Verify**: Data synchronization between sources
- **Test**: Goldsky indexing speed vs previous Graph performance
- **VRF Validation**: Navigate to Pyth Network entropy explorer to verify VRF entropy generation
- **Cross-Reference**: Validate winning numbers match between contract events, Goldsky data, and Pyth entropy

### TanStack Query DevTools Flows

**Scenario 1: Cache Hit Testing**

- Navigate between pages rapidly
- Check query cache status
- Verify background refetch behavior
- Monitor stale data handling

**Scenario 2: Invalidation Testing**

- Trigger state changes (lottery draw)
- Check which queries invalidate
- Verify selective invalidation
- Monitor refetch cascades

### React DevTools Testing Flows

**Scenario 1: Re-render Analysis**

- Enable "Highlight updates when components render"
- Perform user actions
- Identify unnecessary re-renders
- Check hook dependency optimization

**Scenario 2: Component Lifecycle**

- Monitor component mount/unmount
- Check cleanup function execution
- Verify subscription cleanup
- Test memory leak prevention

---

## 🔍 Measurement Techniques

### Baseline Establishment

1. **Fresh browser session** - Clear all caches, localStorage
2. **Network throttling** - Test on "Fast 3G" to simulate real conditions
3. **Multiple runs** - Execute each test 3-5 times for consistency
4. **Device variation** - Test on different screen sizes, device types

### Data Collection Methods

**Quantitative Metrics**:

- **Network requests**: Count, timing, payload size
- **Cache performance**: Hit rate, miss frequency
- **Memory usage**: Heap size, garbage collection frequency
- **Render timing**: First paint, largest contentful paint

**Qualitative Observations**:

- **User experience**: Loading states, error handling
- **Data consistency**: RPC vs Goldsky data alignment
- **Error recovery**: Network failure handling

### Benchmark Targets

**Performance Thresholds**:

- First meaningful data: < 1000ms
- RPC calls per action: < 5
- Cache hit rate: > 70%
- Memory growth: < 10MB/hour
- Bundle size: < 500KB

---

## 🚨 Red Flag Detection Guide

### Network Tab Red Flags

- Multiple identical requests to `api.goldsky.com` within 5 seconds
- 429 rate limit responses from Goldsky API or Etherscan endpoints
- Failed WebSocket connections
- Requests > 2 seconds response time to Goldsky endpoint
- Etherscan API quota exceeded (check usage with key `IN1Y5ZYUW97ZWIBXMZFXP46S6Y6561XICJ`)
- Goldsky indexing lag indicators (stale data warnings)

### TanStack Query Red Flags

- Cache hit rate < 50%
- Queries stuck in loading state
- Excessive background refetches
- Memory usage climbing without plateau

### React DevTools Red Flags

- Components re-rendering > 5 times per action
- Missing dependency warnings in console
- Effects running on every render
- Components not unmounting cleanly

### Console Red Flags

- Unhandled promise rejections
- Memory warnings
- WebSocket connection errors
- Provider connection failures
- Goldsky API errors or timeout warnings

### Goldsky-Specific Red Flags

- Subgraph sync lag > 10 blocks
- Query timeout errors
- Invalid schema responses
- Missing indexed data that should be available

### VRF Red Flags

- Entropy status not "Fulfilled" in Pyth explorer
- Random values mismatch between Pyth and lottery results

---

## 📋 Tool Usage Priority Matrix

| Test Type          | Primary Tool      | Secondary Tool    | Tertiary Tool   |
| ------------------ | ----------------- | ----------------- | --------------- |
| RPC Efficiency     | Network Tab       | Alchemy Dashboard | Console         |
| Cache Performance  | TanStack DevTools | Memory Tab        | Network Tab     |
| Query Optimization | Network Tab       | Goldsky Dashboard | Console         |
| Hook Coordination  | React DevTools    | Console           | Performance Tab |
| Memory Leaks       | Memory Tab        | TanStack DevTools | Console         |
| Bundle Analysis    | Lighthouse        | Network Tab       | Performance Tab |

---

## 📝 Reporting & Documentation

- Document findings with tool evidence
- Provide specific optimization recommendations
- Plan implementation priorities
- **Migration-specific**: Document any behavioral differences between The Graph and Goldsky
- **Performance comparison**: Benchmark Goldsky performance against previous Graph metrics
