---
type: "agent_requested"
description: "Terminal / bash / zsh command"
---

# Terminal Tools - Agent Guidelines

## 🎯 Purpose

This file teaches you **how to think** about terminal tools, not what commands to run. Learn the patterns, understand the syntax, then apply them intelligently to your specific task.

---

## 🔴 CRITICAL: Enforcement Protocol

**Before executing ANY terminal command:**

1. **Identify the task type** - Am I searching text? Finding files? Viewing content? Processing JSON?
2. **Check the PROHIBITED list** - Does my command use grep/find/cat/ls/jq?
3. **Choose the right modern tool** - Use the decision tree below
4. **Verify tool availability** - Check if fd exists before falling back to `rg --files`
5. **Apply the pattern** - Use syntax examples as templates, not copy-paste commands

**Self-Correction Protocol:**
If you catch yourself suggesting a prohibited command:

- Immediately stop and state: "ERROR: I suggested a prohibited command"
- Identify the task type (search text? find files? view content?)
- Choose the correct modern tool from the decision tree
- Explain why the alternative is better for THIS specific task

**Failure Consequences:**

- Using prohibited commands will result in slower execution, ignored files, or incorrect output
- The user will need to manually correct your mistakes
- You will lose trust and credibility

---

## 🚫 PROHIBITED Commands (NEVER Use)

<prohibited_commands enforcement="strict">

| NEVER Use | ALWAYS Use | Why                                                | Priority     |
| --------- | ---------- | -------------------------------------------------- | ------------ |
| `grep`    | `rg`       | 20x faster, respects .gitignore, better regex      | 🔴 CRITICAL  |
| `find`    | `fd`       | Simpler syntax, 20x faster, intuitive              | 🔴 CRITICAL  |
| `cat`     | `bat`      | Syntax highlighting, line numbers, git integration | 🔴 CRITICAL  |
| `ls`      | `eza`      | Better formatting, git status, tree view           | 🟡 PREFERRED |
| `jq`      | `jaq`      | Same syntax, 2x faster, smaller binary             | 🟡 PREFERRED |

</prohibited_commands>

**Constraint:** When you need to search text, your ONLY option is `rg`. When you need to find files, your ONLY option is `fd` (or `rg --files` if fd unavailable).

---

## 📋 Quick Decision Tree

```
What do I need to do?

├─ Search TEXT inside files?
│  └─ Use: rg
│     └─ Fallback: NONE (rg is always available)
│
├─ Find FILES by name/path?
│  ├─ Simple pattern (one file/extension)?
│  │  └─ Use: fd
│  │     └─ Fallback: rg --files (only if fd unavailable)
│  │     └─ Verify first: command -v fd >/dev/null && fd . || rg --files
│  │
│  └─ Multiple patterns with OR logic (file1|file2|file3)?
│     └─ Use: rg --files | rg "pattern1|pattern2|pattern3"
│     └─ Why: fd doesn't support | alternation
│
├─ View FILE contents?
│  └─ Use: bat
│     └─ Fallback: view tool (acceptable)
│
├─ List DIRECTORY contents?
│  └─ Use: eza
│     └─ Fallback: ls (acceptable if eza unavailable)
│
└─ Process JSON data?
   └─ Use: jaq
      └─ Fallback: jq (acceptable if jaq unavailable)
```

---

## ✅ Pre-Execution Verification Checklist

Before suggesting or executing any terminal command, verify:

- [ ] I identified the task type (search text / find files / view content / etc.)
- [ ] I chose the correct modern tool from the decision tree
- [ ] I verified tool availability before using fallbacks
- [ ] No prohibited commands appear anywhere in the command chain
- [ ] If piping commands, ALL commands in the pipe use modern tools
- [ ] I'm using the syntax as a **pattern**, not copy-pasting examples

**Example of FAILED verification:**

```bash
# ❌ FAILS - uses grep (prohibited)
fd -e ts | xargs grep "pattern"

# ❌ FAILS - didn't verify fd availability
rg --files  # Should check: command -v fd first
```

**Example of PASSED verification:**

```bash
# ✅ PASSES - uses rg (required)
fd -e ts -X rg "pattern"

# ✅ PASSES - verified fd availability
command -v fd >/dev/null && fd . || rg --files
```

---

## 📋 Tool Pattern Guide

### ripgrep (rg) - Search Text INSIDE Files

**Purpose:** Fast text search that respects .gitignore and has better defaults than grep

<tool name="rg" priority="critical">
  <never>grep -r "pattern" .</never>
  <always>rg "pattern"</always>
  <use_for>Searching code patterns, function names, imports, any text content</use_for>
  <never_for>Finding files by name (use fd instead)</never_for>
</tool>

**Core Syntax Patterns:**

```bash
# Basic pattern
rg "PATTERN"                    # Search current directory

# Filter by file type
rg "PATTERN" --type TYPE        # TYPE: ts, js, py, md, etc.

# Output control
rg "PATTERN" -l                 # List files only (don't show matches)
rg "PATTERN" -c                 # Count matches per file
rg "PATTERN" -C NUM             # Show NUM lines of context

# Case handling
rg "PATTERN" -i                 # Case insensitive
rg "PATTERN" -s                 # Case sensitive (default)

# Advanced patterns
rg "PATTERN" --glob 'GLOB'      # Filter by glob pattern
rg "PATTERN" -oN                # Only show matching part
rg "PATTERN" --replace 'TEXT'   # Replace matches in output
```

**Common Use Cases (Learn the Pattern, Not the Command):**

**Pattern 1: Search for specific content**

```bash
# Template: rg "YOUR_SEARCH_TERM" [--type FILE_TYPE] [-i for case-insensitive]
# Example: Find React hooks
rg "useState|useEffect" --type ts
```

**Pattern 2: Find files containing pattern**

```bash
# Template: rg "PATTERN" -l [--type TYPE]
# Example: Find files that import a module
rg "import.*from.*react" -l --type ts
```

**Pattern 3: Extract specific parts with regex**

```bash
# Template: rg 'REGEX_WITH_CAPTURE' -oN --replace '$1'
# Example: Extract function names
rg 'function (\w+)' -oN --replace '$1'
```

**Pattern 4: Combine with other tools**

```bash
# Template: rg "PATTERN" -l | xargs COMMAND
# Example: Search and view results
rg "TODO" -l | xargs bat
```

**Anti-Patterns (Learn What NOT to Do):**

```bash
# ❌ WRONG: Using grep
grep -r "pattern" .
# ✅ CORRECT: Use rg with same intent
rg "pattern"

# ❌ WRONG: Using rg for file listing
rg --files | rg -v 'test'
# ✅ CORRECT: Use fd for file operations
fd --exclude test

# ❌ WRONG: Copy-pasting specific examples
rg "TODO|FIXME|HACK" -C 2  # Don't blindly use this!
# ✅ CORRECT: Adapt pattern to YOUR task
rg "YOUR_SEARCH_TERMS" -C 2  # Think about what YOU need
```

---

### fd - Find Files BY NAME

**Purpose:** Simple and fast alternative to find with intuitive syntax and smart defaults

<tool name="fd" priority="critical">
  <never>find . -name "*.ts"</never>
  <always>fd -e ts</always>
  <use_for>Finding files by name, extension, or type</use_for>
  <never_for>Searching text inside files (use rg instead)</never_for>
</tool>

**⚠️ CRITICAL: fd Pattern Syntax**

fd uses **glob patterns** or **simple regex**, NOT regex alternation with `|`:

```bash
# ❌ WRONG: fd doesn't support | alternation like rg
fd "file1|file2|file3" .        # This treats | literally, not as OR

# ✅ CORRECT: Use rg for alternation
rg --files | rg "file1|file2|file3"

# ✅ CORRECT: Use fd with glob
fd --glob '*file*' .

# ✅ CORRECT: Use fd multiple times
fd file1; fd file2; fd file3
```

**Rule:** If you need alternation (OR logic) for filenames, use `rg --files | rg "pattern1|pattern2"`. fd is for simple patterns.

---

**Core Syntax Patterns:**

```bash
# Basic search
fd PATTERN                      # Find files/dirs matching pattern (simple regex)

# Filter by type
fd -e EXTENSION                 # Find by extension (no dot needed)
fd -t TYPE PATTERN              # TYPE: f (file), d (dir), l (symlink)

# Include/exclude
fd -H PATTERN                   # Include hidden files
fd -I PATTERN                   # Include ignored files (.gitignore)
fd --exclude PATTERN            # Exclude pattern

# Glob patterns
fd --glob 'GLOB_PATTERN'        # Use glob instead of regex (*, ?, [])

# Execute commands
fd PATTERN -x COMMAND           # Execute command on each result
fd PATTERN -X COMMAND           # Execute command on all results (batched)
```

**Common Use Cases (Learn the Pattern, Not the Command):**

**Pattern 1: Find files by extension**

```bash
# Template: fd -e EXTENSION [SEARCH_PATH]
# Example: Find all TypeScript files
fd -e ts
```

**Pattern 2: Find files by name pattern**

```bash
# Template: fd "NAME_PATTERN" [PATH]
# Example: Find config files
fd "config"
```

**Pattern 3: Find and process files**

```bash
# Template: fd PATTERN | xargs COMMAND
# Example: Find and view files
fd "config" | xargs bat
```

**Pattern 4: Execute command on each file**

```bash
# Template: fd PATTERN -x COMMAND {}
# Example: Run command on each file
fd -e json -x jaq '.name' {}
```

**Fallback Protocol:**

```bash
# ✅ CORRECT: Verify fd availability before fallback
command -v fd >/dev/null && fd . || rg --files

# ❌ WRONG: Assume fd is unavailable
rg --files  # Don't do this without checking!
```

**Anti-Patterns (Learn What NOT to Do):**

```bash
# ❌ WRONG: Using find
find . -name "*.ts"
# ✅ CORRECT: Use fd with same intent
fd -e ts

# ❌ WRONG: Using fd for content search
fd -e ts | xargs grep "pattern"
# ✅ CORRECT: Use rg for text search
rg "pattern" --type ts

# ❌ WRONG: Using fd with regex alternation (| doesn't work as OR)
fd "context7|tavily|git" tools/
# ✅ CORRECT: Use rg --files with alternation
rg --files tools/ | rg "context7|tavily|git"

# ❌ WRONG: Using rg --files without verification
rg --files
# ✅ CORRECT: Verify fd availability first
command -v fd >/dev/null && fd . || rg --files
```

---

### bat - View Files WITH Syntax Highlighting

**Purpose:** Cat clone with syntax highlighting, line numbers, and git integration

<tool name="bat" priority="critical">
  <never>cat file.ts</never>
  <always>bat file.ts</always>
  <use_for>Reading code files, viewing with syntax highlighting</use_for>
  <never_for>Searching content (use rg) or listing files (use fd)</never_for>
</tool>

**Core Syntax Patterns:**

```bash
# Basic viewing
bat FILE                        # View with syntax highlighting

# Style control
bat --style=STYLE FILE          # STYLE: full, numbers, plain, header, grid
bat -n FILE                     # Shorthand for --style=numbers

# Line range
bat -r START:END FILE           # View specific line range
bat --line-range :50 FILE       # First 50 lines
bat --line-range 100: FILE      # From line 100 to end

# Paging control
bat --paging=never FILE         # No pager (for scripts/pipes)
bat --paging=always FILE        # Always use pager

# Language override
bat -l LANGUAGE FILE            # Force specific syntax highlighting
```

**Common Use Cases (Learn the Pattern, Not the Command):**

**Pattern 1: View file with syntax highlighting**

```bash
# Template: bat FILE
# Example: View TypeScript file
bat src/index.ts
```

**Pattern 2: View specific line range**

```bash
# Template: bat -r START:END FILE
# Example: View lines 50-100
bat -r 50:100 src/index.ts
```

**Pattern 3: Combine with search results**

```bash
# Template: rg "PATTERN" -l | xargs bat
# Example: Search and view matching files
rg "useState" -l | xargs bat
```

**Anti-Patterns (Learn What NOT to Do):**

```bash
# ❌ WRONG: Using cat
cat file.ts
# ✅ CORRECT: Use bat for better readability
bat file.ts

# ❌ WRONG: Using bat for searching
bat file.ts | grep "pattern"
# ✅ CORRECT: Use rg for text search
rg "pattern" file.ts

# ❌ WRONG: Using bat in pipes without --paging=never
bat file.ts | head -10  # May hang waiting for pager
# ✅ CORRECT: Disable paging for pipes
bat --paging=never file.ts | head -10
```

---

### eza - List Directory Contents

**Purpose:** Modern ls replacement with git integration, colors, and tree view

<tool name="eza" priority="preferred">
  <never>ls -la</never>
  <always>eza -la</always>
  <use_for>Inspecting directory structure, checking git status</use_for>
  <never_for>Finding specific files (use fd) or searching content (use rg)</never_for>
</tool>

**Core Syntax Patterns:**

```bash
# Basic listing
eza                             # Simple list
eza -l                          # Long format (detailed)
eza -la                         # Long format + hidden files

# Git integration
eza --git                       # Show git status
eza -l --git                    # Long format + git status
eza --git-ignore                # Respect .gitignore

# Tree view
eza --tree                      # Tree view
eza --tree --level=NUM          # Limit tree depth
eza -T                          # Shorthand for --tree

# Sorting
eza -ls SIZE                    # Sort by size
eza -lt                         # Sort by time (modified)
eza -lS                         # Sort by size (capital S)

# Filtering
eza --only-dirs                 # Show only directories
eza --only-files                # Show only files
```

**Common Use Cases (Learn the Pattern, Not the Command):**

**Pattern 1: Inspect directory structure**

```bash
# Template: eza -la [--git] [PATH]
# Example: View current directory with git status
eza -la --git
```

**Pattern 2: Tree view of project**

```bash
# Template: eza --tree --level=NUM [PATH]
# Example: View 2-level tree
eza --tree --level=2 src/
```

**Pattern 3: Sort by specific criteria**

```bash
# Template: eza -l[SORT_FLAG]
# Example: Sort by modification time
eza -lt
```

**Anti-Patterns (Learn What NOT to Do):**

```bash
# ❌ WRONG: Using ls
ls -la
# ✅ CORRECT: Use eza (preferred)
eza -la

# ❌ WRONG: Using eza for file finding
eza -R | grep "config"
# ✅ CORRECT: Use fd for file discovery
fd "config"

# ❌ WRONG: Using eza for content search
eza --tree | grep "TODO"
# ✅ CORRECT: Use rg for text search
rg "TODO"
```

---

### jaq - Parse/Query JSON

**Purpose:** Faster jq alternative with identical syntax for parsing and querying JSON

<tool name="jaq" priority="preferred">
  <never>jq '.field' file.json</never>
  <always>jaq '.field' file.json</always>
  <use_for>Parsing JSON, extracting fields, filtering data</use_for>
  <never_for>Searching files (use rg) or finding files (use fd)</never_for>
</tool>

**Core Syntax Patterns:**

```bash
# Basic operations
jaq '.' FILE                    # Pretty print JSON
jaq -r 'QUERY' FILE             # Raw output (no quotes)
jaq -c 'QUERY' FILE             # Compact output (one line)

# Field access
jaq '.field' FILE               # Access field
jaq '.field.nested' FILE        # Nested field access
jaq '.array[0]' FILE            # Array index

# Array operations
jaq '.[]' FILE                  # Iterate array
jaq '.[] | select(CONDITION)'   # Filter array
jaq 'map(EXPRESSION)' FILE      # Transform array

# Object operations
jaq 'keys' FILE                 # Get object keys
jaq 'values' FILE               # Get object values
jaq 'to_entries' FILE           # Convert to key-value pairs

# Combining operations
jaq '.field | keys' FILE        # Chain operations
```

**Common Use Cases (Learn the Pattern, Not the Command):**

**Pattern 1: Extract specific field**

```bash
# Template: jaq -r '.FIELD_PATH' FILE
# Example: Get package version
jaq -r '.version' package.json
```

**Pattern 2: Filter array by condition**

```bash
# Template: jaq '.[] | select(.FIELD == "VALUE")' FILE
# Example: Find active items
jaq '.[] | select(.active == true)' data.json
```

**Pattern 3: Process multiple JSON files**

```bash
# Template: fd -e json | xargs jaq 'QUERY'
# Example: Extract names from all JSON files
fd -e json | xargs jaq -r '.name'
```

**Anti-Patterns (Learn What NOT to Do):**

```bash
# ❌ WRONG: Using jq
jq '.field' file.json
# ✅ CORRECT: Use jaq (preferred)
jaq '.field' file.json

# ❌ WRONG: Using cat before jaq
cat file.json | jaq '.'
# ✅ CORRECT: Direct file input
jaq '.' file.json

# ❌ WRONG: Using jaq for file search
jaq '.' *.json | grep "pattern"
# ✅ CORRECT: Use rg for text search
rg "pattern" --type json
```

---

## 🔄 Learning & Self-Correction Loop

**If a command fails:**

1. **Identify the task** - What am I trying to do? (search text / find files / view content / etc.)
2. **Check the decision tree** - Did I use the right tool for this task?
3. **Verify syntax** - Does my syntax match the patterns in this file?
4. **Check for prohibited tools** - Did I accidentally use grep/find/cat/ls/jq?
5. **Verify tool availability** - Did I check if fd exists before using rg --files?

**If output is unexpected:**

1. **Re-read the task** - Am I solving the right problem?
2. **Check tool choice** - Did I use rg for text search and fd for file discovery?
3. **Verify flags** - Are my flags correct for this tool?
4. **Check pipes** - Are all tools in the pipe chain modern tools?

**Remember:**

- These are **patterns and guidelines**, not commands to copy-paste
- **Think about your specific task** and adapt the patterns
- **Verify tool availability** before using fallbacks
- **Learn the syntax**, don't memorize specific examples

---

## 📊 Final Quick Reference

```
What do I need to do?

Search TEXT inside files?
  → Tool: rg
  → Pattern: rg "YOUR_PATTERN" [--type TYPE] [-i] [-l] [-C NUM]
  → Never: grep

Find FILES by name/path?
  → Simple pattern (one file/extension)?
    → Tool: fd (verify first: command -v fd)
    → Pattern: fd "YOUR_PATTERN" [-e EXT] [-t TYPE]
    → Fallback: rg --files (only if fd unavailable)
    → Never: find

  → Multiple patterns with OR logic (file1|file2|file3)?
    → Tool: rg --files | rg "pattern1|pattern2|pattern3"
    → Why: fd doesn't support | alternation
    → Never: fd "pattern1|pattern2|pattern3" (won't work as expected)

View FILE contents?
  → Tool: bat
  → Pattern: bat FILE [-r START:END] [--paging=never]
  → Fallback: view tool (acceptable)
  → Never: cat

List DIRECTORY contents?
  → Tool: eza
  → Pattern: eza [-la] [--git] [--tree] [--level=NUM]
  → Fallback: ls (acceptable if eza unavailable)
  → Never: ls (prefer eza)

Process JSON data?
  → Tool: jaq
  → Pattern: jaq 'QUERY' FILE [-r] [-c]
  → Fallback: jq (acceptable if jaq unavailable)
  → Never: jq (prefer jaq)
```

---

## 🤔 fd vs rg: When to Use Which?

| Task                                           | Tool                   | Example                           | Why                               |
| ---------------------------------------------- | ---------------------- | --------------------------------- | --------------------------------- |
| Find files by **name pattern**                 | `fd`                   | `fd "config"`                     | Faster, simpler syntax            |
| Find files by **extension**                    | `fd`                   | `fd -e ts`                        | Built-in extension filtering      |
| Find files by **type** (file/dir)              | `fd`                   | `fd -t d`                         | Built-in type filtering           |
| Find files by **content**                      | `rg`                   | `rg "pattern" --type ts`          | Searches inside files             |
| Find files matching **multiple patterns** (OR) | `rg --files \| rg`     | `rg --files \| rg "file1\|file2"` | fd doesn't support \| alternation |
| Find files and **execute command**             | `fd -x` or `fd -X`     | `fd -e ts -X bat`                 | Built-in execution                |
| List **all files** (no pattern)                | `fd .` or `rg --files` | `fd .`                            | Either works, fd preferred        |
