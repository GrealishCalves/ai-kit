## dependency-cruiser - Advanced Dependency Analysis & Architectural Validation

### **What It Does**

- Validates module dependencies against architectural rules
- Detects circular dependencies with detailed paths
- Identifies orphaned modules (dead code)
- Prevents production code from depending on devDependencies or test utilities
- Catches unresolvable dependencies before build failures
- Generates visual dependency graphs (JSON, HTML, DOT, SVG)

### ✅ **Safety: Read-Only Analysis**

**All commands in this guide are read-only and safe.** dependency-cruiser only analyzes your code structure and reports violations. It never modifies, deletes, or fixes files automatically. You must manually review all findings and decide what changes to make.

### 📁 **Output Directory**

**All reports are saved to `/temp` directory** to keep the project root clean. The `/temp` directory is gitignored and safe to delete at any time. Reports include:

- `temp/deps.json` - JSON dependency graph
- `temp/deps.svg` - Visual dependency graph
- `temp/deps.html` - HTML dependency report
- `temp/architecture.svg` - High-level architecture diagram

### **Why Use It**

- **Architectural validation:** Enforces custom rules about module dependencies and boundaries
- **Complements dead code tools:** While Knip finds unused exports, dependency-cruiser validates architectural rules
- **Prevents build failures:** Catches unresolvable dependencies before deployment
- **Enforces boundaries:** Prevents production code from importing test utilities or devDependencies
- **Detects circular deps:** Finds circular dependencies that cause runtime issues and maintenance problems
- **Monorepo-aware:** Understands workspace structure and cross-package dependencies
- **Visual insights:** Generates dependency graphs to understand codebase structure

### **Quick Start**

```bash
# Validate entire codebase
pnpm deps:check

# Validate specific directory
depcruise --validate .dependency-cruiser.cjs src/components

# Generate architecture diagram (requires graphviz: brew install graphviz)
pnpm deps:graph
```

### **Essential Commands**

```bash
# Validate all source directories
depcruise --validate .dependency-cruiser.cjs src

# Validate specific directory
depcruise --validate .dependency-cruiser.cjs src/components

# Output validation errors only
depcruise --validate .dependency-cruiser.cjs src --output-type err

# Generate JSON report for analysis
depcruise --validate .dependency-cruiser.cjs src --output-type json > temp/deps.json

# Generate visual graph (requires graphviz: brew install graphviz)
depcruise src --output-type dot | dot -T svg > temp/deps.svg

# Generate high-level architecture diagram
depcruise src --output-type archi | dot -T svg > temp/architecture.svg

# Generate HTML report
depcruise src --output-type html > temp/deps.html

# Check specific subdirectory
depcruise --validate .dependency-cruiser.cjs src/utils

# Focus on specific violations
depcruise --validate .dependency-cruiser.cjs src --output-type err | rg "no-circular"
```

### **Current Configuration**

The project uses `.dependency-cruiser.cjs` with the following rules:

**Extends:** `dependency-cruiser/configs/recommended` (includes best practices)

**Custom Rules:**

- ✅ `no-circular` (error) - Prevents circular dependencies (excludes generated files)
- ⚠️ `no-orphans` (warn) - Detects dead code (excludes config files, scripts, build artifacts)
- ✅ `no-test-dependencies` (error) - Production code can't import test utilities
- ✅ `not-to-dev-dep` (error) - Production code can't depend on devDependencies (excludes devtools)
- ⚠️ `no-duplicate-dep-types` (warn) - Flags dependencies in multiple package.json sections

**Monorepo Support:**

- Collapses workspace directories in architecture diagrams
- Handles scoped packages (`@org/package`) correctly
- TypeScript path mapping support via `tsconfig.json`

### **Configuration Example**

```javascript
/** @type {import('dependency-cruiser').IConfiguration} */
module.exports = {
  extends: "dependency-cruiser/configs/recommended",
  forbidden: [
    {
      name: "no-circular",
      severity: "error",
      comment: "Circular dependencies cause bugs and make code hard to test",
    },
    {
      name: "not-to-unresolvable",
      severity: "error",
      comment: "Unresolvable dependencies will break the build",
      from: {},
      to: { couldNotResolve: true },
    },
    {
      name: "not-to-dev-dep",
      severity: "error",
      comment: "Production code shouldn't depend on devDependencies",
      from: {
        path: "^src",
        pathNot: "\\.test\\.|test/|\\.spec\\.|spec/",
      },
      to: { dependencyTypes: ["npm-dev"] },
    },
  ],
  options: {
    tsPreCompilationDeps: true,
    tsConfig: { fileName: "tsconfig.json" },
    reporterOptions: {
      archi: {
        collapsePattern: "^(src/[^/]+)",
      },
    },
  },
};
```

### **Analyze Dependency Health**

```bash
# Full dependency health check
depcruise --validate .dependency-cruiser.cjs src --output-type err

# Count violations by type
depcruise --validate .dependency-cruiser.cjs src --output-type json | \
  jaq '.summary.violations | group_by(.rule.name) | map({rule: .[0].rule.name, count: length})'

# Find most coupled files
depcruise src --output-type json | \
  jaq '[.modules[] | {file: .source, deps: (.dependencies | length)}] | sort_by(.deps) | reverse | .[0:10]'

# Find circular dependency chains
depcruise --validate .dependency-cruiser.cjs src --output-type json | \
  jaq '.summary.violations | map(select(.rule.name == "no-circular"))'

# Calculate average coupling
depcruise src --output-type json | \
  jaq '[.modules[] | .dependencies | length] | add / length'

# Find production code depending on devDependencies
depcruise --validate .dependency-cruiser.cjs src --output-type json | \
  jaq '.summary.violations | map(select(.rule.name == "not-to-dev-dep"))'
```

### **Directory-Specific Analysis**

```bash
# Specific directory
depcruise --validate .dependency-cruiser.cjs src/components

# Specific subdirectory
depcruise --validate .dependency-cruiser.cjs src/utils

# All source directories
depcruise --validate .dependency-cruiser.cjs src
```

### **Integration with pnpm Scripts**

Add to root `package.json`:

```json
{
  "scripts": {
    "deps:check": "depcruise --validate .dependency-cruiser.cjs src",
    "deps:graph": "depcruise src --output-type archi | dot -T svg > temp/architecture.svg"
  }
}
```

Then run:

```bash
pnpm deps:check
pnpm deps:graph
```

---

### 🔒 **Safety Reminder**

dependency-cruiser is a **read-only analysis tool**. It will never modify your code. After reviewing violations:

1. **Manually review** each violation in the report
2. **Understand the architectural issue** before making changes
3. **Refactor code manually** to fix violations
4. **Re-run validation** to confirm fixes
5. **Run tests** to ensure refactoring didn't break functionality

### **Comparison with Other Tools**

| Tool                   | Purpose                                              | When to Use                               |
| ---------------------- | ---------------------------------------------------- | ----------------------------------------- |
| **dependency-cruiser** | Validates architectural rules, detects circular deps | Enforce boundaries, prevent circular deps |
| **Knip**               | Finds unused exports, dependencies, files            | Remove dead code                          |
| **ts-prune**           | Finds unused TypeScript exports                      | TypeScript-specific dead code             |
| **jscpd**              | Detects code duplication                             | Find copy-paste code                      |
| **Biome**              | Linting and formatting                               | Code style and quality                    |

**Use dependency-cruiser when:**

- You need to enforce architectural boundaries
- You want to prevent circular dependencies
- You need to ensure production code doesn't import test utilities
- You want to catch unresolvable dependencies before deployment
