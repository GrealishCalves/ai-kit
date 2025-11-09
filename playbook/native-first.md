# Native-First Playbook

## Overview

Official CLI commands eliminate maintenance burden. Thin wrappers reduce code complexity by 95%. Future-proof against CLI changes.

## Core Principle

**Always prefer native CLI commands over custom implementations.**

## When to Use

- Integrating with third-party tools (Goldsky, Hardhat, etc.)
- Building CLI wrappers or automation scripts
- Replacing custom implementations with official tools
- Migrating from custom parsers to native commands

## Decision Tree

```
Do I need to interact with a CLI tool?
├─ YES
│  ├─ Does the official CLI provide this feature?
│  │  ├─ YES → Use native CLI command
│  │  │  └─ Add thin wrapper ONLY for:
│  │  │     - Environment loading (.env)
│  │  │     - Configuration injection (dynamic values)
│  │  │     - User-friendly prompts (Clack UI)
│  │  └─ NO → Check if feature is planned
│  │     ├─ Planned → Wait for official release
│  │     └─ Not planned → Consider custom implementation
│  │        └─ But first: Can I use existing flags/features?
│  └─ Does the CLI have --json or --filter flags?
│     ├─ YES → Use them! (avoid custom parsing)
│     └─ NO → Use stdout parsing as last resort
└─ NO → Use native Node.js APIs
```

## Best Practices

### 1. Use Official CLI Commands

❌ **Don't:**

```javascript
// Custom CLI parsing
const output = execSync("goldsky subgraph list").toString();
const lines = output.split("\n");
const versions = lines
  .filter((line) => line.includes("│"))
  .map((line) => {
    const parts = line.split("│");
    return {
      version: parts[1].trim(),
      status: parts[2].trim(),
    };
  });
```

✅ **Do:**

```javascript
// Use native CLI with --json flag
const output = execSync("goldsky subgraph list --json").toString();
const versions = JSON.parse(output);
```

### 2. Leverage Native Flags

Official CLIs often provide:

- `--json` - JSON output (structured data)
- `--filter` - Filter results (avoid custom filtering)
- `--summary` - Summary output (avoid custom aggregation)
- `--quiet` - Suppress output (avoid custom silencing)
- `--verbose` - Detailed output (avoid custom logging)

❌ **Don't:**

```javascript
// Custom filtering
const allVersions = execSync("goldsky subgraph list").toString();
const filteredVersions = allVersions.split("\n").filter((line) => line.includes("production"));
```

✅ **Do:**

```javascript
// Use native --filter flag
const versions = execSync("goldsky subgraph list --filter production").toString();
```

### 3. Thin Wrappers Only

Wrappers should ONLY:

- Load environment variables (.env)
- Inject dynamic configuration (from config files)
- Provide user-friendly prompts (Clack UI)
- Handle authentication checks
- Add timeout protection

❌ **Don't:**

```javascript
// 1050-line custom wizard with:
// - Custom CLI parsing
// - ANSI escape code stripping
// - Regex-based output parsing
// - Custom version management
// - Custom deployment logic
```

✅ **Do:**

```javascript
// 50-line thin wrapper
import { spawn } from "node:child_process";
import { loadEnvironment } from "./utils/environment.js";
import { getSubgraphNetwork } from "../../config/index.js";

async function deploy() {
  // Load environment
  const env = loadEnvironment();

  // Get dynamic config
  const network = getSubgraphNetwork(env.NETWORK);

  // Call native CLI
  const args = ["subgraph", "deploy", network, "--path", "./subgraph.yaml"];

  spawn("goldsky", args, { stdio: "inherit" });
}
```

### 4. Use Native Node.js APIs

❌ **Don't:**

```javascript
// Custom command execution with string interpolation
const result = execSync(`goldsky subgraph deploy ${name} --path ${path}`);
```

✅ **Do:**

```javascript
// Native spawn() with array arguments (prevents command injection)
const args = ["subgraph", "deploy", name, "--path", path];
const result = spawn("goldsky", args, { stdio: "inherit" });
```

### 5. Future-Proof Against Changes

Native CLI commands are:

- Maintained by official teams
- Documented in official docs
- Versioned with semantic versioning
- Backward compatible (usually)

Custom implementations are:

- Maintained by you
- Fragile (break on CLI changes)
- Require constant updates
- Not backward compatible

## Anti-Patterns

### 1. Custom CLI Parsing

❌ **Don't:**

```javascript
// Fragile regex-based parsing
const output = execSync("goldsky subgraph list").toString();
const versionMatch = output.match(/Version:\s+(\S+)/);
const version = versionMatch ? versionMatch[1] : null;
```

✅ **Do:**

```javascript
// Use --json flag
const output = execSync("goldsky subgraph list --json").toString();
const { version } = JSON.parse(output);
```

### 2. ANSI Escape Code Stripping

❌ **Don't:**

```javascript
// Custom ANSI stripping
function stripAnsi(str) {
  return str.replace(/\x1b\[[0-9;]*m/g, "");
}
```

✅ **Do:**

```javascript
// Use --no-color flag
const output = execSync("goldsky subgraph list --no-color").toString();
```

### 3. Custom Version Management

❌ **Don't:**

```javascript
// Custom version parsing and comparison
function getLatestVersion(versions) {
  return versions
    .map((v) => v.split(".").map(Number))
    .sort((a, b) => {
      for (let i = 0; i < 3; i++) {
        if (a[i] !== b[i]) return b[i] - a[i];
      }
      return 0;
    })[0]
    .join(".");
}
```

✅ **Do:**

```javascript
// Use native CLI version management
const latest = execSync("goldsky subgraph list --latest").toString();
```

### 4. Custom Deployment Logic

❌ **Don't:**

```javascript
// Custom deployment with manual steps
async function deploy() {
  // 1. Validate config
  // 2. Build subgraph
  // 3. Upload to IPFS
  // 4. Deploy to network
  // 5. Wait for indexing
  // 6. Verify deployment
  // ... 100+ lines of custom logic
}
```

✅ **Do:**

```javascript
// Use native CLI deploy command
async function deploy() {
  const args = ["subgraph", "deploy", name, "--path", "./subgraph.yaml"];
  spawn("goldsky", args, { stdio: "inherit" });
}
```

## Success Metrics

- **Code Reduction:** 95% (1050 lines → 50 lines)
- **Maintenance Burden:** Eliminated (official CLI maintained by vendor)
- **Fragility:** Eliminated (no regex parsing, no ANSI stripping)
- **Future-Proof:** Yes (official CLI versioned and documented)
- **Security:** Improved (spawn() with array arguments prevents command injection)

## Example: Goldsky CLI Migration

### Before (Custom Implementation)

```javascript
// 1050-line custom wizard
// - Custom CLI parsing (regex-based)
// - ANSI escape code stripping
// - Custom version management
// - Custom deployment logic
// - Fragile and hard to maintain
```

### After (Native-First)

```javascript
// 50-line thin wrapper
import { spawn } from "node:child_process";
import { loadEnvironment } from "./utils/environment.js";
import { getSubgraphNetwork } from "../../config/index.js";

async function deploy() {
  const env = loadEnvironment();
  const network = getSubgraphNetwork(env.NETWORK);

  const args = ["subgraph", "deploy", network, "--path", "./subgraph.yaml"];

  spawn("goldsky", args, { stdio: "inherit" });
}
```

### Impact

- **Lines of Code:** 1050 → 50 (95% reduction)
- **Complexity:** Eliminated regex parsing, ANSI stripping
- **Maintenance:** Official CLI maintained by Goldsky
- **Future-Proof:** Versioned and documented
- **Security:** spawn() prevents command injection

## Key Takeaways

1. **Native CLI first** - Always prefer official commands
2. **Leverage native flags** - Use --json, --filter, --summary
3. **Thin wrappers only** - Environment loading + config injection
4. **Use native Node.js APIs** - spawn() with array arguments
5. **Future-proof** - Official CLI maintained by vendor
6. **Eliminate custom parsing** - Use --json flag instead
7. **Reduce complexity** - 95% code reduction possible

## Checklist

Before implementing custom CLI wrapper:

- [ ] Does official CLI provide this feature?
- [ ] Does CLI have --json or --filter flags?
- [ ] Can I use native flags instead of custom parsing?
- [ ] Is wrapper truly needed or can I call CLI directly?
- [ ] Does wrapper ONLY handle environment/config?
- [ ] Am I using spawn() with array arguments?
- [ ] Have I verified against official documentation?
- [ ] Is this future-proof against CLI changes?

## References

- Official CLI documentation (single source of truth)
- Native Node.js APIs (child_process.spawn)
- Security best practices (avoid command injection)
- DRY principle (don't duplicate CLI logic)
