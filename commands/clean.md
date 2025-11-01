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
5. **Monorepo-Wide Sweep** *(omit the argument or pass `all`)*: Run the workflow across every workspace

**Your Task**: Interpret the domain argument intelligently and adapt tool usage accordingly.

### Running the Command

- **Entire Monorepo:** `clean` (no argument) or `clean all`
- **Single App:** `clean hardhat`, `clean docs`, `clean web`, etc.
- **Focused Module:** `clean apps/web/src/routes`

Confirm the intended scope before triggering heavy tools so you do not accidentally run a full sweep.

### Domain-Specific Tool Selection Examples

**For File/Directory Path** (e.g., `apps/hardhat/contracts`):

- Consider: knip (scoped to the workspace), dependency-cruiser (module structure), jscpd (duplication)
- Skip: unimported (unless checking entire app), git analysis (unless checking history)

**For Conceptual Domain** (e.g., `authentication`):

- Consider: ripgrep (pattern discovery), dependency-cruiser (coupling), jscpd (duplicated logic)
- Skip: ts-prune/unimported (too broad for conceptual work)

**For Git State** (e.g., `uncommitted`):

- Consider: git (to list uncommitted files), ripgrep (change inspection), jscpd (duplicate detection in new code)
- Skip: dependency-cruiser (too heavy), unimported (not relevant to git state)

**For Pattern-Based** (e.g., `*.test.ts`):

- Consider: ripgrep (find matches), knip (verify unused helpers), jscpd (duplicated setup)
- Skip: dependency-cruiser (unless checking test dependencies), unimported (tests are often isolated)

**Remember**: These are suggestions, not rules. Use your judgment based on the specific cleanup goals.

## MANDATORY PRE-ANALYSIS CHECKLIST

REQUIRED: Execute these view commands simultaneously to understand available capabilities:

- read("ai-kit/personas/cleaner_persona.md", type="file"): Risk assessment framework and decision-making principles
- read("ai-kit/tools/knip.md", type="file"): Modern dead code detection for monorepos (RECOMMENDED)
- read("ai-kit/tools/ripgrep.md", type="file"): Text pattern search and cross-validation capabilities
- read("ai-kit/tools/dependency-cruiser.md", type="file"): Dependency analysis and architectural validation
- read("ai-kit/tools/ts-prune.md", type="file"): Legacy unused export detection (review only if Knip cannot run)
- read("ai-kit/tools/unimported.md", type="file"): Legacy orphaned file detector (review only if Knip cannot run)
- read("ai-kit/tools/jscpd.md", type="file"): Semantic code duplication detection
- read("ai-kit/tools/git.md", type="file"): Git operations and history analysis

**Critical Instructions:**

1. Read and internalize cleaner_persona.md risk framework (0-10 scale)
2. Review ALL tool documentation to understand their full capabilities
3. Intelligently select which tools are necessary based on the domain and cleanup scope
4. Reference tool guides before using them to understand their complete feature set

## Analysis Instructions

Please carefully review the **$ARGUMENTS** domain and identify and eliminate any redundant, duplicate, dead code, or files, as well as leftover and outdated logic that may have evolved organically when changing different versions and implementations during the development phase. Then review for unnecessarily complex logic, including spaghetti code, redundant logic, unnecessary wrappers and abstractions, and custom implementations where native solutions by packages or frameworks already exist.

Focus on improving poor readability and removing irrelevant or redundant methods that make the code messy, while ensuring that the core functionality, overall flow, and core logic are preserved while adheres to DRY (Don't Repeat Yourself) principles, and promoting consistency.

Begin your reasoning with some uncertainty, gradually building confidence as you explore alternative suggestions. Before reaching a conclusion, thoroughly argue for each option, comparing the current approach with potential new strategies without rushing to a decision. Starting with some caution, any change can risk breaking implicit assumptions. After checking we implemented only safe, incremental changes and verified.

After conducting a thorough analysis of the current codebase state, please confirm that no crucial logic was accidentally removed in the previous cleanup. then try identified several significant opportunities for further consolidation that align with your philosophy of native solutions and simplicity.

---

## Guidelines - MUST FOLLOW

**Tool Selection Principles:**

- Never use `jq`; always use `jaq` for JSON processing (faster, same syntax)
- Never use `grep`; always use `rg` (ripgrep) for text patterns
- Use `fd` for file discovery; if unavailable fall back to `rg --files`; never use `find`
- Never use `cat`; always use `bat` for file viewing with syntax highlighting
- Prefer `eza` over `ls` for directory listings and structure inspection
- Default to Knip for dead-code detection; treat ts-prune/unimported as last-resort legacy tools
- Intelligently select analysis tools based on domain scope and cleanup goals
- Reference tool documentation to understand full capabilities before use
- All tool outputs should be saved to `/temp` directory to keep project root clean

**Available Analysis Tools (Select Based on Need):**

- **knip** (PRIMARY): Modern dead code detection - unused files, exports, dependencies (monorepo-aware, replaces ts-prune + unimported)
- **ripgrep**: Fast text pattern search, file listing, and cross-validation of tool findings
- **jscpd**: Semantic code duplication detection (AST-based)
- **dependency-cruiser**: Dependency analysis, circular dependencies, architectural violations, coupling metrics
- **git**: History analysis, blame, uncommitted changes

**Legacy Tools (Opt-In):**

- **ts-prune**: Unused export detection when Knip cannot parse a project
- **unimported**: Orphaned file detection for non-workspace Node projects

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

   # Single workspace (faster)
   npx knip --workspace apps/hardhat --reporter json > temp/knip-hardhat.json
   ```

2. **Secondary Verification** - Use ripgrep to verify findings

   ```bash
   # Extract unused files from Knip
   cat temp/knip.json | jaq -r '.files[] | .filePath' > temp/knip-unused.txt

   # Verify with ripgrep (files with no imports)
   rg --files-without-match "import.*from|require\(" apps/TARGET > temp/rg-no-imports.txt
   ```

   If the candidate file is excluded by `.gitignore` (for example `apps/web/src/routeTree.gen.ts`), rerun ripgrep with `--no-ignore` so generated artifacts are still inspected.

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
# Verify with ripgrep:
rg "from.*config/index" .
# Output: (empty) = No static imports

# Check for dynamic usage:
rg "getNetworkConfig|getContractsForEnvironment" .
# Output: scripts/deploy-wizard.js:17 = Exports ARE used!

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

**If analyzing duplication:**

- Code duplication blocks: [count, percentage, file locations]
- Semantic similarity: [files with similar logic]

**If analyzing code quality:**

- Skipped tests: [count and locations]
- Console.log statements: [count and locations]
- Complex functions: [functions >50 lines or >4 parameters]

**Adapt this structure** - only report what's relevant to your analysis. Don't force metrics that don't apply.

### 3. Build Verification (Mandatory for High Confidence)

Before recommending anything at ≥70% confidence, simulate the change and run the workspace build:

```bash
# Contracts & scripts
pnpm --filter hardhat build

# Goldsky subgraph
pnpm --filter production-lottery-enhanced-subgraph build

# Next.js docs
pnpm --filter docs build

# Vite trivia app
pnpm --filter web build

# Express API
pnpm --filter server build
```

- Remove or stub the flagged file locally before running the build.
- If a workspace lacks a build script, run its strongest verification command (tests, lint) and record that evidence.
- Capture logs for failures and note any auto-generated artifacts that must be preserved.

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
Duplication: [jscpd output showing duplicated blocks]
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
