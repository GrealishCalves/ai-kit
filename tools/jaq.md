## jaq - JSON Querying & Transformation

### **What It Does**

- Parse, query, and transform JSON with jq-compatible syntax
- Streamline CLI workflows that need structured data
- Convert JSON into targeted text, CSV-style rows, or aggregated reports

### **Why Use It**

- **jq-compatible filters:** Reuse existing jq knowledge and examples
- **Script friendly:** `-r`, `--slurp`, and `--compact-output` make automation easy
- **Rust performance:** Fast execution, small binary, no runtime dependencies
- **Reliable parsing:** Validates JSON/YAML responses before automation relies on them
- **Great in pipelines:** Pair with curl, ripgrep, Knip, or custom scripts for data wrangling

### **Essential Commands**

```bash
# Pretty-print JSON (reads from STDIN or file)
cat package.json | jaq '.'
jaq '.' schema.json

# Extract a single field as raw text
jaq -r '.version' package.json

# Select nested properties
jaq '.scripts | keys' package.json

# Process NDJSON/line-delimited JSON
cat logs.ndjson | jaq '.timestamp + " " + .message'

# Merge multiple files
jaq --slurp '.[] | {name, version}' packages/*/package.json

# Convert YAML to JSON then filter (via yq)
yq -o=json '.services' docker-compose.yml | jaq '.[] | .name'
```

### **Filter Patterns & Recipes**

```bash
# Keep only matching objects
jaq '.users[] | select(.role == "admin")' data/users.json

# Project specific fields
jaq '.dependencies | to_entries[] | "\(.key)@\(.value)"' package.json

# Flatten arrays and pick unique values
jaq '.projects | map(.tags) | add | unique' data/projects.json

# Guard against missing paths
jaq '.?.metadata?.owner // "unknown"' service.json

# Transform into CSV-style rows
jaq -r '.[] | "\(.id),\(.status),\(.score)"' reports/results.json

# Aggregate counts by key
jaq '.items | group_by(.status) | map({status: .[0].status, count: length})'

# Build lookup maps
jaq '.users | map({key: .email, value: .id}) | from_entries' users.json
```

### **Integration with Other Tools**

```bash
# Inspect API responses
curl -s https://api.example.com/projects | jaq '.data[] | {id, name}'

# Verify Knip report entries
cat temp/knip.json | jaq -r '.files[] | .filePath' > temp/knip-unused.txt

# Combine ripgrep findings with structured filters
rg --json "console\." src | jaq '.data | {path, lines}'

# Summarize Tavern/Tavily output
cat temp/tavily-response.json | jaq '.results | map({url, title})'

# Extract package versions for pnpm audit
pnpm list --json | jaq '.[] | {name, version}'

# Build QA checklist from Linear export
cat exports/linear/issues.json | jaq '.[] | select(.state.title == "In Progress") | .title'
```

### **Advanced Flags & Tips**

- `-r / --raw-output`: Output plain text (skip JSON quoting) for shell scripting
- `-c / --compact-output`: Emit minified JSON, ideal for logs
- `--slurp`: Read entire input as an array, enabling multi-file aggregation
- `--stream`: Process large payloads incrementally to reduce memory usage
- `--sort-keys`: Normalize object key ordering for diffs/comparisons
- `--color`: Force colorized output (pair with `less -R`)
- `--null-input (-n)`: Run filters without input (generate JSON programmatically)

```bash
# Generate JSON without an input file
jaq -n '{timestamp: now, status: "ok"}'

# Normalize for deterministic diffs
jaq --sort-keys '.' env/config.json | sponge env/config.json

# Stream huge datasets (avoids loading whole file)
cat massive.json | jaq --stream 'paths'
```

### **Practical Workflows**

```bash
# Confidence scoring for dead code cleanup
npx knip --include files --reporter json > temp/knip.json
cat temp/knip.json | jaq -r '.files[].filePath' | sort > temp/knip-unused.txt

# Compare API environments
diff <(curl -s https://api.dev/app | jaq --sort-keys '.') \
     <(curl -s https://api.prod/app | jaq --sort-keys '.')

# Extract failing test names from Playwright JSON
cat artifacts/playwright-results.json | jaq -r '.suites[].specs[].tests[] | select(.results[].status != "passed") | .title'

# Build release notes from changelog JSON
cat artifacts/changelog.json | jaq -r '.releases[0].notes[] | "- \(.title)"'

# Inspect contract ABIs for specific functions
cat apps/hardhat/ignition/deployments/**/*.json | jaq -r 'select(.abi? != null) | .abi[] | select(.type == "function") | .name' | sort -u
```

### **Troubleshooting**

- Validate input upstream: jaq fails fast on malformed JSON—add `jq`/`yq` conversion when sourcing YAML or mixed data
- Remember to quote filters in shells to avoid globbing or variable expansion (`jaq '.items[] | .name'`)
- Use `--raw-output` when piping into tools that expect plain text (e.g., `xargs`, `sort`)
- For complex filters, author them in separate `.jq` files and load with `jaq -f script.jq`
