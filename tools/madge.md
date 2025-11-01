## Madge - Quick Dependency Visualization

### ⚠️ **DEPRECATED - Use dependency-cruiser Instead**

**This tool is deprecated in favor of dependency-cruiser**, which provides:

- Everything madge does (graphs, circular dependencies, orphans)
- Plus: architectural rule validation, coupling metrics, better output formats
- See: `ai-kit/tools/dependency-cruiser.md` for full capabilities

**Only use madge if:**

- You need a quick PNG/SVG graph and don't have GraphViz installed
- You're already familiar with madge and need a one-off visualization

**For all other cases, use dependency-cruiser.**

### **What It Does**

- Generate visual dependency graphs (SVG/PNG)
- Detect circular dependencies
- Find orphaned files

### **Essential Commands**

**Note:** All outputs should go to `/temp` directory to keep project root clean.

```bash
# Generate visual dependency graph (outputs to temp/)
madge --image temp/deps.svg src/

# Find circular dependencies
madge --circular src/

# Find orphaned files
madge --orphans src/

# Generate JSON for analysis (outputs to temp/)
madge --json src/ > temp/deps.json

# Find high coupling files (outputs to temp/)
madge --json src/ > temp/deps.json && cat temp/deps.json | jq 'to_entries | map({file: .key, deps: (.value | length)}) | sort_by(.deps) | reverse | .[] | select(.deps > 5)'
```
