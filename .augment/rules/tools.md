---
type: "always_apply"---

# Tool Usage Rules - STRICT ENFORCEMENT

## Prohibited Commands (NEVER USE)
- **NEVER** use `grep` - it is forbidden
- **NEVER** use `cat` - it is forbidden  
- **NEVER** use `find` - it is forbidden
- **NEVER** use `jq` - it is forbidden
- **NEVER** use `ls` - prefer eza instead

## Required Tool Replacements (ALWAYS USE)

### Text Search
- **ALWAYS** use `rg` (ripgrep) instead of grep
- **ALWAYS** use `rg --files` instead of find for file listing
- Example: `rg "pattern" --type ts` NOT `grep -r "pattern" *.ts`

### File Viewing  
- **ALWAYS** use `bat` instead of cat for file viewing
- **ALWAYS** use `bat --style=numbers` for numbered output (replaces `cat -n`)
- **ALWAYS** use `bat --paging=never` for non-interactive output
- Example: `bat file.ts` NOT `cat file.ts`

### JSON Processing
- **ALWAYS** use `jaq` instead of jq (faster, same syntax)
- Example: `jaq '.field' file.json` NOT `jq '.field' file.json`

### File Discovery
- **ALWAYS** use `fd` for file discovery when available
- **FALLBACK** to `rg --files` if fd is unavailable
- Example: `fd "*.ts"` NOT `find . -name "*.ts"`

### Directory Listings
- **ALWAYS** prefer `eza` over ls for directory listings
- Use `eza --tree` for tree structure
- Use `eza -l --git` for detailed listings with git status

### Code Analysis
- **ALWAYS** default to Knip for dead-code detection
- **ONLY** use ts-prune/unimported as last-resort legacy tools when Knip fails

## Enforcement Rules

1. **Before executing any command**, verify it uses the correct tool
2. **If you catch yourself about to use a prohibited command**, STOP and use the required replacement
3. **When viewing files**, ALWAYS use `bat` with appropriate flags
4. **When searching text**, ALWAYS use `rg` with appropriate flags
5. **Reference tool documentation** in `/tools/` directory before use

## Command Translation Examples

```bash
# ❌ WRONG → ✅ CORRECT

# Text search
grep -r "useState" . → rg "useState"
grep -n "TODO" file.ts → rg -n "TODO" file.ts

# File viewing
cat file.ts → bat file.ts
cat -n file.ts → bat --style=numbers file.ts

# File listing  
find . -name "*.ts" → fd "*.ts" OR rg --files --glob "*.ts"
find . -type f → rg --files

# JSON processing
jq '.name' package.json → jaq '.name' package.json

# Directory listing
ls -la → eza -la
ls -R → eza --tree