# Preparation-First Playbook

## Overview

Deep domain understanding prevents rework. Evidence-based decisions build confidence. Terminal tools enable fast, accurate investigation.

## When to Use

- Before implementing any new feature
- Before migrating existing systems
- Before making architectural changes
- Before refactoring complex code

## Prerequisites

1. **Master Official Documentation**
   - Read official docs cover-to-cover
   - Understand API contracts and CLI commands
   - Note version-specific behaviors
   - Identify best practices and patterns

2. **Audit Current Implementation**
   - Use ripgrep to find all related code
   - Use fd to discover all related files
   - Use bat to view file contents
   - Document findings with line numbers and file paths

3. **Understand Security Requirements**
   - Identify 6 security control categories:
     - Credential protection
     - Logging/error handling
     - Destructive action protection
     - Authentication/authorization
     - Path/file security
     - Attack surface reduction

4. **Review Environment & Configuration Standards**
   - Identify centralized config files
   - Understand environment-driven configuration
   - Review .env file structure
   - Note single source of truth patterns

5. **Master Terminal Tools**
   - ripgrep (rg) for text search
   - fd for file discovery
   - bat for file viewing
   - eza for directory listing
   - jaq for JSON parsing

6. **Familiarize with Project Structure**
   - Understand monorepo layout
   - Review package.json scripts
   - Identify shared utilities
   - Note naming conventions

## Workflow

### Phase 1: Investigation (Evidence-Based)

```bash
# Find all related code
rg "PATTERN" --type ts -C 3

# Discover all related files
fd "PATTERN" --type f

# View file contents
bat path/to/file.ts

# List directory structure
eza --tree --level=2 path/to/dir

# Parse JSON configuration
jaq '.field' config.json
```

### Phase 2: Documentation

Document findings with:
- File paths (exact locations)
- Line numbers (specific references)
- Patterns found (exact matches)
- Function signatures (complete definitions)
- Dependencies (all imports/exports)

### Phase 3: Planning

Create implementation plan with:
- Clear objectives
- Success criteria
- Security controls
- Testing strategy
- Rollback plan

### Phase 4: Verification

Before proceeding:
- ✅ All official docs mastered
- ✅ Current implementation audited
- ✅ Security requirements defined
- ✅ Configuration standards reviewed
- ✅ Terminal tools mastered
- ✅ Project structure understood

## Anti-Patterns

❌ **Don't:**
- Rush to implementation without investigation
- Assume you understand the system
- Skip official documentation
- Rely on assumptions instead of evidence
- Use grep/find/cat instead of rg/fd/bat

✅ **Do:**
- Invest time in preparation phase
- Use terminal tools for evidence-based investigation
- Document findings with line numbers and file paths
- Master official documentation as single source of truth
- Verify understanding before proceeding

## Success Metrics

- Zero major architectural pivots during implementation
- All security gaps identified upfront
- Clear migration path defined
- Evidence-based decisions (line numbers, file paths, patterns)
- Confidence level: 90%+ before implementation

## Example: Goldsky CLI Migration

### Preparation Phase (6 Steps)

1. **Mastered Goldsky Docs**
   - Read official CLI documentation
   - Understood `goldsky subgraph deploy` command
   - Noted `--filter`, `--summary`, `--json` flags
   - Identified version management patterns

2. **Audited Current Wizard**
   - Found 1050-line custom wizard script
   - Identified fragile regex-based CLI parsing
   - Noted ANSI escape code stripping logic
   - Documented hardcoded configurations

3. **Understood Security Requirements**
   - Defined 6 security control categories
   - Identified command injection vulnerability
   - Noted API key exposure in logs
   - Planned timeout protection

4. **Reviewed Configuration Standards**
   - Found centralized config/networks.json
   - Understood environment-driven deployment
   - Noted .env.development / .env.production pattern
   - Identified single source of truth approach

5. **Mastered Terminal Tools**
   - Used rg to find all hardcoded configs
   - Used fd to discover all related files
   - Used bat to view file contents
   - Used eza to visualize directory structure

6. **Familiarized with Project Structure**
   - Understood monorepo layout (apps/goldsky)
   - Reviewed package.json scripts
   - Identified shared utilities pattern
   - Noted naming conventions

### Outcome

- Zero major architectural pivots
- All security gaps identified upfront
- Clear migration path: custom wizard → native CLI
- Confidence level: 95%
- Implementation time: 50% faster than without preparation

## Key Takeaways

1. **Preparation beats implementation** - Deep understanding prevents rework
2. **Evidence-based decisions** - Terminal tools provide fast, accurate investigation
3. **Documentation with evidence** - Line numbers and file paths build confidence
4. **Master official docs** - Single source of truth for best practices
5. **Security upfront** - Define controls before implementation
6. **Verify before proceeding** - Complete all prerequisites before planning

