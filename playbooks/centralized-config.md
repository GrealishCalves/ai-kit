# Centralized Configuration Playbook

## Overview

Single source of truth prevents configuration drift. Dynamic configuration enables environment switching. Helper functions provide type-safe access.

## Core Principle

**All configuration must live in centralized config files, not scattered across codebase.**

## When to Use

- Managing multi-environment deployments (local/testnet/mainnet)
- Storing network-specific settings (chain IDs, RPC URLs, contract addresses)
- Configuring third-party integrations (API endpoints, project names)
- Defining application constants (magic numbers, thresholds)

## Architecture

```
config/
├── networks.json          # Network-specific configuration
├── index.js              # Helper functions for type-safe access
└── README.md             # Configuration documentation

.env.development          # Development environment variables
.env.production           # Production environment variables
```

## Best Practices

### 1. Single Source of Truth

❌ **Don't:**
```javascript
// Scattered configuration across files

// File 1: hardhat.config.js
const BASE_SEPOLIA_CHAIN_ID = 84532;

// File 2: deploy.js
const BASE_SEPOLIA_RPC = 'https://sepolia.base.org';

// File 3: subgraph.yaml
network: base-sepolia

// File 4: constants.js
const EXPLORER_URL = 'https://sepolia.basescan.org';
```

✅ **Do:**
```javascript
// config/networks.json (single source of truth)
{
  "84532": {
    "name": "Base Sepolia",
    "chainId": 84532,
    "rpcUrl": "https://sepolia.base.org",
    "explorerUrl": "https://sepolia.basescan.org",
    "explorerApiUrl": "https://api-sepolia.basescan.org/api",
    "subgraphNetwork": "base-sepolia",
    "subgraphProjectName": "production-lottery-enhanced"
  }
}
```

### 2. Dynamic Configuration

❌ **Don't:**
```javascript
// Hardcoded environment-specific values
function getSubgraphName() {
  return 'production-lottery-enhanced'; // Always testnet name
}
```

✅ **Do:**
```javascript
// Dynamic based on NODE_ENV
function getSubgraphProjectName(environment = process.env.NODE_ENV) {
  const chainId = getChainId(environment);
  const network = networks[chainId];
  return network.subgraphProjectName;
}

// Usage
const name = getSubgraphProjectName('production'); // 'production-lottery-mainnet'
const name = getSubgraphProjectName('development'); // 'production-lottery-enhanced'
```

### 3. Helper Functions for Type-Safe Access

❌ **Don't:**
```javascript
// Direct access to config (no type safety, error-prone)
const chainId = networks[process.env.NODE_ENV === 'production' ? '8453' : '84532'].chainId;
```

✅ **Do:**
```javascript
// Helper function with validation
export function getChainId(environment = process.env.NODE_ENV) {
  const chainId = environment === 'production' ? '8453' : '84532';
  
  if (!networks[chainId]) {
    throw new Error(`Network configuration not found for chain ID: ${chainId}`);
  }
  
  return chainId;
}

// Usage
const chainId = getChainId(); // Type-safe, validated
```

### 4. Environment-Driven Configuration

❌ **Don't:**
```javascript
// Hardcoded environment checks scattered across codebase
if (process.env.NODE_ENV === 'production') {
  // Use mainnet config
} else {
  // Use testnet config
}
```

✅ **Do:**
```javascript
// Centralized environment mapping
export function getNetworkConfig(environment = process.env.NODE_ENV) {
  const chainId = getChainId(environment);
  return networks[chainId];
}

// Usage
const config = getNetworkConfig(); // Automatically uses correct environment
```

### 5. Verify All Functions Are Used

❌ **Don't:**
```javascript
// Create functions "just in case" (YAGNI violation)
export function getNetworkName() { /* ... */ }
export function getRpcUrl() { /* ... */ }
export function getExplorerUrl() { /* ... */ }
export function getSubgraphNetwork() { /* ... */ }
// ... 10 more functions, only 3 actually used
```

✅ **Do:**
```javascript
// Only create functions when immediately needed
// Verify usage with: rg "functionName" --type js

// Used by Goldsky scripts (5 functions)
export function getChainId() { /* ... */ }
export function getExplorerApiUrl() { /* ... */ }
export function getSubgraphNetwork() { /* ... */ }
export function getSubgraphProjectName() { /* ... */ }
export function getPlaceholders() { /* ... */ }

// Used by Hardhat scripts (4 functions)
export function getNetworkConfig() { /* ... */ }
export function getRpcUrl() { /* ... */ }
export function getExplorerUrl() { /* ... */ }
export function getContractAddress() { /* ... */ }
```

## Configuration Structure

### networks.json

```json
{
  "84532": {
    "name": "Base Sepolia",
    "chainId": 84532,
    "rpcUrl": "https://sepolia.base.org",
    "explorerUrl": "https://sepolia.basescan.org",
    "explorerApiUrl": "https://api-sepolia.basescan.org/api",
    "subgraphNetwork": "base-sepolia",
    "subgraphProjectName": "production-lottery-enhanced",
    "contracts": {
      "ProductionLottery": "0x1234...",
      "Treasury": "0x5678...",
      "USDC": "0x9abc..."
    }
  },
  "8453": {
    "name": "Base Mainnet",
    "chainId": 8453,
    "rpcUrl": "https://mainnet.base.org",
    "explorerUrl": "https://basescan.org",
    "explorerApiUrl": "https://api.basescan.org/api",
    "subgraphNetwork": "base-mainnet",
    "subgraphProjectName": "production-lottery-mainnet",
    "contracts": {
      "ProductionLottery": "0xdef0...",
      "Treasury": "0x1234...",
      "USDC": "0x5678..."
    }
  }
}
```

### config/index.js

```javascript
import networks from './networks.json' assert { type: 'json' };

export function getChainId(environment = process.env.NODE_ENV) {
  return environment === 'production' ? '8453' : '84532';
}

export function getNetworkConfig(environment = process.env.NODE_ENV) {
  const chainId = getChainId(environment);
  const network = networks[chainId];
  
  if (!network) {
    throw new Error(`Network configuration not found for chain ID: ${chainId}`);
  }
  
  return network;
}

export function getRpcUrl(environment = process.env.NODE_ENV) {
  return getNetworkConfig(environment).rpcUrl;
}

export function getExplorerUrl(environment = process.env.NODE_ENV) {
  return getNetworkConfig(environment).explorerUrl;
}

export function getExplorerApiUrl(environment = process.env.NODE_ENV) {
  return getNetworkConfig(environment).explorerApiUrl;
}

export function getSubgraphNetwork(environment = process.env.NODE_ENV) {
  return getNetworkConfig(environment).subgraphNetwork;
}

export function getSubgraphProjectName(environment = process.env.NODE_ENV) {
  return getNetworkConfig(environment).subgraphProjectName;
}

export function getContractAddress(contractName, environment = process.env.NODE_ENV) {
  const network = getNetworkConfig(environment);
  const address = network.contracts[contractName];
  
  if (!address) {
    throw new Error(`Contract address not found: ${contractName}`);
  }
  
  return address;
}

export function getPlaceholders(environment = process.env.NODE_ENV) {
  const chainId = getChainId(environment);
  const network = getNetworkConfig(environment);
  
  return {
    CHAIN_ID: chainId,
    NETWORK_NAME: network.name,
    RPC_URL: network.rpcUrl,
    EXPLORER_URL: network.explorerUrl,
    SUBGRAPH_NETWORK: network.subgraphNetwork,
    PRODUCTION_LOTTERY_ADDRESS: network.contracts.ProductionLottery,
    TREASURY_ADDRESS: network.contracts.Treasury,
    USDC_ADDRESS: network.contracts.USDC
  };
}
```

## Anti-Patterns

### 1. Hardcoded Configuration

❌ **Don't:**
```javascript
const CHAIN_ID = 84532; // Hardcoded testnet
const RPC_URL = 'https://sepolia.base.org'; // Hardcoded testnet
```

✅ **Do:**
```javascript
const chainId = getChainId(); // Dynamic based on NODE_ENV
const rpcUrl = getRpcUrl(); // Dynamic based on NODE_ENV
```

### 2. Duplicate Configuration

❌ **Don't:**
```javascript
// File 1
const BASE_SEPOLIA_CHAIN_ID = 84532;

// File 2
const TESTNET_CHAIN_ID = 84532;

// File 3
const chainId = 84532;
```

✅ **Do:**
```javascript
// All files import from single source
import { getChainId } from '../../config/index.js';
const chainId = getChainId();
```

### 3. Environment Checks Scattered Across Codebase

❌ **Don't:**
```javascript
// File 1
if (process.env.NODE_ENV === 'production') { /* ... */ }

// File 2
if (process.env.NODE_ENV === 'production') { /* ... */ }

// File 3
if (process.env.NODE_ENV === 'production') { /* ... */ }
```

✅ **Do:**
```javascript
// Centralized environment mapping in config/index.js
export function getChainId(environment = process.env.NODE_ENV) {
  return environment === 'production' ? '8453' : '84532';
}

// All files use helper function
const chainId = getChainId();
```

## Success Metrics

- **Configuration Drift:** Eliminated (single source of truth)
- **Hardcoded Values:** 0 (all dynamic)
- **Function Usage:** 100% (no unused functions)
- **Environment Switching:** Seamless (NODE_ENV only)
- **Type Safety:** Improved (helper functions with validation)

## Example: Goldsky CLI Migration

### Before

```javascript
// Hardcoded network mapping (3 functions)
function getNetworkFromChainId(chainId) {
  if (chainId === 84532) return 'base-sepolia';
  if (chainId === 8453) return 'base-mainnet';
  throw new Error('Unsupported chain');
}

// Hardcoded placeholders
const PLACEHOLDERS = {
  CHAIN_ID: '84532', // Always testnet
  NETWORK_NAME: 'base-sepolia',
  // ... more hardcoded values
};
```

### After

```javascript
// Dynamic from config/networks.json
export function getSubgraphNetwork(environment = process.env.NODE_ENV) {
  return getNetworkConfig(environment).subgraphNetwork;
}

// Dynamic placeholders
export function getPlaceholders(environment = process.env.NODE_ENV) {
  const chainId = getChainId(environment);
  const network = getNetworkConfig(environment);
  
  return {
    CHAIN_ID: chainId,
    NETWORK_NAME: network.name,
    // ... all values dynamic
  };
}
```

### Impact

- **Hardcoded Values Removed:** 3 network mapping functions
- **Configuration Fields Added:** 3 (explorerApiUrl, subgraphNetwork, subgraphProjectName)
- **Dynamic Configuration:** 100% (environment-driven)
- **Function Usage:** 100% (9/9 functions used)

## Key Takeaways

1. **Single source of truth** - config/networks.json for all network config
2. **Dynamic configuration** - Environment-driven (NODE_ENV)
3. **Helper functions** - Type-safe access with validation
4. **Verify usage** - Use ripgrep to ensure all functions are used
5. **Eliminate duplication** - No hardcoded values scattered across codebase
6. **Environment switching** - Seamless testnet/mainnet switching

## Checklist

Before adding new configuration:

- [ ] Is this configuration network-specific?
- [ ] Should this be in config/networks.json?
- [ ] Do I need a helper function for type-safe access?
- [ ] Is this configuration used immediately?
- [ ] Have I verified no duplicate configuration exists?
- [ ] Is this configuration dynamic (environment-driven)?
- [ ] Have I tested both testnet and mainnet?
- [ ] Have I verified function usage with ripgrep?

