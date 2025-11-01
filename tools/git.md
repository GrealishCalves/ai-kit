## git - Version Control & Change Management

### **What It Does**

- Manages staging area for selective commits
- Shows diffs between versions, branches, commits
- Compresses and cleans repository history

### **Why Use It**

- **Precise staging:** Stage exactly what you want using interactive hunk selection
- **Change visibility:** See what changed before committing to avoid mistakes
- **History search:** Find commits by code changes using pickaxe search
- **Cleanup power:** Remove bloat, compress history, and optimize repository size
- **Diff analysis:** Compare changes between branches, commits, and working directory
- **Selective commits:** Create focused, atomic commits by staging specific changes

### **Staging & Unstaging**

```bash
# Interactive staging (choose specific hunks)
git add -p
git add --patch

# Stage only tracked files (ignore new untracked files)
git add -u

# Unstage everything
git restore --staged .

# Unstage specific hunks interactively
git restore --staged -p <file>

# View what's staged vs unstaged
git diff --staged
git diff
git diff HEAD
```

### **Viewing Changes & Diffs**

```bash
# Show staged changes (what will be committed)
git diff --staged
git diff --cached

# Show all changes (staged + unstaged)
git diff HEAD

# Show stat summary (files changed, insertions, deletions)
git diff --stat
git diff --shortstat

# Show only file names with status (A=added, M=modified, D=deleted)
git diff --name-status
git diff --name-only

# Ignore whitespace changes
git diff -w
git diff --ignore-all-space

# Show word-level diff (not line-level)
git diff --word-diff

# Show diff excluding specific files
git diff -- . ':(exclude)pnpm-lock.yaml' ':(exclude)*.json'

# Show diff for specific file types
git diff -- "*.ts" "*.tsx" "*.sol"

# Show function/class context in diff
git diff -W
git diff --function-context
```

### **Commit Review & History**

```bash
# View commit history with graph
git log --graph --oneline --all

# View specific file history (follow renames)
git log --follow -- src/components/Button.tsx

# View commit details with diff
git show <commit-hash>
git show HEAD
git show HEAD~1

# Show only files changed in commit
git show --name-status <commit-hash>
git show --stat <commit-hash>

# Search commits by code changes (pickaxe)
git log -S "functionName" --oneline
git log -S "function functionName" -p

# Search commits by regex pattern
git log -G "function.*Handler" --oneline
git log -G "export.*Component" -p

# Search commits by message
git log --grep="feature" --oneline
git log --grep="fix.*bug" --oneline

# View commits in range
git log main..feature-branch
git log abc123..def456

# View commits that changed specific file with diff
git log -p -- src/utils/helpers.ts
```

### **Useful Aliases**

Add to `.gitconfig`:

```ini
[alias]
  st = status -s
  unstage = restore --staged
  last = log -1 HEAD
  lg = log --graph --oneline --all
  d = diff
  ds = diff --staged
  dh = diff HEAD
  amend = commit --amend --no-edit
  files = diff --name-status
```

### **Compression & Cleanup**

```bash
# Remove untracked files (dry run first)
git clean -n
git clean -nd

# Remove untracked files (execute)
git clean -f
git clean -fd

# Remove ignored files too
git clean -fdx

# Compress repository
git gc --aggressive --prune=now

# Remove remote-tracking branches that no longer exist
git fetch --prune
git remote prune origin

# Find large files in history
git rev-list --objects --all | \
  git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | \
  sed -n 's/^blob //p' | \
  sort --numeric-sort --key=2 | \
  tail -10

# Reduce repository size
git reflog expire --expire=now --all
git gc --prune=now --aggressive

# Check repository size
du -sh .git
git count-objects -vH
```

### **Advanced Patterns**

```bash
# Amend last commit without editing message
git add forgotten-file.ts
git commit --amend --no-edit

# Undo last commit (keep changes staged)
git reset --soft HEAD~1

# View changes before pulling
git fetch
git diff HEAD..origin/main

# Interactive rebase last 3 commits
git rebase -i HEAD~3

# Compare current branch with main
git diff main
git diff main...feature-branch

# View reflog (recover lost commits)
git reflog
git checkout -b recovered-branch <commit-hash>
```
