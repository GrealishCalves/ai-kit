## jscpd - Semantic Code Duplication Detection

### **What It Does**

- Detects semantic/structural code duplication (AST-based)
- Finds similar logic with different variable names
- Identifies copy-paste code blocks
- Generates detailed duplication reports (JSON, HTML, XML)
- Supports multiple languages (JS, TS, Python, Java, etc.)

### ✅ **Safety: Read-Only Analysis**

**All commands in this guide are read-only and safe.** jscpd only analyzes your code for duplication patterns and generates reports. It never modifies, deletes, or fixes files automatically. You must manually review duplicates and decide whether to refactor them.

### 📁 **Output Directory**

**All reports are saved to `/temp/jscpd-report/` directory** to keep the project root clean. The `/temp` directory is gitignored and safe to delete at any time. Reports include:

- `temp/jscpd-report/jscpd-report.html` - Interactive HTML report (PRIMARY)
- `temp/jscpd-report/jscpd-report.json` - JSON report for scripting
- Console output for quick overview

### **Why Use It**

- **Semantic analysis:** Finds structural duplication, not just text matching (detects same logic with different variable names)
- **Complements text search:** Discovers duplication that ripgrep and grep cannot detect
- **Refactoring opportunities:** Identifies code that should be consolidated into shared utilities
- **Reduces maintenance burden:** Less duplicated code = easier to maintain and update
- **Improves code quality:** Enforces DRY (Don't Repeat Yourself) principles
- **Multi-language support:** Works across JavaScript, TypeScript, Python, Java, and more

### **Essential Commands**

```bash
# Detect duplication in directory (uses .jscpd.json config)
npx jscpd src/

# Set minimum thresholds
npx jscpd src/ --min-lines 5 --min-tokens 50

# Generate HTML report
npx jscpd src/ --reporters html

# Generate JSON + HTML reports
npx jscpd src/ --reporters json,html

# Set output directory (outputs to temp/)
npx jscpd src/ --output ./temp/jscpd-report

# Set duplication threshold (fail if >3%)
npx jscpd src/ --threshold 3

# Ignore patterns
npx jscpd src/ --ignore "**/*.test.ts,**/*.spec.ts"

# Specific file formats
npx jscpd src/ --format "typescript,javascript"

# Verbose output
npx jscpd src/ --verbose

# Silent mode (only errors)
npx jscpd src/ --silent
```

### **Configuration File**

Create `.jscpd.json` in project root:

```json
{
  "threshold": 3,
  "reporters": ["html", "json", "console"],
  "ignore": ["**/*.test.ts", "**/*.spec.ts", "**/*.test.tsx", "**/*.spec.tsx", "**/node_modules/**", "**/dist/**", "**/build/**"],
  "format": ["typescript", "javascript", "tsx", "jsx"],
  "minLines": 5,
  "minTokens": 50,
  "output": "./temp/jscpd-report",
  "absolute": true,
  "gitignore": true
}
```

### **Example Output**

```bash
$ jscpd src/ --min-lines 5 --reporters console

Found 23 clones in 12 files (3.2% duplicated)

Clone #1 (15 lines, 95% similarity):
  src/utils/formatUser.ts:10-25
  src/utils/formatAdmin.ts:15-30

Clone #2 (8 lines, 87% similarity):
  src/components/Button.tsx:20-28
  src/components/Link.tsx:25-33

Clone #3 (12 lines, 100% similarity):
  src/services/api.ts:50-62
  src/services/legacy-api.ts:30-42
```

### **Interpretation**

**What jscpd detects:**

```javascript
// File A - src/utils/calculateTotal.ts
function calculateTotal(items) {
  let sum = 0;
  for (let i = 0; i < items.length; i++) {
    sum += items[i].price;
  }
  return sum;
}

// File B - src/utils/computeSum.ts
function computeSum(products) {
  let total = 0;
  for (let j = 0; j < products.length; j++) {
    total += products[j].cost;
  }
  return total;
}
```

**jscpd says:** "95% duplication - same logic, different names" ✅

**ripgrep says:** "No duplication - text doesn't match" ❌

### **Common Patterns**

```bash
# Generate HTML report (PRIMARY USE - outputs to temp/)
npx jscpd src/ --min-lines 5 --min-tokens 50 --reporters html --output ./temp/jscpd-report
open ./temp/jscpd-report/jscpd-report.html

# Find high-similarity duplicates (>90%)
npx jscpd src/ --reporters json | jaq '.duplicates[] | select(.percentage > 90)'

# Count total duplicates
npx jscpd src/ --reporters json | jaq '.statistics.total.duplicates'

# Find files with most duplicates
npx jscpd src/ --reporters json | \
  jaq '.duplicates | group_by(.firstFile.name) | map({file: .[0].firstFile.name, count: length}) | sort_by(.count) | reverse | .[0:10]'
```

---

### 🔒 **Safety Reminder**

jscpd is a **read-only analysis tool**. It will never modify your code. After reviewing duplicates:

1. **Manually review** each duplication in the HTML report
2. **Evaluate if refactoring is worth it** (some duplication is acceptable)
3. **Extract common logic** into shared functions/utilities manually
4. **Update imports** in affected files
5. **Run tests** to ensure refactoring didn't break functionality
6. **Re-run jscpd** to verify duplication percentage decreased
