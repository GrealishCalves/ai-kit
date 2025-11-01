## pnpm - Monorepo Package Manager

### **What It Does**

- Manages monorepo workspaces with filtering
- Strict node_modules (prevents phantom dependencies)
- Content-addressable storage (disk efficient)

### **Why Use It**

- **Workspace filtering:** Run commands on specific packages or groups of packages
- **Strict dependencies:** Only declared dependencies accessible (prevents phantom dependencies)
- **Git-aware filtering:** Run commands only on packages changed since a specific branch
- **Disk efficiency:** Content-addressable storage saves disk space across projects
- **Fast installations:** Symlinks packages from global store instead of copying
- **Monorepo support:** Built-in workspace support for managing multiple packages

### **Common Workspace Commands**

```bash
# Run tests in specific workspace
pnpm --filter core test
pnpm --filter api test:e2e

# Run development server
pnpm --filter web dev
pnpm --filter api dev

# Build specific workspace
pnpm --filter web build
pnpm --filter core build

# Add dependency to specific workspace
pnpm --filter core add lodash
pnpm --filter web add react-query

# Add workspace dependency (link internal packages)
pnpm --filter web add @repo/core@workspace:*

# Add dev dependency to root (affects all workspaces)
pnpm add -Dw typescript eslint prettier
```

### **Workspace Filtering Patterns**

```bash
# Run in specific workspace
pnpm --filter core <script>
pnpm --filter web <script>

# Run in multiple workspaces
pnpm --filter core --filter web build

# Run in workspace and its dependencies
pnpm --filter core... build

# Run in workspace and its dependents
pnpm --filter ...web build

# Run in changed workspaces since main
pnpm --filter "...[origin/main]" test
pnpm --filter "...[origin/main]" build

# Exclude specific workspace
pnpm --filter "!core" build

# Run in all workspaces (recursive)
pnpm -r build
pnpm -r test

# Run in parallel across workspaces
pnpm -r --parallel build
```

### **Monorepo Cleanup**

```bash
# Clean all node_modules and reinstall
pnpm -r exec rm -rf node_modules
pnpm install

# Rebuild all packages
pnpm -r rebuild

# Prune unused packages
pnpm prune

# Check for duplicate dependencies across workspaces
pnpm list --depth Infinity | grep -E "^\S" | sort | uniq -d

# Update all dependencies to latest
pnpm update --latest --recursive

# Check outdated packages across workspaces
pnpm outdated --recursive
```

### **Troubleshooting**

```bash
# Clear pnpm cache and store
pnpm store prune

# Fix lockfile issues
rm pnpm-lock.yaml && pnpm install

# Debug why package is installed
pnpm why <package>

# Fix peer dependency conflicts
pnpm install --force

# Verify store integrity
pnpm store status
```
