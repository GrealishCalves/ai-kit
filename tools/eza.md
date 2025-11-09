## eza - Modern Directory Listings

### **What It Does**

- List files with rich formatting, icons, and git metadata
- Provide tree views, file type grouping, and human-readable sizes
- Replace `ls` with faster, more visual directory inspection

### **Why Use It**

- **Readable output:** Colors, icons, and alignment make structure obvious
- **Git-aware:** Inline status (staged, modified, untracked) for review-ready listings
- **Tree & recurse:** Built-in `--tree` replaces ad-hoc `find`/`ls` loops
- **Extensible:** Toggle blocks (permissions, owners, dates) via flags or config
- **Smart defaults:** Sorted, human-sized, respects `.gitignore` when requested

### **Essential Commands**

```bash
# Default list (replaces ls)
eza

# Long view with git status and human sizes
eza -l --git --group

# Show hidden files
eza -a

# Tree view (depth 2 by default)
eza --tree
eza --tree --level=3 apps/frontend

# Sort by modification time
eza -lt

# Columns for quick scanning
eza --grid src

# Group directories first
eza --group-directories-first
```

### **Contextual Views**

```bash
# Inspect permissions, owners, and timestamps
eza -l --time-style=relative --extended

# Display file types and total size
eza --long --classify --total

# Follow symlinks (useful for monorepos)
eza -lH packages

# Show inode numbers or ACL info
eza -li --acls

# Compare prod vs staging configs
eza -lR env/prod env/staging
```

### **Integration with Other Tools**

```bash
# Pair with ripgrep results
rg -l 'TODO' src | eza --stdin --long

# View git status grouped by directory
git status --short | cut -c4- | sort -u | eza --stdin --long --git

# Inspect large folders before cleanup
eza -l --sort=size node_modules | head

# Combine with fd for targeted listings
fd -t d 'migrations' apps | eza --stdin --long --tree

# Display JSON exports alongside metadata
fd -e json exports | eza --stdin --long --icons --bytes
```

### **Advanced Flags & Tips**

- `--icons=always`: show Nerd Font icons (set terminal font accordingly)
- `--git-repos`: summarize status of nested git repos (great for monorepos)
- `--header`: add column headers in long mode
- `--bytes`: display raw byte sizes instead of human readable
- `--time-style=iso,dot`: align timestamps for diffing directories
- `--sort=size/type/extension`: pick sorting strategy
- `--ignore-glob`: filter out patterns without editing `.gitignore`
- `--color-scale`: colorize size/time columns to spot outliers quickly

```bash
# Persist defaults (e.g., add to ~/.config/eza/config.yml)
eza --save-config

# Show git repo summary at root
eza --git-repos --level=2 .

# Highlight stale artifacts
eza --long --sort=oldest --reverse tmp
```

### **Practical Workflows**

```bash
# Code review prep: show component updates
eza -l --git --group apps/frontend/src/components

# Generate tree snapshot for LLM prompts
eza --tree --level=2 apps/backend > temp/backend-structure.txt

# Audit build artifacts before deployment
eza -l dist | bat --style=plain

# Check disk footprint of services
eza --long --sort=size services | tail

# Verify new packages added by pnpm install
eza -l node_modules/.pnpm | head
```

### **Troubleshooting**

- Seeing blank icons? Switch terminal font to Nerd Font or disable icons (`--icons=never`)
- Tree output noisy? Reduce depth with `--level` or filter via `--ignore-glob`
- Git status missing? Make sure you run inside repo root or add `--git-repos`
- Colors missing over SSH? Force with `--color=always` or ensure `$TERM` supports color
- Prefer `ls` paths? Use alias `alias ls='eza'` in shell config, with fallback `\ls`
