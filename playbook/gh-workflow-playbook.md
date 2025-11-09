# GitHub CLI Workflow Playbook

A step-by-step guide for the complete software development cycle using GitHub CLI (`gh`) - from feature planning to production deployment.

---

## Overview

This playbook covers the entire development workflow:

1. **Planning** - Issue creation and branch setup
2. **Development** - Code changes and commits
3. **Review** - PR creation and code review
4. **Testing** - CI checks and validation
5. **Deployment** - Merge and release
6. **Maintenance** - Hotfixes and cleanup

---

## Prerequisites

### One-Time Setup

```bash
# 1. Install GitHub CLI
brew install gh

# 2. Authenticate
gh auth login
# Choose: GitHub.com → HTTPS → Yes → Login with browser

# 3. Verify authentication
gh auth status

# 4. Configure git
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# 5. Set up useful aliases
gh alias set prc 'pr create --fill --assignee @me'
gh alias set prv 'pr view --web'
gh alias set prs 'pr status'
gh alias set prm 'pr merge --squash --delete-branch'
```

---

## Workflow 1: Feature Development (Standard)

### Step 1: Create Issue (Optional but Recommended)

```bash
# Interactive issue creation
gh issue create

# Or with flags
gh issue create \
  --title "feat: add user authentication" \
  --body "## Description
Implement JWT-based authentication system

## Acceptance Criteria
- [ ] User can login with email/password
- [ ] JWT tokens are generated
- [ ] Protected routes require authentication

## Technical Notes
- Use bcrypt for password hashing
- Store tokens in httpOnly cookies" \
  --label enhancement \
  --assignee @me

# Save issue number (e.g., #42)
```

### Step 2: Create Feature Branch

```bash
# Update main branch
git checkout main
git pull origin main

# Create feature branch (use consistent naming)
git checkout -b feature/user-authentication

# Or reference issue number
git checkout -b feature/42-user-authentication
```

**Branch Naming Conventions:**

- `feature/description` - New features
- `fix/description` - Bug fixes
- `chore/description` - Maintenance tasks
- `docs/description` - Documentation updates
- `refactor/description` - Code refactoring
- `test/description` - Test additions

### Step 3: Develop and Commit

```bash
# Make your changes
# ... edit files ...

# Stage changes interactively (review each hunk)
git add -p

# Or stage all changes
git add .

# Commit with conventional commit message
git commit -m "feat: implement JWT authentication

- Add login endpoint
- Generate JWT tokens
- Add authentication middleware
- Store tokens in httpOnly cookies

Refs #42"

# Make additional commits as needed
git commit -m "test: add authentication tests"
git commit -m "docs: update API documentation"
```

**Commit Message Format:**

```
<type>: <subject>

<body>

<footer>
```

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

### Step 4: Push Branch

```bash
# Push branch to remote
git push -u origin feature/user-authentication

# Or push current branch
git push -u origin HEAD
```

### Step 5: Create Pull Request

```bash
# Interactive PR creation
gh pr create

# Or use HEREDOC for formatted body (recommended)
gh pr create --title "feat: implement user authentication" --body "$(cat <<'EOF'
## Summary
Implements JWT-based authentication system for user login and protected routes.

## Changes
- **What**: Added authentication endpoints and middleware
- **Why**: Enable secure user authentication
- **How**: JWT tokens stored in httpOnly cookies

## Testing
- [x] Unit tests passing
- [x] Integration tests passing
- [x] Manual testing completed
- [ ] E2E tests (pending review)

## Review Focus
- Security of JWT implementation
- Cookie settings (httpOnly, secure, sameSite)
- Error handling in authentication middleware

## Checklist
- [x] Code follows project conventions
- [x] Tests added/updated
- [x] Documentation updated
- [x] No breaking changes
- [ ] Reviewed by security team

Fixes #42
EOF
)" --base main --assignee @me --label enhancement

# Or use alias
gh prc

# Save PR number (e.g., #123)
```

### Step 6: Check PR Status

```bash
# View PR status
gh pr status

# View PR details
gh pr view 123

# View PR in browser
gh pr view 123 --web

# Check CI status
gh pr checks

# Watch CI in real-time
gh run watch
```

### Step 7: Address Review Comments

```bash
# View PR comments
gh pr view 123

# Make changes based on feedback
# ... edit files ...

git add .
git commit -m "fix: address review comments

- Update cookie security settings
- Add input validation
- Improve error messages"

git push

# Add comment to PR
gh pr comment 123 --body "✅ Updated based on feedback:
- Fixed cookie security settings
- Added input validation
- Improved error messages

Ready for re-review!"
```

### Step 8: Merge PR

```bash
# Check if CI is passing
gh pr checks

# Merge with squash (recommended for feature branches)
gh pr merge 123 --squash --delete-branch

# Or use alias
gh prm 123

# Or interactive merge
gh pr merge 123
# Choose: Squash and merge → Confirm → Delete branch

# Pull latest main
git checkout main
git pull origin main
```

### Step 9: Close Issue (if not auto-closed)

```bash
# Close issue
gh issue close 42 --comment "Resolved in #123"
```

---

## Workflow 2: Bug Fix (Hotfix)

### Quick Hotfix Process

```bash
# 1. Create issue
gh issue create --title "fix: login button not working" --label bug

# 2. Create fix branch from main
git checkout main
git pull origin main
git checkout -b fix/login-button

# 3. Make fix
# ... edit files ...
git add .
git commit -m "fix: resolve login button click handler

The button was missing the onClick event handler.

Fixes #43"

# 4. Push and create PR
git push -u origin HEAD
gh pr create --title "fix: login button not working" --body "$(cat <<'EOF'
## Problem
Login button was not responding to clicks.

## Root Cause
Missing onClick event handler in Button component.

## Solution
Added onClick handler to trigger login action.

## Testing
- [x] Manual testing - button now works
- [x] Unit test added
- [x] No regression in other buttons

Fixes #43
EOF
)" --label bug --assignee @me

# 5. Merge immediately if urgent
gh pr merge --squash --delete-branch
```

---

## Workflow 3: Review Someone's PR

### Review Process

```bash
# 1. List PRs needing review
gh pr list --assignee @me

# 2. View PR details
gh pr view 124

# 3. Checkout PR locally
gh pr checkout 124

# 4. Test changes
pnpm install
pnpm test
pnpm build

# 5. Review code
gh pr diff 124

# 6. Approve or request changes
# Approve
gh pr review 124 --approve --body "LGTM! Great work on the authentication implementation.

Tested locally and all tests pass. Code is clean and well-documented."

# Request changes
gh pr review 124 --request-changes --body "Please address the following:

1. Add input validation for email format
2. Update error messages to be more user-friendly
3. Add test for invalid credentials

Otherwise looks good!"

# Add comment without approval
gh pr review 124 --comment --body "Nice implementation! Just a few suggestions:

- Consider using a constant for token expiry time
- Add JSDoc comments to the middleware function"

# 7. Return to your branch
git checkout main
```

---

## Workflow 4: Draft PR for Early Feedback

### Draft PR Process

```bash
# 1. Create branch and make initial changes
git checkout -b feature/new-dashboard
# ... make changes ...
git add .
git commit -m "wip: initial dashboard layout"
git push -u origin HEAD

# 2. Create draft PR
gh pr create --draft --title "WIP: new dashboard layout" --body "$(cat <<'EOF'
## Work in Progress

Early draft for feedback on the new dashboard layout.

## Current Status
- [x] Basic layout structure
- [ ] Add charts and graphs
- [ ] Implement data fetching
- [ ] Add responsive design
- [ ] Write tests

## Questions for Reviewers
1. Does the layout match the design mockups?
2. Should we use Chart.js or D3 for visualizations?
3. Any concerns about the component structure?

**Note:** This is not ready for merge, just seeking early feedback.
EOF
)" --assignee @me

# 3. Continue development
# ... make more changes ...
git add .
git commit -m "feat: add chart components"
git push

# 4. Mark as ready when done
gh pr ready 123

# 5. Request reviews
gh pr edit 123 --add-reviewer teammate1,teammate2
```

---

## Workflow 5: Update PR Branch with Main

### Sync PR with Latest Main

```bash
# Method 1: Using gh CLI
gh pr checkout 125
gh pr update-branch 125

# Method 2: Manual rebase
git checkout feature/my-feature
git fetch origin
git rebase origin/main

# Resolve conflicts if any
# ... fix conflicts ...
git add .
git rebase --continue

# Force push (required after rebase)
git push --force-with-lease

# Method 3: Merge main into feature
git checkout feature/my-feature
git merge origin/main
git push
```

---

## Workflow 6: Multiple Related PRs (Stacked PRs)

### Stacked PR Strategy

```bash
# 1. Create base feature
git checkout main
git checkout -b feature/auth-base
# ... implement base auth ...
git add . && git commit -m "feat: add auth base"
git push -u origin HEAD
gh pr create --title "feat: auth base" --base main

# 2. Create dependent feature
git checkout -b feature/auth-ui
# ... implement UI on top of base ...
git add . && git commit -m "feat: add auth UI"
git push -u origin HEAD
gh pr create --title "feat: auth UI" --base feature/auth-base

# 3. Merge base first
gh pr merge <base-pr-number> --squash

# 4. Update dependent PR to target main
gh pr edit <ui-pr-number> --base main

# 5. Merge dependent PR
gh pr merge <ui-pr-number> --squash --delete-branch
```

---

## Workflow 7: Emergency Rollback

### Rollback Process

```bash
# 1. Identify problematic commit
git log --oneline

# 2. Create revert branch
git checkout main
git pull origin main
git checkout -b revert/broken-feature

# 3. Revert the commit
git revert <commit-hash>

# Or revert a merge commit
git revert -m 1 <merge-commit-hash>

# 4. Push and create PR
git push -u origin HEAD
gh pr create --title "revert: rollback broken feature" --body "$(cat <<'EOF'
## Emergency Rollback

Reverting commit <hash> due to production issue.

## Issue
- Users unable to login after deployment
- Error: "Invalid token format"

## Impact
- Affects all users
- Deployed at: 2024-01-15 14:30 UTC

## Resolution
Revert to previous working version while investigating root cause.

## Follow-up
- [ ] Investigate root cause
- [ ] Fix issue in separate PR
- [ ] Add tests to prevent regression
EOF
)" --label urgent --assignee @me

# 5. Merge immediately
gh pr merge --squash --delete-branch
```

---

## Workflow 8: Release Management

### Create Release

```bash
# 1. Ensure main is up to date
git checkout main
git pull origin main

# 2. Create release tag
git tag -a v1.2.0 -m "Release v1.2.0

## Features
- User authentication
- Dashboard improvements
- Performance optimizations

## Bug Fixes
- Fixed login button
- Resolved memory leak

## Breaking Changes
- None"

git push origin v1.2.0

# 3. Create GitHub release
gh release create v1.2.0 \
  --title "Release v1.2.0" \
  --notes "$(cat <<'EOF'
## 🚀 Features
- User authentication (#123)
- Dashboard improvements (#124)
- Performance optimizations (#125)

## 🐛 Bug Fixes
- Fixed login button (#126)
- Resolved memory leak (#127)

## 📝 Documentation
- Updated API docs
- Added authentication guide

## ⚠️ Breaking Changes
None

## 📦 Assets
- Source code (zip)
- Source code (tar.gz)
EOF
)"

# 4. View release
gh release view v1.2.0 --web
```

---

## Best Practices

### 1. Commit Messages

```bash
# Good
git commit -m "feat: add user authentication

Implements JWT-based authentication with:
- Login endpoint
- Token generation
- Auth middleware

Refs #42"

# Bad
git commit -m "updates"
git commit -m "fix stuff"
```

### 2. PR Descriptions

- Always include: Summary, Changes, Testing, Review Focus
- Reference related issues with `Fixes #123` or `Refs #123`
- Use checklists for tracking progress
- Add screenshots for UI changes

### 3. Branch Management

- Keep branches short-lived (< 1 week)
- Delete branches after merge
- Sync with main regularly
- Use descriptive branch names

### 4. Code Review

- Review within 24 hours
- Test changes locally
- Provide constructive feedback
- Approve only when confident

### 5. CI/CD

- Always wait for CI to pass
- Fix failing tests immediately
- Don't merge with warnings
- Monitor deployment after merge

---

## Troubleshooting

### PR Creation Fails

```bash
# Check authentication
gh auth status

# Re-authenticate
gh auth login

# Check remote
git remote -v
```

### CI Checks Failing

```bash
# View checks
gh pr checks

# View detailed logs
gh run view <run-id>

# Re-run failed jobs
gh run rerun <run-id> --failed
```

### Merge Conflicts

```bash
# Update branch with main
gh pr update-branch <pr-number>

# Or manually
git checkout feature/branch
git fetch origin
git merge origin/main
# Resolve conflicts
git add .
git commit
git push
```

### Can't Approve Own PR

This is expected GitHub behavior. You need another team member to review and approve.

---

## Quick Reference

```bash
# Issue
gh issue create                      # Create issue
gh issue list                        # List issues
gh issue view 42                     # View issue

# Branch
git checkout -b feature/name         # Create branch
git push -u origin HEAD              # Push branch

# PR
gh pr create --fill                  # Create PR
gh pr status                         # Check status
gh pr checks                         # Check CI
gh pr view 123                       # View PR
gh pr merge 123 --squash             # Merge PR

# Review
gh pr checkout 123                   # Checkout PR
gh pr review 123 --approve           # Approve PR
gh pr comment 123 -b "message"       # Comment

# Release
gh release create v1.0.0             # Create release
gh release list                      # List releases
```

---

## Summary

This playbook provides a complete workflow for:

- ✅ Feature development with issues and PRs
- ✅ Bug fixes and hotfixes
- ✅ Code review process
- ✅ Draft PRs for early feedback
- ✅ Branch synchronization
- ✅ Stacked PRs for complex features
- ✅ Emergency rollbacks
- ✅ Release management

Follow these workflows to maintain a clean, efficient development process using GitHub CLI.
