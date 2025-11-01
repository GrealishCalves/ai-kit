## unimported - Comprehensive Dead Code & Dependency Detection

⚠️ **Note:** Consider using [Knip](./knip.md) instead - it's actively maintained, monorepo-aware, and has better entry point detection with lower false positive rates. This tool is still useful but requires careful configuration.

### **What It Does**

- Finds unimported files (orphaned modules)
- Detects unused npm dependencies in package.json
- Identifies unresolved imports (potential bugs)
- Checks test files separately
- Supports TypeScript path aliases

### ✅ **Safety: Read-Only Analysis**

**All commands in this guide are read-only and safe.** unimported only analyzes your imports and dependencies, generating reports. It never modifies, deletes, or removes files/dependencies automatically.

### 📁 **Output Directory**

**Reports can be saved to `/temp` directory** to keep the project root clean. The `/temp` directory is gitignored and safe to delete at any time. Example:

- `temp/unimported.txt` - Full unimported analysis report
- `temp/unimported_files.txt` - List of unimported files
- `temp/unused_export_files.txt` - Cross-reference with ts-prune

### ⚠️ **CRITICAL WARNING: Manual Review Required**

**NEVER blindly delete files or dependencies reported by unimported.** Always follow this workflow:

**For Unimported Files:**

1. Run `npx unimported` to generate report
2. Review each unimported file carefully
3. Check if file is used dynamically (e.g., `require(dynamicPath)`, webpack contexts)
4. Check if file is an entry point for scripts, tests, or build tools
5. Verify file is truly orphaned before deletion
6. **Commit changes to git before deleting anything**
7. Manually delete the file
8. Run full test suite to ensure nothing breaks

**For Unused Dependencies:**

1. Review each unused dependency in the report
2. Check if dependency is used in build scripts, config files, or CLI tools
3. Check if dependency is a peer dependency or required by other packages
4. Verify dependency is truly unused
5. **Commit changes to git before removing anything**
6. Manually remove from package.json using `pnpm remove <package>`
7. Run `pnpm install` and test the application

**For Unresolved Imports (BUGS - Fix Immediately):**

1. These are broken imports that will cause runtime errors
2. Fix each unresolved import by correcting the path or installing missing dependencies
3. These are NOT safe to ignore

### **Why Use It**

- **Comprehensive analysis:** Finds orphaned files, unused npm packages, and broken imports
- **Catches bugs early:** Detects broken imports and unresolved dependencies before runtime
- **Reduces bundle size:** Remove unused dependencies to decrease installation time and disk usage
- **Better TypeScript support:** Handles path aliases and TypeScript-specific imports correctly
- **Dependency cleanup:** Identifies packages that can be safely removed from package.json

### **Essential Commands**

```bash
# Find all issues (unimported files + unused deps) - uses .unimportedrc.json
npx unimported

# Save to file for analysis (outputs to temp/)
npx unimported > temp/unimported.txt

# Update .unimportedrc.json config
npx unimported --init

# Clear cache and re-analyze
npx unimported --clear-cache

# Show configuration being used
npx unimported --show-config

# Show preset being used
npx unimported --show-preset
```

### **Configuration File**

Create `.unimportedrc.json` in project root:

```json
{
  "entry": ["src/index.ts", "src/index.tsx", "scripts/**/*.ts", "test/**/*.test.ts", "test/**/*.e2e.test.ts"],
  "extensions": [".ts", ".tsx", ".js", ".jsx"],
  "ignorePatterns": ["**/node_modules/**", "**/*.test.ts", "**/*.spec.ts", "**/*.test.tsx", "**/*.spec.tsx", "**/dist/**", "**/build/**", "**/.next/**", "**/coverage/**"],
  "ignoreUnresolved": ["react", "react-dom"],
  "ignoreUnused": ["@types/*", "typescript", "eslint", "prettier"],
  "respectGitignore": true
}
```

### **Critical: Configure Entry Points Correctly**

**Common Mistake:** Forgetting to add test files as entry points

**Symptom:** All test helpers flagged as "unimported" (false positives)

**Why This Happens:**

- Test files import helpers/constants/utilities
- If test files aren't entry points, unimported thinks helpers are unused
- Results in 90%+ false positive rate

**Solution:**

```json
{
  "entry": [
    "src/index.{ts,tsx}", // App entry points
    "test/**/*.test.{ts,tsx}", // Test entry points ← CRITICAL
    "test/**/*.e2e.test.{ts,tsx}", // E2E test entry points ← CRITICAL
    "scripts/**/*.{ts,js}" // Script entry points
  ]
}
```

**Verification:**

```bash
# Before fix: 33 unimported files (mostly test helpers)
npx unimported | rg "unimported files"

# After fix: ~2 unimported files (actual dead code)
npx unimported | rg "unimported files"
```

### **Example Output**

```bash
$ unimported --show-unused-deps

Unimported files:
  src/utils/oldHelper.ts
  src/components/Legacy.tsx
  src/services/deprecated.ts

Unused dependencies in package.json:
  moment (used to use, now using date-fns)
  lodash (only using lodash.debounce now)
  bluebird (replaced with native promises)

Unresolved imports (potential bugs):
  src/components/Button.tsx:5
    import { getData } from './api'
    // api.ts doesn't export getData

  src/utils/format.ts:10
    import { helper } from '@utils/helper'
    // @utils/helper doesn't exist
```

### **Common Patterns**

```bash
# Find unimported files
npx unimported | grep "unimported files" -A 100 | grep ".ts"

# Find unused dependencies
npx unimported | grep "unused dependencies" -A 100

# Check for bugs (unresolved imports - FIX IMMEDIATELY)
npx unimported | grep "unresolved imports" -A 100

# Count unimported files
npx unimported | grep "unimported files" -A 100 | grep ".ts" | wc -l

# Cross-reference with ts-prune (outputs to temp/)
npx ts-prune | cut -d: -f1 | sort | uniq > temp/unused_export_files.txt
npx unimported | grep "unimported files" -A 100 | grep ".ts" > temp/unimported_files.txt
comm -12 temp/unused_export_files.txt temp/unimported_files.txt
```

---

### 🔒 **Safety Reminder**

unimported is a **read-only analysis tool**. It will never modify your code or package.json. After reviewing the report:

**Before Deleting Anything:**

1. ✅ **Commit all changes to git** (so you can revert if needed)
2. ✅ **Review each item carefully** (see warnings above)
3. ✅ **Verify it's truly unused** (check for dynamic usage, build scripts, CLI tools)
4. ✅ **Make one change at a time** (easier to identify what broke)
5. ✅ **Run full test suite after each deletion**
6. ✅ **Test the application manually** (some usage isn't covered by tests)

**Common False Positives:**

- Files used by build tools (webpack, vite, rollup configs)
- Files used in package.json scripts
- Files used as CLI entry points
- Dependencies used by build tools or in scripts
- Peer dependencies required by other packages
- Type-only dependencies used in `.d.ts` files

**When in Doubt, Don't Delete.** It's better to keep unused code than to break production.
