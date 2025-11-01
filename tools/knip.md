## Knip - Modern Dead Code Detection for Monorepos

### **What It Does**

- Finds unused files, dependencies, and exports (combines ts-prune + unimported functionality)
- Monorepo-aware with workspace support (designed for Turborepo, pnpm workspaces)
- Detects unused dependencies, devDependencies, and peer dependencies
- Identifies duplicate exports and type definitions
- Lower false positive rate than legacy tools

### ✅ **Safety: Read-Only Analysis**

**All commands in this guide are read-only and safe.** Knip only analyzes your code and generates reports. It never modifies, deletes, or removes files/dependencies automatically. You must manually review each finding and decide whether to act on it.

### 📁 **Output Directory**

**Reports can be saved to `/temp` directory** to keep the project root clean. The `/temp` directory is gitignored and safe to delete at any time. Example:

- `temp/knip.json` - Full JSON report with all findings
- `temp/knip-unused-files.txt` - List of unused files only
- `temp/knip-unused-deps.txt` - List of unused dependencies only

### **Why Use Knip Over ts-prune/unimported**

| Feature               | Knip       | ts-prune            | unimported |
| --------------------- | ---------- | ------------------- | ---------- |
| Monorepo support      | ✅         | ❌                  | ❌         |
| Unused exports        | ✅         | ✅                  | ❌         |
| Unused dependencies   | ✅         | ❌                  | ✅         |
| Unused files          | ✅         | ❌                  | ✅         |
| Active maintenance    | ✅         | ⚠️ Maintenance mode | ✅         |
| Entry point detection | ✅✅ Smart | ⚠️ Manual           | ⚠️ Manual  |
| Workspace-aware       | ✅         | ❌                  | ❌         |
| Duplicate detection   | ✅         | ❌                  | ❌         |
| Production mode       | ✅         | ❌                  | ❌         |

**Key Advantages:**

- **Better Entry Point Detection:** Automatically detects test files, scripts, and build entry points
- **Workspace-Aware:** Understands monorepo structure (Turborepo, pnpm workspaces, Nx)
- **Comprehensive:** Single tool replaces ts-prune + unimported
- **Lower False Positives:** Smarter analysis reduces manual review time significantly
- **Active Development:** Regular updates, bug fixes, new features

### **Essential Commands**

```bash
# Run analysis on entire codebase
npx knip

# Run analysis on specific workspace (if using monorepo)
npx knip --workspace packages/core

# Generate JSON report (outputs to temp/)
npx knip --reporter json > temp/knip.json

# Production mode (ignore dev dependencies and test files)
npx knip --production

# Show only specific issue types
npx knip --include files,dependencies

# Exclude specific issue types
npx knip --exclude exports,types

# Fix auto-fixable issues (use with caution)
npx knip --fix

# Show configuration being used
npx knip --debug

# Performance mode (faster, less accurate)
npx knip --performance
```

### **Configuration File**

Example `knip.json` configuration:

```json
{
  "$schema": "https://unpkg.com/knip@5/schema.json",
  "workspaces": {
    "packages/core": {
      "entry": [
        "src/index.ts", // Main entry point
        "scripts/**/*.{js,ts}", // CLI scripts
        "test/**/*.test.ts", // Unit tests
        "test/**/*.e2e.test.ts" // E2E tests
      ],
      "project": ["**/*.{ts,js}"],
      "ignore": [
        "dist/**", // Build output
        "coverage/**", // Test coverage
        ".cache/**" // Cache directory
      ]
    }
  },
  "ignore": ["**/node_modules/**", "**/dist/**"],
  "ignoreDependencies": ["@types/*", "typescript"],
  "rules": {
    "files": "error", // Unused files = error
    "dependencies": "warn", // Unused deps = warning
    "exports": "error" // Unused exports = error
  }
}
```

### **Example Output**

```bash
$ npx knip

packages/core (4 issues)

  Unused files (2)
    src/utils/deprecated.ts
    src/helpers/legacy.ts

  Unused dependencies (1)
    moment

  Unused exports (1)
    src/utils/helpers.ts:15 - unusedFunction

packages/ui (1 issue)

  Unused files (1)
    src/components/OldComponent.tsx
```

### **Cross-Validation Workflow**

Combine Knip with ripgrep for high-confidence dead code detection:

```bash
# Step 1: Run Knip and save JSON report
npx knip --reporter json > temp/knip.json

# Step 2: Extract unused files from Knip
cat temp/knip.json | jaq -r '.files[] | .filePath' > temp/knip-unused-files.txt

# Step 3: Verify with ripgrep (files with no imports)
rg --files-without-match "import.*from|require\(" src > temp/rg-no-imports.txt

# Step 4: Find intersection (HIGH confidence deletions)
comm -12 <(sort temp/knip-unused-files.txt) <(sort temp/rg-no-imports.txt) > temp/high-confidence-deletions.txt

# Step 5: Review high-confidence deletions
bat temp/high-confidence-deletions.txt
```

**Confidence Scoring:**

| Knip Result   | ripgrep Result   | Confidence | Action                    |
| ------------- | ---------------- | ---------- | ------------------------- |
| Unused file   | No imports found | 95%        | **DELETE**                |
| Unused file   | Imports found    | 20%        | **KEEP** (false positive) |
| Unused export | No usage found   | 90%        | **DELETE export**         |
| Unused export | Usage found      | 10%        | **KEEP** (false positive) |

### **Common Patterns**

```bash
# Find unused files only
npx knip --include files

# Find unused dependencies only
npx knip --include dependencies

# Find unused exports only
npx knip --include exports

# Count total issues
npx knip --reporter json | jaq '.files | length'

# Group by workspace
npx knip --reporter json | jaq 'group_by(.workspace) | map({workspace: .[0].workspace, count: length})'

# Find files with most unused exports
npx knip --reporter json | jaq '.exports | group_by(.filePath) | map({file: .[0].filePath, count: length}) | sort_by(.count) | reverse | .[0:10]'
```

### **Migration from ts-prune + unimported**

```bash
# Old workflow (2 tools, high false positives)
npx ts-prune > temp/ts-prune.txt
npx unimported > temp/unimported.txt
# Manual cross-reference required
# ~94% false positive rate on test helpers

# New workflow (1 tool, lower false positives)
npx knip --reporter json > temp/knip.json
# Single source of truth
# ~10% false positive rate (properly configured)
```

### **Handling False Positives**

**Ignore specific files:**

```json
{
  "ignore": ["src/legacy/**", "scripts/temp-*.ts"]
}
```

**Ignore specific exports (inline comment):**

```typescript
// @knip-ignore-next
export const intentionallyUnused = () => {};
```

**Ignore specific dependencies:**

```json
{
  "ignoreDependencies": ["@types/*", "eslint-*"]
}
```

### **Production Mode**

Use production mode to find code/dependencies only used in development:

```bash
# Find dev-only dependencies
npx knip --production

# Mark files as production-only in config
{
  "entry": ["src/index.ts!"],  // ! = production entry
  "project": ["src/**/*.ts!", "!src/test-helpers/**!"]
}
```

---

### 🔒 **Safety Reminder**

Knip is a **read-only analysis tool**. It will never modify your code. After reviewing the report:

1. **Manually review** each finding in the report
2. **Verify it's truly unused** (check for dynamic imports, external package usage)
3. **Consider if it's part of a public API** (libraries should keep public exports)
4. **Use cross-validation** (combine with ripgrep for higher confidence)
5. **Manually delete** the unused code/dependency
6. **Run tests** to ensure nothing breaks
7. **Re-run Knip** to verify the issue is resolved

**Common False Positives:**

- Exports used by external packages (if you're building a library)
- Exports used dynamically via `import()` or `require()`
- Files used in configuration or build scripts
- Type exports used only in `.d.ts` files
- Dependencies used by build tools (webpack, vite, etc.)

**When in Doubt, Don't Delete.** It's better to keep unused code than to break production.
