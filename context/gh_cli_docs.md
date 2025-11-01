# GitHub CLI (gh) - Complete Guide

Comprehensive documentation for GitHub CLI (`gh`) focusing on PR and branch management workflows.

## Table of Contents
- [Installation](#installation)
- [Authentication](#authentication)
- [Core Concepts](#core-concepts)
- [Branch Management](#branch-management)
- [Pull Request Workflow](#pull-request-workflow)
- [PR Review & Collaboration](#pr-review--collaboration)
- [Advanced Patterns](#advanced-patterns)
- [Common Workflows](#common-workflows)
- [Best Practices](#best-practices)

## Installation

### macOS
```bash
brew install gh
brew upgrade gh
```

### Verify Installation
```bash
gh --version
```

## Authentication

### Initial Setup
```bash
gh auth login
```

Interactive prompts will ask:
1. **GitHub host**: Choose `GitHub.com` (or your enterprise instance)
2. **Protocol**: Choose `HTTPS` or `SSH`
3. **Authenticate Git**: Choose `Yes` to use gh for git operations
4. **Authentication method**: Choose `Login with a web browser` or `Paste an authentication token`

### Check Auth Status
```bash
gh auth status
```

### Switch Between Accounts
```bash
gh auth switch
```

### Refresh Authentication
```bash
gh auth refresh -s project
```

### Use Token (for automation)
```bash
export GITHUB_TOKEN=your_token_here
export GH_TOKEN=your_token_here
```

## Core Concepts

### Repository Context
Most `gh` commands auto-detect the current repository from your git remote. Override with:
```bash
gh pr list -R owner/repo
```

### Output Formats
```bash
gh pr list --json number,title,state
gh pr list --json number,title --jq '.[] | select(.state=="OPEN")'
```

## Branch Management

### Create and Switch Branch
```bash
git checkout -b feature/my-feature
```

### Push Branch to Remote
```bash
git push -u origin feature/my-feature
git push -u origin HEAD
```

### View Current Branch
```bash
git branch --show-current
```

### Common Branch Patterns
```bash
feature/feature-name
fix/bug-description
chore/task-description
docs/documentation-update
test/test-description
```

## Pull Request Workflow

### 1. Create PR - Interactive
```bash
gh pr create
```

This will prompt for:
- Title
- Body
- Reviewers
- Assignees
- Labels
- Projects
- Milestones

### 2. Create PR - With Flags
```bash
gh pr create \
  --title "feat: add user authentication" \
  --body "Implements JWT-based authentication system" \
  --base main \
  --head feature/auth \
  --assignee @me \
  --reviewer username1,username2 \
  --label bug,enhancement \
  --draft
```

### 3. Create PR - Auto-fill from Commits
```bash
gh pr create --fill
```

Uses first commit message as title and all commits as body.

### 4. Create PR - With Body File
```bash
gh pr create --title "Feature Title" --body-file pr-template.md
```

### 5. Create PR - Using HEREDOC (Recommended)
```bash
gh pr create --title "feat: implement user profile" --body "$(cat <<'EOF'
## Summary
- Implemented user profile page
- Added profile editing functionality
- Integrated avatar upload

## Changes
- **What**: Core user profile features
- **Breaking**: None
- **Dependencies**: Added multer for file uploads

## Testing
- [x] Unit tests passing
- [x] E2E tests passing
- [ ] Manual testing required

## Review Focus
- Security of file upload implementation
- Performance of profile queries

Fixes #123
EOF
)"
```

### 6. Create PR - Open in Browser
```bash
gh pr create --web
```

### 7. Create Draft PR
```bash
gh pr create --draft --title "WIP: new feature"
```

## PR Management Commands

### List PRs
```bash
gh pr list
gh pr list --state open
gh pr list --state closed
gh pr list --state merged
gh pr list --state all

gh pr list --author @me
gh pr list --assignee @me
gh pr list --label bug
gh pr list --base develop
```

### View PR Details
```bash
gh pr view
gh pr view 123
gh pr view --web
gh pr view --json title,body,state
```

### View PR Diff
```bash
gh pr diff
gh pr diff 123
```

### Check PR Status
```bash
gh pr status
```

Shows:
- Current branch PR
- PRs created by you
- PRs requesting your review

### Checkout PR Locally
```bash
gh pr checkout 123
gh pr checkout https://github.com/owner/repo/pull/123
```

### Edit PR
```bash
gh pr edit 123 --title "New title"
gh pr edit 123 --body "New description"
gh pr edit 123 --add-label bug
gh pr edit 123 --remove-label wip
gh pr edit 123 --add-reviewer username
gh pr edit 123 --add-assignee @me
```

### Close PR
```bash
gh pr close 123
gh pr close 123 --delete-branch
```

### Reopen PR
```bash
gh pr reopen 123
```

### Mark PR as Ready
```bash
gh pr ready 123
```

Converts draft PR to ready for review.

## PR Review & Collaboration

### Check CI Status
```bash
gh pr checks
gh pr checks 123
```

### Add Comment
```bash
gh pr comment 123 --body "Great work!"
```

### Review PR
```bash
gh pr review 123 --approve
gh pr review 123 --approve --body "LGTM!"

gh pr review 123 --comment --body "Please fix the typo"

gh pr review 123 --request-changes --body "Tests are failing"
```

### Merge PR
```bash
gh pr merge 123
gh pr merge 123 --merge
gh pr merge 123 --squash
gh pr merge 123 --rebase

gh pr merge 123 --squash --delete-branch
gh pr merge 123 --auto
```

### Update PR Branch
```bash
gh pr update-branch 123
```

Syncs PR branch with latest base branch changes.

## Advanced Patterns

### Create PR with Template
```bash
gh pr create --template .github/pull_request_template.md
```

### Batch Operations
```bash
for pr in 123 124 125; do
  gh pr review $pr --approve
done
```

### Search PRs
```bash
gh search prs --author @me --state open
gh search prs --label bug --repo owner/repo
```

### View PR Files Changed
```bash
gh pr view 123 --json files --jq '.files[].path'
```

### Auto-merge When CI Passes
```bash
gh pr merge 123 --auto --squash
```

## Common Workflows

### Complete Feature Workflow
```bash
git checkout main
git pull origin main

git checkout -b feature/user-profile

git add .
git commit -m "feat: implement user profile feature"

git push -u origin feature/user-profile

gh pr create --title "feat: implement user profile" --body "$(cat <<'EOF'
## Summary
Implemented user profile page with editing capabilities

## Testing
- [x] Unit tests passing
- [x] E2E tests passing

Fixes #42
EOF
)" --assignee @me --label enhancement

gh pr checks

gh pr merge --squash --delete-branch
```

### Quick PR Creation
```bash
git checkout -b fix/typo
echo "fix" > file.txt
git add . && git commit -m "fix: correct typo"
git push -u origin HEAD
gh pr create --fill --assignee @me
```

### Review Someone's PR
```bash
gh pr checkout 123
pnpm test
gh pr review 123 --approve --body "Tests pass locally, LGTM!"
```

### Update PR After Feedback
```bash
gh pr checkout 123
git add .
git commit -m "fix: address review comments"
git push
gh pr comment 123 --body "Updated based on feedback"
```

## Best Practices

### PR Title Conventions
```bash
feat: add new feature
fix: resolve bug
chore: update dependencies
docs: improve documentation
test: add missing tests
refactor: restructure code
perf: improve performance
```

### PR Body Template
```markdown
## Summary
Brief description of changes

## Changes
- What changed
- Why it changed
- Impact of changes

## Testing
- [ ] Unit tests
- [ ] Integration tests
- [ ] Manual testing

## Review Focus
Areas that need special attention

Fixes #issue_number
```

### Useful Aliases
```bash
gh alias set prc 'pr create --fill --assignee @me'
gh alias set prv 'pr view --web'
gh alias set prs 'pr status'
gh alias set prm 'pr merge --squash --delete-branch'

gh alias list
```

### Environment Variables
```bash
export GH_HOST=github.com
export GH_TOKEN=your_token
export GH_EDITOR=code
export GH_BROWSER=chrome
```

## Troubleshooting

### Authentication Issues
```bash
gh auth logout
gh auth login
gh auth refresh
```

### View API Calls
```bash
gh pr list --verbose
```

### Check Configuration
```bash
gh config list
gh config get editor
gh config set editor vim
```

## Additional Resources

- Official Docs: https://cli.github.com/manual/
- PR Commands: https://cli.github.com/manual/gh_pr
- Examples: https://cli.github.com/manual/examples
- Extensions: https://github.com/topics/gh-extension

