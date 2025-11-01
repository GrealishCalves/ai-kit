# gh - GitHub CLI

> **Docs:** https://cli.github.com/manual/ | **v2.0+** | **Requires:** Git

## What It Does

GitHub CLI brings GitHub to your terminal - manage PRs, issues, repos, and workflows without leaving the command line.

**Key Features:**

- ✅ Create, view, merge PRs from terminal
- ✅ Manage issues, labels, milestones
- ✅ Review PRs with approve/comment/request-changes
- ✅ Check CI status and view workflow runs
- ✅ Clone, fork, create repositories
- ✅ Manage releases and gists
- ✅ Interactive prompts or scriptable flags
- ✅ JSON output for scripting

**Safety:** Read-only by default. Destructive operations (merge, delete) require explicit confirmation.

---

## Installation

```bash
# macOS
brew install gh
brew upgrade gh

# Verify installation
gh --version
```

---

## Authentication

### Initial Setup

```bash
# Interactive login
gh auth login

# Prompts:
# 1. GitHub.com or Enterprise
# 2. HTTPS or SSH
# 3. Authenticate Git with gh credentials? (Yes)
# 4. Login with browser or paste token
```

### Auth Management

```bash
# Check auth status
gh auth status

# Switch accounts
gh auth switch

# Refresh token
gh auth refresh

# Refresh with additional scopes
gh auth refresh -s project

# Logout
gh auth logout

# Use token (for automation)
export GITHUB_TOKEN=ghp_xxxxx
export GH_TOKEN=ghp_xxxxx
```

---

## Pull Request Workflow

### Create PR

```bash
# Interactive (prompts for title, body, reviewers, etc.)
gh pr create

# Auto-fill from commits
gh pr create --fill

# With flags
gh pr create \
  --title "feat: add user authentication" \
  --body "Implements JWT auth system" \
  --base main \
  --assignee @me \
  --reviewer username1,username2 \
  --label bug,enhancement \
  --draft

# With body file
gh pr create --title "Feature" --body-file pr-template.md

# Using HEREDOC (recommended for formatted bodies)
gh pr create --title "feat: user profile" --body "$(cat <<'EOF'
## Summary
- Implemented user profile page
- Added profile editing

## Testing
- [x] Unit tests passing
- [x] E2E tests passing

Fixes #123
EOF
)" --base main --assignee @me

# Open in browser
gh pr create --web

# Create draft PR
gh pr create --draft
```

### List & View PRs

```bash
# List PRs
gh pr list
gh pr list --state open
gh pr list --state closed
gh pr list --state merged
gh pr list --state all

# Filter by author/assignee/label
gh pr list --author @me
gh pr list --assignee @me
gh pr list --label bug
gh pr list --base develop

# View PR details
gh pr view
gh pr view 123
gh pr view --web

# View PR diff
gh pr diff
gh pr diff 123

# Check PR status
gh pr status
```

### Manage PRs

```bash
# Checkout PR locally
gh pr checkout 123
gh pr checkout https://github.com/owner/repo/pull/123

# Edit PR
gh pr edit 123 --title "New title"
gh pr edit 123 --body "New description"
gh pr edit 123 --add-label bug
gh pr edit 123 --remove-label wip
gh pr edit 123 --add-reviewer username
gh pr edit 123 --add-assignee @me

# Add comment
gh pr comment 123 --body "Great work!"

# Close PR
gh pr close 123
gh pr close 123 --delete-branch

# Reopen PR
gh pr reopen 123

# Mark draft as ready
gh pr ready 123
```

### Review PRs

```bash
# Check CI status
gh pr checks
gh pr checks 123

# Review PR
gh pr review 123 --approve
gh pr review 123 --approve --body "LGTM!"
gh pr review 123 --comment --body "Please fix the typo"
gh pr review 123 --request-changes --body "Tests are failing"

# Note: Cannot approve your own PRs
```

### Merge PRs

```bash
# Interactive merge
gh pr merge 123

# Merge strategies
gh pr merge 123 --merge      # Create merge commit
gh pr merge 123 --squash     # Squash and merge
gh pr merge 123 --rebase     # Rebase and merge

# Merge and delete branch
gh pr merge 123 --squash --delete-branch

# Auto-merge when CI passes
gh pr merge 123 --auto --squash

# Update PR branch with base
gh pr update-branch 123
```

---

## Issue Management

```bash
# Create issue
gh issue create
gh issue create --title "Bug report" --body "Description"
gh issue create --web

# List issues
gh issue list
gh issue list --state open
gh issue list --state closed
gh issue list --assignee @me
gh issue list --label bug

# View issue
gh issue view 42
gh issue view 42 --web

# Edit issue
gh issue edit 42 --title "New title"
gh issue edit 42 --add-label bug
gh issue edit 42 --add-assignee @me

# Comment on issue
gh issue comment 42 --body "Working on this"

# Close/reopen issue
gh issue close 42
gh issue reopen 42

# Pin/unpin issue
gh issue pin 42
gh issue unpin 42
```

---

## Repository Operations

```bash
# View repo
gh repo view
gh repo view owner/repo
gh repo view --web

# Clone repo
gh repo clone owner/repo
gh repo clone owner/repo target-dir

# Fork repo
gh repo fork
gh repo fork owner/repo
gh repo fork --clone

# Create repo
gh repo create
gh repo create my-repo --public
gh repo create my-repo --private
gh repo create my-repo --source=. --push

# Edit repo settings
gh repo edit --description "New description"
gh repo edit --homepage "https://example.com"
gh repo edit --enable-issues
gh repo edit --enable-wiki

# Sync fork
gh repo sync
gh repo sync owner/repo
```

---

## Workflow & CI

```bash
# List workflow runs
gh run list
gh run list --workflow=ci.yml
gh run list --branch=main

# View run details
gh run view
gh run view 123456

# Watch run (live updates)
gh run watch
gh run watch 123456

# Rerun workflow
gh run rerun 123456
gh run rerun 123456 --failed

# Cancel run
gh run cancel 123456

# Download artifacts
gh run download 123456
```

---

## Advanced Features

### JSON Output & Scripting

```bash
# JSON output
gh pr list --json number,title,state
gh pr view 123 --json title,body,state

# Use jq for processing
gh pr list --json number,title,state | jq '.[] | select(.state=="OPEN")'

# Get PR URL
gh pr view 123 --json url --jq '.url'

# List PR files
gh pr view 123 --json files --jq '.files[].path'
```

### Search

```bash
# Search PRs
gh search prs --author @me --state open
gh search prs --label bug --repo owner/repo

# Search issues
gh search issues --assignee @me
gh search issues --label enhancement

# Search code
gh search code "function authenticate"

# Search repos
gh search repos "topic:typescript"
```

### Aliases

```bash
# Create alias
gh alias set prc 'pr create --fill --assignee @me'
gh alias set prv 'pr view --web'
gh alias set prs 'pr status'
gh alias set prm 'pr merge --squash --delete-branch'

# List aliases
gh alias list

# Delete alias
gh alias delete prc

# Use alias
gh prc
```

### API Access

```bash
# Make API calls
gh api repos/owner/repo
gh api repos/owner/repo/pulls
gh api repos/owner/repo/pulls/123/comments

# POST request
gh api repos/owner/repo/issues -f title="Bug" -f body="Description"

# With pagination
gh api --paginate repos/owner/repo/issues
```

---

## Common Workflows

### Complete Feature Workflow

```bash
# 1. Create branch
git checkout main
git pull origin main
git checkout -b feature/user-profile

# 2. Make changes
git add .
git commit -m "feat: implement user profile"

# 3. Push branch
git push -u origin feature/user-profile

# 4. Create PR
gh pr create --title "feat: user profile" --body "$(cat <<'EOF'
## Summary
Implemented user profile page

## Testing
- [x] Unit tests passing
- [x] E2E tests passing

Fixes #42
EOF
)" --assignee @me

# 5. Check CI
gh pr checks

# 6. Merge when ready
gh pr merge --squash --delete-branch
```

### Review Workflow

```bash
# 1. List PRs needing review
gh pr list --assignee @me

# 2. Checkout PR
gh pr checkout 123

# 3. Test locally
pnpm test

# 4. Review
gh pr review 123 --approve --body "LGTM!"
```

### Quick PR Creation

```bash
# One-liner
git checkout -b fix/typo && \
  echo "fix" > file.txt && \
  git add . && \
  git commit -m "fix: typo" && \
  git push -u origin HEAD && \
  gh pr create --fill --assignee @me
```

---

## Configuration

```bash
# View config
gh config list

# Set editor
gh config set editor vim
gh config set editor code

# Set browser
gh config set browser chrome

# Set git protocol
gh config set git_protocol https
gh config set git_protocol ssh

# Set pager
gh config set pager bat
```

---

## Quick Reference

```bash
# Auth
gh auth login                        # Login
gh auth status                       # Check status

# PR
gh pr create                         # Create PR (interactive)
gh pr create --fill                  # Auto-fill from commits
gh pr list                           # List PRs
gh pr view 123                       # View PR
gh pr checkout 123                   # Checkout PR
gh pr checks                         # Check CI
gh pr merge 123 --squash             # Merge PR

# Issue
gh issue create                      # Create issue
gh issue list                        # List issues
gh issue view 42                     # View issue

# Repo
gh repo view                         # View repo
gh repo clone owner/repo             # Clone repo
gh repo fork                         # Fork repo

# Workflow
gh run list                          # List runs
gh run watch                         # Watch run

# Utility
gh alias set name 'command'          # Create alias
gh api endpoint                      # API call
```

