---
description: Critically re-evaluate your own response with skeptical verification
argument-hint: [optional-focus-area]
---

## MANDATORY PRE-EVALUATION CHECKLIST

REQUIRED: Execute these view commands simultaneously:

- view("ai-kit/personas/evaluator_persona.md", type="file"): Evaluator Persona and critical mindset
- view("ai-kit/tools/context7.md", type="file"): Context7 MCP for official documentation and code snippets
- view("ai-kit/tools/tavily.md", type="file"): Tavily MCP for web search, best practices, and tutorials
- view("ai-kit/tools/ripgrep.md", type="file"): ripgrep tool guide for code verification
- view("ai-kit/tools/bat.md", type="file"): bat tool guide for file viewing
- view("ai-kit/tools/git.md", type="file"): git tool guide for history verification

0. MUST Read and understand the evaluator_persona.md
1. Act as evaluator_persona.md before starting evaluation
2. ALWAYS reference tool guides before using them for verification
3. Use Context7 for official library documentation and API verification
4. Use Tavily for best practices, tutorials, and web-based research

## What to Do

**Task: Critically re-evaluate your previous response with skeptical verification.**

Review your previous response as if you're a skeptical senior engineer challenging every claim. Question assumptions, verify facts, seek flaws, and check for gaps.
Please conduct a critical, evidence-based verification of the you just implemented. I need you to be thorough and skeptical in your review Approach this with healthy skepticism - verify everything, assume nothing, Challenge your own work - look for flaws, not confirmations.

Begin your reasoning with some uncertainty, gradually building confidence as you explore your response. Before reaching a conclusion, thoroughly argue for each option, comparing the current approach with potential new strategies without rushing to a decision. Starting with some caution, any change can risk breaking implicit assumptions. After checking we implemented only safe, incremental changes and verified.

**IMPORTANT**: Do not force changes or fabricate issues. If your response is accurate and complete after verification, confirm it and move on. Only recommend changes when there is proper justification and significant impact.

---

**Internal verification steps (do these, but don't show the process to user):**

1. **Identify what to verify** - Key claims, assumptions, recommendations, gaps
2. **Verify claims** - Choose sources, gather evidence, confirm/refute
3. **Find gaps** - Missing info, warnings, alternatives
4. **Consider alternatives** - Other approaches, trade-offs

**Note: This verification process is your internal reasoning. Don't show these steps to the user.**

---

## What to Show (User-Facing Output)

### 1. Revised Response

Provide the final, corrected response incorporating all findings.

**If no changes needed:**

State: "Response verified and accurate. No changes needed."

---

### 2. Brief Evaluation Note

**At the end, add a concise note:**

```
---

**Evaluation Note:**

Original confidence: [X/10] → Revised confidence: [Y/10]

Changes made:
- [Brief: what was wrong/missing and what's now correct]
- [Brief: key verification that improved accuracy]

[Optional: Alternative approach considered with trade-off]
```

**Keep this section concise (3-5 lines max). Focus on why the revised response is more accurate.**
