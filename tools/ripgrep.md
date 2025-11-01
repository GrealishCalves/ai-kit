## ripgrep (rg) - Fast Text Search

### **What It Does**

- Search codebase at lightning speed
- Smart defaults (auto-ignores node_modules, respects .gitignore)
- Pattern detection (TODOs, console.logs, magic strings)
- File listing and discovery (replaces `find` and `ls`)
- Text extraction with capture groups and replacements

### **Why Use It**

- **Lightning fast:** Search entire codebase in milliseconds
- **Smart defaults:** Automatically ignores node_modules, respects .gitignore
- **Pattern detection:** Find TODOs, console.logs, hardcoded values, magic numbers
- **Cross-validation:** Verify findings from dead code detection tools (Knip)
- **Regex support:** Powerful pattern matching with regular expressions
- **Text-based search:** Complements semantic tools like jscpd for different use cases
- **File discovery:** Replaces `find`, `ls`, and `view` tool for searching
- **Extract & transform:** Capture groups with `--replace` for data extraction

### **Essential Commands**

```bash
# Basic search
rg "useState"

# Search specific file types
rg "TODO" --type ts

# Count occurrences
rg "console.log" --count

# Search with context
rg "FIXME" -C 3

# Case insensitive
rg -i "error"

# Show only filenames
rg "console.log" -l

# List all files (replaces find/ls)
rg --files
rg --files src/
rg --files --type ts
rg --files --glob '*.md'

# Invert match (exclude pattern)
rg --files | rg -v 'test'
```

### **Advanced Features & Flags**

| Flag                    | Short | Purpose                             | Example                             |
| ----------------------- | ----- | ----------------------------------- | ----------------------------------- |
| `--files`               | -     | List files (replaces `find`)        | `rg --files src/`                   |
| `--type`                | `-t`  | Filter by file type                 | `rg --type ts "useState"`           |
| `--glob`                | `-g`  | Pattern matching for files          | `rg --glob '*.md' "TODO"`           |
| `--only-matching`       | `-o`  | Show only matched text              | `rg 'https://[^\s]+' -o`            |
| `--no-line-number`      | `-N`  | Suppress line numbers               | `rg 'pattern' -N`                   |
| `--replace`             | `-r`  | Replace/extract with capture groups | `rg '(.*)' --replace '$1'`          |
| `--color=never`         | -     | Disable colors (for scripts)        | `rg 'pattern' --color=never`        |
| `--count`               | `-c`  | Count matches per file              | `rg 'TODO' --count`                 |
| `--files-with-matches`  | `-l`  | Show only filenames                 | `rg 'error' -l`                     |
| `--files-without-match` | -     | Files NOT matching pattern          | `rg --files-without-match 'import'` |
| `--context`             | `-C`  | Show N lines before/after           | `rg 'error' -C 3`                   |
| `--before-context`      | `-B`  | Show N lines before                 | `rg 'error' -B 2`                   |
| `--after-context`       | `-A`  | Show N lines after                  | `rg 'error' -A 2`                   |
| `--ignore-case`         | `-i`  | Case insensitive search             | `rg -i 'error'`                     |
| `--invert-match`        | `-v`  | Invert match (exclude)              | `rg -v 'test'`                      |
| `--no-ignore`           | -     | Don't respect .gitignore            | `rg --no-ignore 'pattern'`          |
| `--hidden`              | -     | Search hidden files                 | `rg --hidden 'pattern'`             |

### **Common Cleanup Patterns**

```bash
# Find TODOs/FIXMEs/HACKs
rg "TODO|FIXME|HACK" --count

# Find console.log statements
rg "console\.(log|warn|error|debug)" --type ts --count

# Find long string literals (potential constants)
rg -o '"[^"]{30,}"' | sort | uniq -c | sort -rn

# Find skipped tests
rg "\.only\(|\.skip\(" --type ts

# Find hardcoded URLs
rg "https?://[^\s\"']+"

# Find magic numbers
rg "\b\d{3,}\b" --type ts
```

### **Advanced Patterns & Use Cases**

#### **1. File Discovery (Replaces `find` and `ls`)**

```bash
# List all files in directory
rg --files .linear

# List only markdown files
rg --files .linear --type md
rg --files .linear --glob '*.md'

# List files excluding patterns
rg --files src | rg -v 'test|spec'

# Count total files
rg --files src | wc -l

# List files in multiple directories
rg --files apps/hardhat apps/web

# List files with specific extension
rg --files --glob '*.{ts,tsx,js,jsx}'
```

#### **2. Markdown Link Extraction & Verification**

```bash
# Find all markdown links
rg '\[.*\]\(.*\)' file.md

# Find relative markdown links
rg '\[.*\]\(\.\/[^\)]+\)' file.md

# Extract only the link URLs
rg '\[.*\]\((.*)\)' file.md --only-matching --replace '$1'

# Extract file paths from markdown links
rg '\]\(\./([^)]+\.md)\)' file.md -oN --replace '$1' | sort -u

# Verify markdown link targets exist
rg '\]\(\./([^)]+\.md)\)' .linear/guide.md -oN --replace '$1' --color=never | \
  sort -u | while read -r file; do
    [ -f ".linear/$file" ] && echo "✅ $file" || echo "❌ $file (MISSING)"
  done

# Find broken internal links
rg '\[.*\]\(\.\/([^\)]+)\)' docs/ -oN --replace '$1' | \
  while read -r path; do
    [ ! -f "docs/$path" ] && echo "Broken: $path"
  done
```

#### **3. Text Extraction with Capture Groups**

```bash
# Extract import statements
rg "import .* from ['\"](.+)['\"]" --replace '$1' -oN

# Extract function names
rg "function (\w+)\(" --replace '$1' -oN

# Extract URLs from markdown
rg '\[([^\]]+)\]\(([^\)]+)\)' --replace 'Text: $1 → URL: $2'

# Extract version numbers
rg 'version["\s:]+([0-9.]+)' --replace '$1' -oN

# Extract email addresses
rg '\b([A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,})\b' -oN

# Extract hex colors
rg '#([0-9A-Fa-f]{6}|[0-9A-Fa-f]{3})\b' -oN
```

#### **4. File Content Verification**

```bash
# Find files WITHOUT imports (potential dead code)
rg --files-without-match "import.*from|require\(" src/

# Find files WITH specific pattern
rg --files-with-matches "useState" src/

# Find empty files or files with only whitespace
rg --files src/ | while read -r f; do
  [ ! -s "$f" ] && echo "Empty: $f"
done

# Verify all TypeScript files have exports
rg --files --type ts | while read -r f; do
  rg -q "export" "$f" || echo "No exports: $f"
done
```

#### **5. Cross-File Reference Checking**

```bash
# Find all files importing from a specific module
rg "from ['\"]@/components/Button['\"]" src/

# Find all usages of a specific function
rg "\bhandleSubmit\b" --type ts

# Find all files that reference a specific file
rg "from ['\"].*config/database" .

# Check if dependency is actually used
rg "from ['\"]react-query['\"]|require\(['\"]react-query" .

# Find circular dependencies
rg "from ['\"]\.\./" src/components/ -l | \
  while read -r f; do
    echo "File: $f"
    rg "from ['\"]\.\./" "$f"
  done
```

#### **6. Counting & Statistics**

```bash
# Count occurrences per file
rg "TODO" --count

# Count total occurrences across all files
rg "TODO" --count-matches

# Count files containing pattern
rg "useState" -l | wc -l

# Count lines of code (excluding comments)
rg --type ts -v "^\s*(//|/\*|\*)" | wc -l

# Find most common imports
rg "from ['\"](.+)['\"]" --replace '$1' -oN | sort | uniq -c | sort -rn | head -20
```

#### **7. Complex Pattern Matching**

```bash
# Find TODO comments with context
rg "TODO|FIXME|HACK" -C 2

# Find functions with specific parameters
rg "function \w+\([^)]*userId[^)]*\)"

# Find React components
rg "export (default )?(function|const) \w+.*React" --type tsx

# Find environment variables
rg "process\.env\.\w+" -oN | sort -u

# Find API endpoints
rg "(GET|POST|PUT|DELETE|PATCH)\s+['\"]([^'\"]+)['\"]" --replace '$1 $2'

# Find SQL queries
rg "(SELECT|INSERT|UPDATE|DELETE)\s+.+FROM" -i
```

#### **8. Scripting & Automation**

```bash
# Generate file list for processing
rg --files src/ --type ts > files.txt

# Batch verification script
rg '\]\(\./([^)]+\.md)\)' guide.md -oN --replace '$1' --color=never | \
  sort -u | while read -r file; do
    if [ -f "$file" ]; then
      echo "✅ $file"
    else
      echo "❌ $file (MISSING)"
      exit 1
    fi
  done

# Find and replace across files (dry-run)
rg "oldFunction" -l | while read -r f; do
  echo "Would update: $f"
  rg "oldFunction" "$f" --replace "newFunction"
done

# Extract data to JSON
rg '"name":\s*"([^"]+)"' package.json --replace '$1' -oN | \
  jaq -R -s 'split("\n") | map(select(length > 0))'
```

### **When to Use `rg` vs `view` Tool**

| Task                | Use `rg`               | Use `view`                       |
| ------------------- | ---------------------- | -------------------------------- |
| Search/Extract/List | ✅ Always              | ❌ Never with search_query_regex |
| Read full file      | ❌ Use `view` or `bat` | ✅ Only for reading content      |

**Rule:** Use `rg` for searching/filtering/extracting/listing. Use `view` only for reading full file content.

### **Cross-Validation with Dead Code Tools**

ripgrep excels at verifying findings from static analysis tools like Knip:

```bash
# Verify Knip's "unused file" claim
# If file has NO imports, likely truly unused
rg "import.*from|require\(" suspected-unused-file.ts
# No output = HIGH confidence it's dead code

# Generated artifacts are commonly `.gitignore`d. Use `--no-ignore` when you must inspect them:
rg --no-ignore "from.*types|getTypes" src
# Finds type-only imports inside generated files

# Verify Knip's "unused export" claim
# Search for export usage across codebase
rg "import.*\{.*exportName.*\}" .
# No matches = HIGH confidence export is unused

# Find files that import from a specific file
rg "from ['\"].*path/to/file" .
# No matches = file is not imported anywhere

# Verify if a dependency is actually used
rg "from ['\"]package-name" .
rg "require\(['\"]package-name" .
# No matches = dependency likely unused

# Find all references to a specific function/class
rg "\bfunctionName\b" --type ts
# Count references to determine if export is used
```

**Confidence Scoring Workflow:**

```bash
# Step 1: Get unused files from Knip
npx knip --include files --reporter json > temp/knip.json
cat temp/knip.json | jaq -r '.files[] | .filePath' > temp/knip-unused.txt

# Step 2: Verify with ripgrep (files with no imports)
rg --files-without-match "import.*from|require\(" src > temp/rg-no-imports.txt

# Step 3: Find HIGH confidence deletions (flagged by both tools)
comm -12 <(sort temp/knip-unused.txt) <(sort temp/rg-no-imports.txt) > temp/high-confidence.txt

# Step 4: Find MEDIUM confidence (flagged by Knip only)
comm -23 <(sort temp/knip-unused.txt) <(sort temp/rg-no-imports.txt) > temp/medium-confidence.txt

# Step 5: Review findings
echo "HIGH confidence deletions (95%):"
bat temp/high-confidence.txt

echo "MEDIUM confidence - manual review needed (40%):"
bat temp/medium-confidence.txt
```

**Confidence Matrix:**

| ripgrep Result   | Knip Result               | Confidence | Action                    |
| ---------------- | ------------------------- | ---------- | ------------------------- |
| No imports found | Flagged as unused         | 95%        | **DELETE**                |
| Imports found    | Flagged as unused         | 20%        | **KEEP** (false positive) |
| No usage found   | Export flagged unused     | 90%        | **DELETE export**         |
| Usage found      | Export flagged unused     | 10%        | **KEEP** (false positive) |
| No references    | Dependency flagged unused | 85%        | **REMOVE dependency**     |
| References found | Dependency flagged unused | 5%         | **KEEP** (false positive) |

**Example: Verifying Config File Usage**

```bash
# Knip says: "src/config/database.ts is unused"
# Verify with ripgrep:

# Check if any file imports from config
rg "from.*config/database" .
# Output: (empty) = No imports found

# Check if config exports are used
rg "getDatabaseConfig|getConnectionString" .
# Output: src/scripts/migrate.ts:17 = Exports ARE used!

# Conclusion: FALSE POSITIVE - file is used via dynamic import
# Action: KEEP the file
```

**Example: Verifying Dead Code**

```bash
# Knip says: "src/utils/legacy-helper.ts is unused"
# Verify with ripgrep:

# Check if helper is imported anywhere
rg "from.*legacy-helper" src
# Output: (empty) = No imports found

# Check if helper functions are used
rg "legacyHelper|formatLegacy" src
# Output: (empty) = No usage found

# Conclusion: TRUE POSITIVE - safe to delete
# Confidence: 95%
# Action: DELETE the file
```

---

## Quick Reference

```bash
# File listing (replaces find/ls)
rg --files [path]                    # List all files
rg --files --type ts                 # By file type
rg --files --glob '*.md'             # By pattern
rg --files | rg -v 'test'            # Exclude pattern

# Search (replaces grep/view with search_query_regex)
rg 'pattern' [path]                  # Basic search
rg 'pattern' --type ts               # Filter by type
rg 'pattern' -i                      # Case insensitive
rg 'pattern' -l                      # Show filenames only
rg 'pattern' --count                 # Count per file

# Extract (capture groups & replace)
rg 'pattern (capture)' --replace '$1' -oN
rg '\[.*\]\((.*)\)' --replace '$1' -oN              # Extract URLs
rg '\]\(\./([^)]+\.md)\)' --replace '$1' -oN        # Extract paths

# Verify
rg --files-without-match 'import' src/              # Files WITHOUT pattern
rg --files-with-matches 'useState' src/             # Files WITH pattern

# Common patterns
rg 'pattern' -oN | sort -u                          # Unique values
rg 'pattern' -C 3                                   # With context
rg 'pattern' --color=never                          # No colors (scripts)

# Real-world: Verify markdown links
rg '\]\(\./([^)]+\.md)\)' guide.md -oN --replace '$1' --color=never | \
  sort -u | while read -r f; do
    [ -f "$f" ] && echo "✅ $f" || echo "❌ $f (MISSING)"
  done
```
