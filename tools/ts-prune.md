## ts-prune - Unused Export Detection

### **What It Does**

- Detects unused exports in TypeScript/JavaScript files
- Finds unused re-exports in index.ts files
- Works at export granularity (not just file level)
- Identifies dead code that other tools miss

### ✅ **Safety: Read-Only Analysis**

**All commands in this guide are read-only and safe.** ts-prune only analyzes your exports and reports which ones are unused. It never modifies, deletes, or removes code automatically. You must manually review each unused export and decide whether to remove it.

### 📁 **Output Directory**

**Reports can be saved to `/temp` directory** to keep the project root clean. The `/temp` directory is gitignored and safe to delete at any time. Example:

- `temp/unused_exports.txt` - Full list of unused exports
- `temp/unused_export_files.txt` - List of files with unused exports

**Manual Review Workflow:**

1. Run `npx ts-prune` to generate report
2. Review each unused export carefully
3. Verify the export is truly unused (not used dynamically or by external packages)
4. Manually delete the unused export from the source file
5. Run tests to ensure nothing breaks

### **Why Use It**

- **Export-level granularity:** Detects specific unused functions, classes, types within files
- **Complements file-level tools:** A file can be imported but still have unused exports
- **Reduces bundle size:** Remove unused exports to reduce build output and improve tree-shaking
- **Improves maintainability:** Less code to maintain and understand
- **API cleanup:** Helps identify and remove deprecated or unused public APIs

### **Essential Commands**

```bash
# Find all unused exports
npx ts-prune

# Save to file for analysis (outputs to temp/)
npx ts-prune > temp/unused_exports.txt

# Filter by specific directory
npx ts-prune | grep "src/components"

# Count unused exports
npx ts-prune | wc -l

# Exclude test files
npx ts-prune | grep -v ".test.ts" | grep -v ".spec.ts"

# Find unused exports in specific directory
npx ts-prune | grep "src/utils"

# Show only files with unused exports
npx ts-prune | cut -d: -f1 | sort | uniq

# Count unused exports per file
npx ts-prune | cut -d: -f1 | sort | uniq -c | sort -rn
```

### **Configuration File**

Create `.ts-prunerc` in project root:

**IMPORTANT:** The `ignore` field expects a **single regex pattern string**, not an array of glob patterns.

```json
{
  "ignore": "src/index\\.(ts|tsx)$|\\.(test|spec)\\.(ts|tsx)$|types/global\\.d\\.ts$|scripts/.*\\.ts$"
}
```

**Pattern Explanation:**

- `src/index\\.(ts|tsx)$` - Ignores main entry files
- `\\.(test|spec)\\.(ts|tsx)$` - Ignores all test/spec files
- `types/global\\.d\\.ts$` - Ignores global type definitions
- `scripts/.*\\.ts$` - Ignores script files

### **Example Output**

```bash
$ ts-prune

src/utils/math.ts:5 - multiply (unused)
src/utils/math.ts:10 - divide (unused)
src/components/Button/index.ts:2 - ButtonVariant (unused type)
src/services/api.ts:10 - legacyFetchData (unused)
src/hooks/useAuth.ts:15 - validateToken (unused)
```

### **Common Patterns**

```bash
# Filter by directory
npx ts-prune | grep "src/utils"

# Count unused exports
npx ts-prune | wc -l

# Find files with most unused exports
npx ts-prune | cut -d: -f1 | sort | uniq -c | sort -rn | head -10

# Cross-reference with unimported files (outputs to temp/)
npx ts-prune | cut -d: -f1 | sort | uniq > temp/unused_export_files.txt
npx unimported | grep "unimported files" -A 100 | grep ".ts" > temp/unimported_files.txt
comm -12 temp/unused_export_files.txt temp/unimported_files.txt
```

---

### 🔒 **Safety Reminder**

ts-prune is a **read-only analysis tool**. It will never modify your code. After reviewing unused exports:

1. **Manually review** each unused export in the report
2. **Verify it's truly unused** (check for dynamic imports, external package usage)
3. **Consider if it's part of a public API** (libraries should keep public exports)
4. **Manually delete** the unused export from the source file
5. **Run tests** to ensure nothing breaks
6. **Re-run ts-prune** to verify the export is gone

**False Positives to Watch For:**

- Exports used by external packages (if you're building a library)
- Exports used dynamically via `import()` or `require()`
- Exports used in configuration files or scripts
- Type exports used only in `.d.ts` files
