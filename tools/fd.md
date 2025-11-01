## fd - Fast File Discovery

### **What It Does**

- Locate files and directories using intuitive patterns
- Recursively search while respecting `.gitignore`
- Replace slow, verbose `find` invocations for day-to-day work

### **Why Use It**

- **Human-friendly syntax:** `fd pattern path` instead of complex flags
- **Blazing speed:** Written in Rust with parallel directory traversal
- **Smart defaults:** Ignores hidden, `.git`, and `node_modules` unless asked
- **Regex & glob support:** Choose the matching style that suits the task
- **Great in pipelines:** Outputs clean paths for `xargs`, scripts, and LLM-ready summaries

### **Essential Commands**

```bash
# Basic search (recursive from cwd)
fd prisma

# Limit to directories or files
fd -t d seeder apps/
fd -t f 'config\.ts'

# Search specific extensions
fd -e ts -etsx 'schema' src/

# Case-insensitive search
fd -i 'production'

# Show absolute paths
fd prisma --absolute-path

# Dry-run delete candidates (pairs nicely with xargs)
fd -0 '.*\.log$' tmp/ | xargs -0 ls -lh
```

### **Pattern Recipes**

```bash
# Find test files missing .spec suffix
fd -e ts --ignore-file .gitignore 'tests?/$' src | fd --exclude '*.spec.ts'

# Locate configuration files
fd --glob 'config*.{js,ts}' apps/

# List directories named migrations
fd -t d migrations

# Match files containing dates like 2024-xx-xx
fd '\d{4}-\d{2}-\d{2}' reports/

# Filter by size or depth (via fd + du/find hybrid)
fd -I -d 3 -t f '' src | xargs -I{} du -h {}
```

### **Integration with Other Tools**

```bash
# Inspect matches with bat
fd -e md 'api' docs | xargs bat

# Run ripgrep only on matching files
fd -e ts 'service' src | xargs rg 'TODO'

# Feed to jaq for structured reports
fd -e json 'schema' apps | jaq -Rnc '[inputs | {path: ., size: (statpath(.)|.size)}]'

# Generate pnpm selective install list
fd -e package.json --min-depth 2 packages | xargs pnpm install --filter

# Cross-check Knip unused files
fd -e ts src | comm -23 <(sort -u) temp/knip-unused.txt
```

### **Advanced Flags & Tips**

- `-H / --hidden`: include hidden files (still respects `.gitignore`)
- `-I / --no-ignore`: ignore `.gitignore` and `.fdignore`
- `-u`: include hidden + ignored + git-ignored entries (use sparingly)
- `-d / --max-depth`: constrain recursion depth
- `--exact-depth`: match only at specific depth (e.g., `fd --exact-depth 2`)
- `-x/--exec`: run a command for each result (like `find … -exec`)
- `-0`: output NUL-separated paths for safe piping to `xargs -0`
- `--strip-cwd-prefix`: omit leading `./` for cleaner output
- `--color always`: force color for terminals or logs that support it

```bash
# Delete generated cache files (prompt each)
fd -e cache --hidden --exec rm -i '{}'

# Rename files with sd
fd -e ts 'Controller' src --exec-batch sd 'Controller' 'Api'

# Copy finds into temp folder
fd -e env --exact-depth 2 services --exec cp '{}' tmp/env-backup/
```

### **Practical Workflows**

```bash
# Audit large folders before pruning
fd -d 3 '' apps | xargs -I{} dust '{}'

# Generate review checklist for LLM
fd -e tsx 'components' apps/frontend | tee temp/component-files.txt

# Validate codegen outputs exist
fd -d 2 -t f 'index.d.ts' packages | diff -u expected-types.txt -

# Prep files for batch formatting
fd -e ts -e tsx src | xargs pnpm lint --fix --file

# Confirm Playwright screenshots were produced
fd -e png 'playwright-report' | sort
```

### **Troubleshooting**

- No results? Add `-I` to bypass `.gitignore`, or `-H` for hidden directories
- Complex regex clashes with shell quoting—wrap patterns in single quotes
- Prefer `--glob` when translating shell wildcards (`fd --glob '*.graphql'`)
- Use `fd --debug 'pattern'` to see why paths were skipped
- Paths with spaces? Pair with `-0` and `xargs -0` to avoid splitting issues

