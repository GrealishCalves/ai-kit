# Knip - Dead Code Detection

> **Docs:** https://knip.dev | **v5+** | **Node.js v18.18.0+** | **TypeScript 5.0.4+**

## What It Does

Finds unused files, dependencies, exports, and types in JavaScript/TypeScript projects.

**Key Features:**
- ✅ Unused files, dependencies, exports, types, class/enum members
- ✅ Monorepo support (pnpm, Yarn, Nx, Turborepo)
- ✅ 115+ framework plugins (auto-detects entry points)
- ✅ Auto-fix with `--fix` flag (granular control with `--fix-type`)
- ✅ Production mode with `--production`
- ✅ Watch mode with `--watch` (live updates)
- ✅ Built-in compilers (.astro, .mdx, .svelte, .vue)
- ✅ Framework-agnostic (works with any JS/TS project)

**Safety:** Read-only by default. Never modifies code unless you use `--fix`.

**Output:** Save reports to `/temp` directory (gitignored).

---

## Installation

```bash
# Quick setup (recommended)
pnpm create @knip/config
pnpm knip

# Manual install
pnpm add -D knip typescript @types/node

# Run without install
pnpm dlx knip
```

---

## Essential Commands

### Basic Usage

```bash
# Run analysis
pnpm knip

# Help & version
pnpm knip --help
pnpm knip --version
```

### Filtering

```bash
# Specific workspace (monorepo)
pnpm knip --workspace packages/core
pnpm knip -W apps/web

# Specific issue types
pnpm knip --include files,dependencies
pnpm knip --files                    # Only unused files
pnpm knip --dependencies             # Only dependencies
pnpm knip --exports                  # Only exports

# Exclude issue types
pnpm knip --exclude exports,types
```

### Production Mode

```bash
# Ignore dev dependencies and test files
pnpm knip --production

# Strict mode (production + stricter checks)
pnpm knip --strict
```

### Reporters & Output

```bash
# JSON output (save to temp/)
pnpm knip --reporter json > temp/knip.json

# Compact output
pnpm knip --reporter compact

# Multiple reporters
pnpm knip --reporter json --reporter compact

# Reporter with options (e.g., CODEOWNERS path)
pnpm knip --reporter codeowners --reporter-options '{"path":".github/CODEOWNERS"}'

# View JSON with syntax highlighting
pnpm knip --reporter json > temp/knip.json
bat temp/knip.json
```

### Auto-Fix (⚠️ Destructive)

```bash
# ⚠️ WARNING: Automatically removes unused code
# ALWAYS review findings first, then use --fix

# Dry run first (safe)
pnpm knip --include files,dependencies

# Then auto-fix (destructive)
pnpm knip --fix --include files,dependencies

# Granular fix with --fix-type (more control)
pnpm knip --fix-type exports,types
pnpm knip --fix-type exports --fix-type types  # Same as above
pnpm knip --fix-type dependencies

# Fix with auto-formatting
pnpm knip --fix --format

# Fix only unused files (requires explicit flag)
pnpm knip --fix --allow-remove-files --include files

# Fix only unused dependencies
pnpm knip --fix --include dependencies
```

**Auto-fix removes:**
- ✅ Unused files (deletes files) - requires `--allow-remove-files`
- ✅ Unused dependencies (removes from package.json)
- ✅ Unused exports (removes export statements)
- ✅ Unused types (removes type exports)
- ✅ Unused enum members (removes enum values)
- ✅ Unused class members (disabled by default, opt-in)
- ❌ Does NOT remove: unlisted deps, unresolved imports, duplicates

---

## Configuration

### Default Config

Knip works with zero config. Default entry points:

```json
{
  "entry": [
    "{index,cli,main}.{js,cjs,mjs,jsx,ts,cts,mts,tsx}",
    "src/{index,cli,main}.{js,cjs,mjs,jsx,ts,cts,mts,tsx}"
  ],
  "project": ["**/*.{js,cjs,mjs,jsx,ts,cts,mts,tsx}!"]
}
```

### Config File Locations

Knip looks for config in this order:

1. `knip.json` / `knip.jsonc`
2. `.knip.json` / `.knip.jsonc`
3. `knip.ts` / `knip.js`
4. `knip.config.ts` / `knip.config.js`
5. `package.json` (in `"knip"` property)

### Custom Config Path

```bash
pnpm knip --config path/to/knip.json
```

### Basic Config

```json
{
  "$schema": "https://unpkg.com/knip@5/schema.json",
  "entry": ["src/index.ts"],
  "project": ["src/**/*.ts"]
}
```

### Framework-Agnostic Config

```json
{
  "$schema": "https://unpkg.com/knip@5/schema.json",
  "entry": [
    "src/index.{ts,tsx,js,jsx}",
    "src/main.{ts,tsx,js,jsx}",
    "src/app.{ts,tsx,js,jsx}"
  ],
  "project": [
    "src/**/*.{ts,tsx,js,jsx}",
    "!src/**/*.test.{ts,tsx,js,jsx}",
    "!src/**/*.spec.{ts,tsx,js,jsx}",
    "!src/**/__tests__/**",
    "!src/**/test-helpers/**"
  ],
  "ignore": ["src/generated/**"]
}
```

**Tip:** Prefer production mode (`pnpm knip --production`) over `ignore` entries for excluding test code; use negated `project` patterns to shape unused-file analysis.

### Monorepo Config

```json
{
  "$schema": "https://unpkg.com/knip@5/schema.json",
  "workspaces": {
    ".": {
      "entry": "src/index.ts"
    },
    "packages/*": {
      "entry": "src/index.ts!",
      "project": "src/**/*.ts!"
    },
    "apps/*": {
      "entry": ["src/main.ts", "src/**/*.page.ts"],
      "project": "src/**/*.ts"
    }
  }
}
```

**Note:** The `!` suffix marks files as production code for `--production` mode.

### Ignore Patterns

```json
{
  "ignore": [
    "**/*.test.ts",
    "**/__tests__/**",
    "**/dist/**",
    "scripts/legacy/**"
  ],
  "ignoreFiles": ["src/generated/**"],
  "ignoreDependencies": ["@types/*"],
  "ignoreWorkspaces": ["packages/deprecated"],
  "ignoreExportsUsedInFile": true
}
```

### Rules Configuration

Control how issue types are reported:

```json
{
  "rules": {
    "files": "error",       // Counted & printed (default)
    "dependencies": "warn",  // Printed in gray, not counted
    "exports": "off"        // Not printed or counted
  }
}
```

**Rule values:**
- `"error"` - Printed & counted toward exit code (default)
- `"warn"` - Printed in faded/gray color, not counted
- `"off"` - Not printed or counted (same as `--exclude`)

---

## Issue Types

| Type | Description | Auto-fix | Default |
|------|-------------|----------|---------|
| `files` | Unused files | ✅ | ✅ |
| `dependencies` | Unused dependencies | ✅ | ✅ |
| `devDependencies` | Unused dev dependencies | ✅ | ✅ |
| `unlisted` | Used but not listed in package.json | ❌ | ✅ |
| `binaries` | Unused binaries | ❌ | ✅ |
| `unresolved` | Unresolved imports | ❌ | ✅ |
| `exports` | Unused exports | ✅ | ✅ |
| `types` | Unused exported types | ✅ | ✅ |
| `nsExports` | Unused namespace exports | ✅ | 🟠 Opt-in |
| `nsTypes` | Unused namespace types | ✅ | 🟠 Opt-in |
| `enumMembers` | Unused enum members | ✅ | ✅ |
| `classMembers` | Unused class members | ✅ | 🟠 Opt-in |
| `duplicates` | Duplicate exports | ❌ | ✅ |

**Legend:**
- ✅ = Included/supported by default
- 🟠 = Opt-in (use `--include` to enable)
- ❌ = Not auto-fixable

**Enable opt-in types:**
```bash
# Enable namespace exports
pnpm knip --include nsExports,nsTypes

# Enable class members
pnpm knip --include classMembers
```

---

## Common Workflows

### 1. Initial Analysis

```bash
# Run Knip and save report
pnpm knip --reporter json > temp/knip.json

# View report with syntax highlighting
bat temp/knip.json

# Extract unused files
bat temp/knip.json | jaq -r '.files[] | .filePath' > temp/knip-unused-files.txt
bat temp/knip-unused-files.txt
```

### 2. Cross-Validation with Ripgrep

```bash
# Find files with no imports (high confidence unused)
rg --files-without-match "import.*from|require\(" src > temp/rg-no-imports.txt

# Find high-confidence deletions (in both Knip and ripgrep)
comm -12 <(sort temp/knip-unused-files.txt) <(sort temp/rg-no-imports.txt) > temp/high-confidence.txt

# Review findings
bat temp/high-confidence.txt

# Inspect specific files
fd -e ts src/ | rg -f temp/high-confidence.txt | xargs bat
```

### 3. Safe Cleanup Process

```bash
# 1. Run analysis
pnpm knip --reporter json > temp/knip.json

# 2. Review findings
bat temp/knip.json

# 3. Fix unused dependencies (safest)
pnpm knip --fix --include dependencies

# 4. Manually review unused files before deletion
bat temp/knip.json | jaq -r '.files[] | .filePath'

# 5. Delete files manually after review
# (or use --fix --include files if confident)
```

---

## Handling False Positives

### Ignore Specific Files

```json
{
  "ignore": ["src/legacy/**", "scripts/old-build.js"]
}
```

### Ignore Specific Dependencies

```json
{
  "ignoreDependencies": ["@types/node", "typescript"]
}
```

### Ignore Exports Used in Same File

```json
{
  "ignoreExportsUsedInFile": true
}
```

### Ignore Specific Files (Unused Files Only)

```json
{
  "ignoreFiles": ["src/generated/**"]
}
```

**Note:** `ignoreFiles` only affects the `files` issue type. Files are still analyzed for other issues (exports, dependencies, unresolved). Use `ignore` to suppress all issue types.

### JSDoc Tags (Inline Ignore)

```ts
/** @public - Exported for public API */
export const publicAPI = () => {};

/** @beta - Exported for beta testing */
export const betaFeature = () => {};

/** @alpha - Exported for alpha testing */
export const alphaFeature = () => {};

/** @internal - Exported for internal use (ignored in production mode) */
export const internalUtil = () => {};

/** @lintignore - Custom tag for ignoring */
export const customIgnored = () => {};
```

**Use custom tags with CLI:**
```bash
# Exclude tagged exports from report
pnpm knip --tags -lintignore,-internal

# Include only tagged exports
pnpm knip --tags +lintignore
```

**Or in config:**
```json
{
  "tags": ["-lintignore", "-internal"]
}
```

---

## Debugging & Tracing

```bash
# Debug mode (shows workspaces, configs, plugins, resolved files)
pnpm knip --debug

# Trace all exports (see where they're used)
pnpm knip --trace

# Trace specific file
pnpm knip --trace-file src/utils.ts

# Trace specific export
pnpm knip --trace-export myFunction

# Trace specific export in specific file
pnpm knip --trace-file src/utils.ts --trace-export myFunction

# Performance profiling (shows function timings)
pnpm knip --performance

# Profile specific function
pnpm knip --performance-fn resolveSync

# Memory usage monitoring
pnpm knip --memory

# Memory stats even on crash
pnpm knip --memory-realtime

# No progress output (for CI)
pnpm knip --no-progress
```

---

## Performance Optimization

```bash
# Enable caching (10-40% faster on consecutive runs)
pnpm knip --cache

# Custom cache location
pnpm knip --cache-location ./custom-cache

# Analyze single workspace (faster in monorepos)
pnpm knip --workspace packages/core

# Watch mode (live updates on file changes)
pnpm knip --watch

# Watch + auto-fix
pnpm knip --watch --fix
```

---

## Exit Codes

- `0` - No issues found
- `1` - Issues found
- `2` - Configuration error

Use in CI/CD:

```bash
# Fail CI if issues found
pnpm knip || exit 1

# Allow issues but save report
pnpm knip --reporter json > temp/knip.json || true

# Always exit with code 0 (don't fail CI)
pnpm knip --no-exit-code

# Set max issues threshold
pnpm knip --max-issues 10

# Treat config hints as errors
pnpm knip --treat-config-hints-as-errors

# Suppress config hints
pnpm knip --no-config-hints
```

---

## Advanced Features

### External Library Support

```bash
# Include external library type definitions
# Useful for false positives with external libraries
pnpm knip --include-libs
```

### Entry File Exports

```bash
# Report unused exports in entry files
# Useful for private/self-contained repos
pnpm knip --include-entry-exports
```

### Preprocessors

```bash
# Preprocess results before reporting
pnpm knip --preprocessor ./my-preprocessor.ts

# With options
pnpm knip --preprocessor ./preproc.ts --preprocessor-options '{"key":"value"}'
```

### Plugin Configuration

```json
{
  "mocha": {
    "config": "config/mocha.config.js",
    "entry": ["**/*.spec.js"]
  },
  "playwright": true,   // Force enable
  "webpack": false      // Disable
}
```

### Path Aliases

```json
{
  "paths": {
    "@lib": ["./lib/index.ts"],
    "@lib/*": ["./lib/*"],
    "@components": ["./src/components/index.ts"]
  }
}
```

### Bun Runtime

```bash
# Run Knip with Bun instead of Node.js
knip-bun

# Equivalent to:
bunx --bun knip
```

### Compilers

Knip has built-in compilers for:
- `.astro` - Astro components
- `.mdx` - MDX files
- `.svelte` - Svelte components
- `.vue` - Vue components

**Custom compilers:**
```json
{
  "compilers": {
    ".css": "(text) => [...extractImports(text)]"
  }
}
```

---

## JSDoc/TSDoc Tags Reference

### Standard Tags

**`@public`** - Mark as public API (won't be reported as unused)
```ts
/** @public */
export const publicAPI = () => {};
```

**`@internal`** - Mark as internal (ignored in `--production` mode)
```ts
/** @internal */
export const internalUtil = () => {};
```

**`@beta`** - Mark as beta feature
```ts
/** @beta */
export const betaFeature = () => {};
```

**`@alpha`** - Mark as alpha feature
```ts
/** @alpha */
export const alphaFeature = () => {};
```

**`@alias`** - Handle duplicate exports
```ts
/** @alias */
export { default as AliasName } from './module';
```

### Custom Tags

Use arbitrary tags for flexible filtering:

```ts
/** @lintignore */
export const customIgnored = () => {};

/** @experimental */
export const experimentalFeature = () => {};
```

**Filter by tags:**
```bash
# Exclude tagged exports
pnpm knip --tags -lintignore,-internal

# Include only tagged exports
pnpm knip --tags +experimental
```

**In config:**
```json
{
  "tags": ["-lintignore", "-internal", "+experimental"]
}
```

---

## Additional Workflows

### Workflow 4: Namespace Exports Analysis

```bash
# Enable namespace export checking
pnpm knip --include nsExports,nsTypes

# Save to report
pnpm knip --include nsExports,nsTypes --reporter json > temp/knip-ns.json
```

### Workflow 5: Class Members Analysis

```bash
# Enable class member checking (opt-in, can be slow)
pnpm knip --include classMembers

# With performance monitoring
pnpm knip --include classMembers --performance
```

### Workflow 6: Watch Mode Development

```bash
# Start watch mode
pnpm knip --watch

# Watch + auto-fix (use with caution)
pnpm knip --watch --fix --include dependencies

# Watch specific workspace
pnpm knip --watch --workspace packages/core
```

### Workflow 7: External Library Integration

```bash
# When getting false positives from external libraries
pnpm knip --include-libs

# Combine with specific issue types
pnpm knip --include-libs --include exports,types
```

---

## Best Practices for LLM Agents

### 1. Always Review Before Auto-Fix
```bash
# ✅ CORRECT: Review first
pnpm knip --reporter json > temp/knip.json
bat temp/knip.json
pnpm knip --fix-type dependencies

# ❌ WRONG: Auto-fix without review
pnpm knip --fix
```

### 2. Start with Safest Fixes
```bash
# Safest: dependencies only
pnpm knip --fix-type dependencies

# Medium: exports and types
pnpm knip --fix-type exports,types

# Riskiest: files (requires explicit flag)
pnpm knip --fix --allow-remove-files --include files
```

### 3. Use Production Mode for Shipping Code
```bash
# Analyze only production code
pnpm knip --production

# Strict mode for libraries
pnpm knip --strict
```

### 4. Debug False Positives
```bash
# Understand what Knip sees
pnpm knip --debug

# Trace why export is unused
pnpm knip --trace-file src/utils.ts --trace-export myFunction

# Check external library integration
pnpm knip --include-libs
```

### 5. Optimize Performance
```bash
# Enable caching for faster runs
pnpm knip --cache

# Analyze single workspace in monorepos
pnpm knip --workspace packages/core

# Monitor performance
pnpm knip --performance
```

### 6. Handle False Positives Gracefully
```bash
# Use rules to downgrade to warnings
# In knip.json:
{
  "rules": {
    "exports": "warn"  // Print but don't count
  }
}

# Or exclude specific types
pnpm knip --exclude exports,types

# Or use JSDoc tags
/** @public */
export const keepThis = () => {};
```

---

## Quick Reference

```bash
# Basic
pnpm knip                                    # Run analysis
pnpm knip --help                             # Show help
pnpm knip --version                          # Show version

# Filtering
pnpm knip -W packages/core                   # Specific workspace
pnpm knip --include files,dependencies       # Specific types
pnpm knip --exclude exports                  # Exclude types

# Output
pnpm knip --reporter json > temp/knip.json   # JSON report
bat temp/knip.json                           # View with syntax highlighting

# Production
pnpm knip --production                       # Production mode
pnpm knip --strict                           # Strict mode

# Auto-fix (⚠️ destructive)
pnpm knip --fix-type dependencies            # Fix dependencies only
pnpm knip --fix --allow-remove-files         # Fix including files
pnpm knip --fix --format                     # Fix + auto-format

# Performance
pnpm knip --cache                            # Enable caching
pnpm knip --watch                            # Watch mode

# Debugging
pnpm knip --debug                            # Debug mode
pnpm knip --trace-file src/utils.ts          # Trace specific file
pnpm knip --performance                      # Performance metrics
pnpm knip --memory                           # Memory usage

# Advanced
pnpm knip --include-libs                     # External library support
pnpm knip --include-entry-exports            # Check entry file exports
pnpm knip --tags -lintignore,-internal       # Filter by JSDoc tags
```
