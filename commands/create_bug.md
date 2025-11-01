---
description: Create a well-structured Linear bug report using Linear MCP tool
argument-hint: [bug-description]
---

## MANDATORY PRE-CREATION CHECKLIST

REQUIRED: Execute these view commands simultaneously to gather all necessary context:

- view("ai-kit/.linear/guide.md", type="file"): Linear workflow guide and MCP tool usage
- view("ai-kit/.linear/templates/bug-template.md", type="file"): Bug template structure
- view("ai-kit/.linear/reference/labels.md", type="file"): Label selection algorithm (includes bug categories)

**Critical Instructions:**
1. Use the Linear MCP tool (`linear`) for ALL Linear operations
2. Follow the bug template structure from `bug-template.md`
3. Apply label selection algorithm from `labels.md` (including bug category - REQUIRED)
4. Use Linear native fields (priority, labelIds, assigneeId) via API, NOT in description text

---

## What to Do

**Task: Create a well-structured Linear bug report based on user input.**

The **$ARGUMENTS** parameter contains the bug description. Your job is to:
1. Parse the user's bug report
2. Gather missing information (ask user if needed)
3. Apply the bug template structure
4. Create the Linear issue using Linear MCP tool
5. Confirm creation with issue identifier and URL

---

## Step-by-Step Workflow

### Step 1: Parse Bug Report

Extract from **$ARGUMENTS**:
- **Component**: Which part is broken? (frontend, backend, database, etc.)
- **Description**: What's not working?
- **Severity**: How critical? (Critical, High, Medium, Low)
- **Bug Category**: What type? (logic-error, display-error, query-error, etc.)
- **Steps to Reproduce**: How to trigger?
- **Root Cause**: Where is the issue? (file path, line number)

**If information is missing, ASK the user.**

### Step 2: Fetch Linear Team and Labels

Query Linear MCP tool to get team ID and available labels.

**Reference:** See `.linear/guide.md` (lines 299-310) for Linear MCP tool syntax.

### Step 3: Apply Label Selection Algorithm

**Reference:** Follow the algorithm in `.linear/reference/labels.md`

**Required labels:**
- Type: "Bug" (exactly 1)
- Component: At least 1 from [frontend, backend, database, infrastructure, mobile, api, other]
- Bug Category: Exactly 1 from [logic-error, display-error, query-error, state-sync, performance, security, integration, other] - **REQUIRED for bugs**
- Domain: Optional (e.g., user-management, payments, analytics)

**Bug Category Selection:** See `.linear/reference/labels.md` (lines 99-116) for category descriptions and keywords.

### Step 4: Determine Priority from Severity

**Map severity to priority:**
- Critical → Priority 1
- High → Priority 2
- Medium → Priority 3
- Low → Priority 4

### Step 5: Structure Bug Description

**Reference:** Use the template from `.linear/templates/bug-template.md`

**Required sections:**
- Summary
- Impact (Severity, Affected Users, Workaround)
- Steps to Reproduce
- Expected vs Actual Behavior
- Root Cause
- Proposed Fix
- Verification

### Step 6: Create Linear Issue

Use Linear MCP tool to create the issue with:
- Title format: `Bug: [Component] - [Description]`
- Full markdown description from template
- Team ID, priority, and label IDs (including bug category)

### Step 7: Link to Related Feature (If Applicable)

**Ask user:** "Was this bug discovered during feature implementation?"

**If YES:** Query feature to get UUID, then create "related" relationship.

**Reference:** See `.linear/guide.md` (lines 427-442) for relationship creation.

**Important:** Use "related" relationship, NOT sub-task.

### Step 8: Confirm Creation

Provide user with:
- Issue identifier (e.g., "ENG-456")
- Issue URL
- Summary of what was created
- Related feature (if linked)
- Next steps

---

## Validation Checklist

**Reference:** See `.linear/templates/bug-template.md` for complete validation checklist.

**Key validations:**
- [ ] Title follows format: `Bug: [Component] - [Description]`
- [ ] All required template sections are complete
- [ ] Bug category label is included (REQUIRED for bugs)
- [ ] Priority set via API (1-4) based on severity
- [ ] Labels applied via API (Bug + components + bug category + domain)
- [ ] Team ID is valid

---

## Common Scenarios

**Reference:** See `.linear/reference/examples.md` for detailed scenarios.

**Quick examples:**
- **Critical bug:** Severity=Critical, Priority=1, immediate assignment
- **UI issue:** Severity=Medium, Category=display-error
- **Bug during development:** Link to related feature using "related" relationship
- **Performance issue:** Category=performance, include metrics

---

## Bug Category Selection Guide

**Reference:** See `.linear/reference/labels.md` (lines 99-116) for complete bug category schema.

**Quick reference:**
- `logic-error` - Business logic incorrect
- `display-error` - UI rendering issues
- `query-error` - Data fetching issues
- `state-sync` - State synchronization problems
- `performance` - Performance issues
- `security` - Security vulnerabilities
- `integration` - Third-party integration issues
- `other` - Other categories

---

## Error Handling

**Common errors:**
- Missing bug category → Infer from description or ask user (REQUIRED for bugs)
- Invalid severity → Ask user to clarify
- Missing steps to reproduce → Ask user for detailed steps
- Related feature not found → Ask user to verify feature identifier

---

## Quick Reference

**Severity to Priority Mapping:**
- Critical → Priority 1
- High → Priority 2
- Medium → Priority 3
- Low → Priority 4

**Required Labels:**
- Type: "Bug" (exactly 1)
- Component: At least 1
- Bug Category: Exactly 1 (REQUIRED)
- Domain: Optional

**Key References:**
- Linear MCP tool syntax: `.linear/guide.md` (lines 299-310)
- Label selection algorithm: `.linear/reference/labels.md`
- Bug template: `.linear/templates/bug-template.md`
- Bug categories: `.linear/reference/labels.md` (lines 99-116)
- Relationship creation: `.linear/guide.md` (lines 427-442)

