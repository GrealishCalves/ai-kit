---
description: Clean up and refactor code in a specific domain or the entire monorepo
argument-hint: [domain]
---

## Understanding the Domain Argument

The **$ARGUMENTS** parameter is flexible and can represent:

1. **File/Directory Path**: `apps/hardhat/test/local`, `src/components`
2. **Conceptual Domain**: `authentication`, `payment-flow`, `user-management`
3. **Git State**: `uncommitted`, `unstaged`, `modified-files`
4. **Pattern-Based**: `*-helpers.ts`, `*.test.ts`
5. **Monorepo-Wide Sweep** _(omit the argument or pass `all`)_: Run the workflow across every workspace

**Your Task**: Interpret the domain argument intelligently and adapt tool usage accordingly.

### Running the Command

- **Entire Monorepo:** `clean` (no argument) or `clean all`
- **Single App:** `clean hardhat`, `clean docs`, `clean web`, etc.
- **Focused Module:** `clean apps/web/src/routes`

Confirm the intended scope before triggering heavy tools so you do not accidentally run a full sweep.

### Domain-Specific Tool Selection Examples

**For File/Directory Path** (e.g., `apps/contracts`, `src/components`):

- Primary: knip (scoped to the workspace for dead code detection)
- Secondary: ripgrep (cross-validation of Knip findings)
- Optional: git (if checking history)

**For Conceptual Domain** (e.g., `authentication`, `payment-flow`):

- Primary: ripgrep (pattern discovery across codebase)
- Optional: git (change history)
- Skip: knip (not effective for conceptual analysis)

**For Git State** (e.g., `uncommitted`, `modified-files`):

- Primary: git (list uncommitted/modified files)
- Secondary: ripgrep (inspect changes)
- Skip: knip (not relevant to git state)

**For Pattern-Based** (e.g., `*.test.ts`, `*-helpers.ts`):

- Primary: ripgrep (find pattern matches)
- Secondary: knip (verify unused exports in matched files)

**Remember**: These are suggestions, not rules. Use your judgment based on the specific cleanup goals.

## MANDATORY PRE-ANALYSIS CHECKLIST

REQUIRED: Execute these view commands simultaneously to understand available capabilities:

**Persona & Analysis Tools:**

- read("ai-kit/personas/cleaner_persona.md", type="file"): Risk assessment framework and decision-making principles
- read("ai-kit/tools/knip.md", type="file"): Dead code detection for monorepos (PRIMARY TOOL)

**Terminal Tools:**

- read("ai-kit/tools/ripgrep.md", type="file"): Fast text search, pattern detection, cross-validation (REQUIRED)
- read("ai-kit/tools/jaq.md", type="file"): JSON querying and transformation (REQUIRED for processing tool outputs)
- read("ai-kit/tools/fd.md", type="file"): Fast file discovery (REQUIRED for file listing)
- read("ai-kit/tools/bat.md", type="file"): Enhanced file viewer with syntax highlighting (REQUIRED for code inspection)
- read("ai-kit/tools/eza.md", type="file"): Modern directory listings with tree views (REQUIRED for structure inspection)
- read("ai-kit/tools/git.md", type="file"): Git operations and history analysis (optional)

**Critical Instructions:**

1. Read and internalize cleaner_persona.md risk framework (0-10 scale)
2. Review ALL tool documentation to understand capabilities before use
3. Intelligently select which tools are necessary based on the domain and cleanup scope
4. Knip is the PRIMARY and ONLY tool for dead code detection
5. Use terminal tools (ripgrep, jaq, fd, bat, eza) for all file operations and data processing
6. NEVER use deprecated commands: grep, jq, find, cat, ls
7. Use git only when specifically needed for history analysis

## Analysis Instructions

Please carefully review the **$ARGUMENTS** domain and identify and eliminate any redundant, duplicate, dead code, or files, as well as leftover and outdated logic that may have evolved organically when changing different versions and implementations during the development phase. Then review for unnecessarily complex logic, including spaghetti code, redundant logic, unnecessary wrappers and abstractions, and custom implementations where native solutions by packages or frameworks already exist.

Focus on improving poor readability and removing irrelevant or redundant methods that make the code messy, while ensuring that the core functionality, overall flow, and core logic are preserved while adheres to DRY (Don't Repeat Yourself) principles, and promoting consistency.

Begin your reasoning with some uncertainty, gradually building confidence as you explore alternative suggestions. Before reaching a conclusion, thoroughly argue for each option, comparing the current approach with potential new strategies without rushing to a decision. Starting with some caution, any change can risk breaking implicit assumptions. After checking we implemented only safe, incremental changes and verified.

After conducting a thorough analysis of the current codebase state, please confirm that no crucial logic was accidentally removed in the previous cleanup. then try identified several significant opportunities for further consolidation that align with your philosophy of native solutions and simplicity.

---

## Guidelines - MUST FOLLOW

**Tool Selection Principles:**

- **NEVER** use deprecated commands: `jq`, `grep`, `find`, `cat`, `ls`
- **ALWAYS** use modern terminal tools (see ai-kit/tools/ for full documentation):
  - `jaq` for JSON processing (faster than jq, same syntax)
  - `rg` (ripgrep) for text search and pattern detection
  - `fd` for file discovery (fallback: `rg --files` if fd unavailable)
  - `bat` for file viewing with syntax highlighting
  - `eza` for directory listings and tree structure inspection
- Use Knip as the PRIMARY and ONLY tool for dead code detection
- Intelligently select analysis tools based on domain scope and cleanup goals
- Reference tool documentation (ai-kit/tools/\*.md) to understand full capabilities before use
- All tool outputs should be saved to `/temp` directory to keep project root clean

**Available Analysis Tools:**

- **knip** (PRIMARY): Dead code detection - unused files, exports, dependencies (monorepo-aware, framework-aware)
- **ripgrep** (SECONDARY): Fast text search, pattern detection, cross-validation of Knip findings
- **git** (OPTIONAL): History analysis, blame, uncommitted changes - use only when analyzing git state

**Required Terminal Tools (see ai-kit/tools/ for usage):**

- **ripgrep** (`rg`): Fast text search, pattern detection, cross-validation of Knip findings
- **jaq**: JSON querying and transformation for processing tool outputs (Knip JSON, package.json, etc.)
- **fd**: Fast file discovery and listing (replaces `find`)
- **bat**: Enhanced file viewer with syntax highlighting (replaces `cat`)
- **eza**: Modern directory listings with tree views and git status (replaces `ls`)

**Evidence-Based Analysis:**

- Always reference cleaner_persona.md risk assessment framework (0-10 scale)
- Persist raw tool outputs in `/temp` (e.g., `temp/knip.json`, `temp/rg-no-imports.txt`)
- Provide evidence from tool outputs, not opinions
- Use quantified metrics (LOC, file counts, complexity scores)
- Cross-reference multiple tools for validation when needed

**Cross-Validation & Confidence Scoring:**

To reduce false positives and increase confidence in findings, use multi-tool verification:

**Workflow:**

1. **Primary Analysis** - Run Knip across the chosen scope

   ```bash
   # Monorepo sweep
   npx knip --reporter json > temp/knip.json

   # Single workspace (faster, replace <workspace-name> with actual workspace)
   npx knip --workspace <workspace-name> --reporter json > temp/knip-workspace.json
   ```

2. **Secondary Verification** - Use ripgrep to verify findings

   ```bash
   # Extract unused files from Knip using jaq (NOT jq)
   jaq -r '.files[] | .filePath' temp/knip.json > temp/knip-unused.txt

   # Verify with ripgrep (files with no imports)
   rg --files-without-match "import.*from|require\(" <target-directory> > temp/rg-no-imports.txt
   ```

   If the candidate file is excluded by `.gitignore`, rerun ripgrep with `--no-ignore` so generated artifacts are still inspected.

3. **Calculate Confidence Scores**

   ```bash
   # HIGH confidence (95%): Flagged by both Knip AND ripgrep
   comm -12 <(sort temp/knip-unused.txt) <(sort temp/rg-no-imports.txt) > temp/high-confidence.txt

   # MEDIUM confidence (40%): Flagged by Knip only
   comm -23 <(sort temp/knip-unused.txt) <(sort temp/rg-no-imports.txt) > temp/medium-confidence.txt
   ```

4. **Manual Verification** - For medium confidence findings, manually verify:

   ```bash
   # Check if file is used via dynamic imports
   rg "require\(.*filename|import\(.*filename" .

   # Check if exports are used
   rg "exportName" .

   # View file with syntax highlighting using bat (NOT cat)
   bat path/to/suspicious-file.ts
   ```

**Confidence Thresholds:**

| Evidence                               | Confidence | Action                           |
| -------------------------------------- | ---------- | -------------------------------- |
| Knip + ripgrep + manual verification   | 95%        | **DELETE** (safe)                |
| Knip + ripgrep                         | 85%        | **DELETE** (very likely safe)    |
| Knip only, ripgrep shows imports       | 20%        | **KEEP** (false positive)        |
| Knip only, manual verification unclear | 40%        | **REVIEW** (needs investigation) |

**Example Cross-Validation:**

```bash
# Knip says: "config/index.ts is unused"

# Step 1: Verify with ripgrep for static imports
rg "from.*config/index" .
# Output: (empty) = No static imports

# Step 2: Check for dynamic usage
rg "getNetworkConfig|getContractsForEnvironment" .
# Output: scripts/deploy-wizard.js:17 = Exports ARE used!

# Step 3: Inspect the file using bat (NOT cat)
bat config/index.ts

# Conclusion: FALSE POSITIVE - keep the file
# Confidence: 20% (Knip flagged, but ripgrep found usage)
```

**Reporting Structure:**

Report findings from the tools you actually used. Adapt the structure based on what's relevant to the domain. Examples:

**If analyzing dependencies:**

- Circular dependencies: [count and paths]
- Architectural violations: [specific violations found]
- High-coupling files: [files with >5 dependencies]

**If analyzing dead code:**

- Unused exports: [count and file locations]
- Unimported files: [count and paths]
- Unused npm packages: [package names]

**If analyzing code quality:**

- Skipped tests: [count and locations]
- Console.log statements: [count and locations]
- Complex functions: [functions >50 lines or >4 parameters]

**Adapt this structure** - only report what's relevant to your analysis. Don't force metrics that don't apply.

### 3. Build Verification (Mandatory for High Confidence)

Before recommending anything at ≥70% confidence, simulate the change and run the workspace build:

```bash
# Example build commands (adapt to your project's package manager and workspace names)
pnpm --filter <workspace-name> build
npm run build --workspace=<workspace-name>
yarn workspace <workspace-name> build

# If using monorepo tools like Turbo
pnpm turbo build --filter=<workspace-name>

# Single-repo projects
pnpm build
npm run build
yarn build
```

**Build Verification Steps:**

1. Remove or stub the flagged file locally before running the build
2. Run the build command for the affected workspace(s)
3. If a workspace lacks a build script, run its strongest verification command (tests, lint, typecheck)
4. Capture logs for failures and note any auto-generated artifacts that must be preserved
5. Restore the file if build fails

### 4. Deletions

File: [path/to/file]
Lines: [line numbers or "entire file"]
Risk: [0-10] - [justification using cleaner_persona.md framework]
Confidence: [20%/40%/85%/95%] - [based on cross-validation results]
Evidence:

- Knip: [flagged as unused/not flagged]
- ripgrep: [no imports found/imports found]
- Manual: [verified unused/verified used/unclear]
  Impact: [files affected, LOC removed]

### 5. Modifications

File: [path/to/file]
Lines: [line numbers]
Change: [brief description]
Reason: [why this improves the code]
Risk: [0-10] - [justification]
Evidence: [tool output supporting this change]
Impact: [complexity reduction, coupling improvement]

### 6. Consolidations (DRY)

Files: [list of files with duplication]
Proposed: [how to consolidate]
Risk: [0-10] - [justification]
Impact: [LOC reduction, maintainability improvement]

### 7. Final Recommendation

Provide a final summary with evidence-based analysis and RECOMMENDATIONS that Focus on actionable, high-impact recommendations for the codebase, including proactive suggestions beyond what was explicitly requested.

Include:

- Quantified impact (files, lines, complexity reduction)
- Priority ordering (what to do first)
- Verification steps (how to confirm changes are safe)
- Rollback plan (if something breaks)
