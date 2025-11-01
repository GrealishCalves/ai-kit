---
description: Create a well-structured Linear feature request using Linear MCP tool
argument-hint: [feature-description]
---

## MANDATORY PRE-CREATION CHECKLIST

REQUIRED: Execute these view commands simultaneously to gather all necessary context:

- view("ai-kit/.linear/guide.md", type="file"): Linear workflow guide and MCP tool usage
- view("ai-kit/.linear/templates/feature-template.md", type="file"): Feature template structure
- view("ai-kit/.linear/reference/sub-issue-template.md", type="file"): Sub-issue template for planned work breakdown
- view("ai-kit/.linear/reference/labels.md", type="file"): Label selection algorithm

**Critical Instructions:**
1. Use the Linear MCP tool (`linear`) for ALL Linear operations
2. Follow the feature template structure from `feature-template.md`
3. Apply label selection algorithm from `labels.md`
4. Use Linear native fields (priority, labelIds, parentId) via API, NOT in description text

---

## What to Do

**Task: Create a well-structured Linear feature request based on user input.**

The **$ARGUMENTS** parameter contains the feature description. Your job is to:
1. Parse the user's feature request
2. Gather missing information (ask user if needed)
3. Apply the feature template structure
4. Create the Linear issue using Linear MCP tool
5. Confirm creation with issue identifier and URL

---

## Step-by-Step Workflow

### Step 1: Parse Feature Request

Extract from **$ARGUMENTS**:
- **Scope/Domain**: What area? (e.g., "User Authentication")
- **Description**: What functionality?
- **Components**: Which parts? (frontend, backend, database, etc.)
- **Priority**: How urgent? (1=Urgent, 2=High, 3=Medium, 4=Low)

**If information is missing, ASK the user.**

### Step 2: Fetch Linear Team and Labels

Query Linear MCP tool to get team ID and available labels.

**Reference:** See `.linear/guide.md` (lines 299-310) for Linear MCP tool syntax.

### Step 3: Apply Label Selection Algorithm

**Reference:** Follow the algorithm in `.linear/reference/labels.md`

**Required labels:**
- Type: "Feature" (exactly 1)
- Component: At least 1 from [frontend, backend, database, infrastructure, mobile, api, other]
- Domain: Optional (e.g., user-management, payments, analytics)

### Step 4: Structure Feature Description

**Reference:** Use the template from `.linear/templates/feature-template.md`

**Required sections:**
- Overview
- Scope (In Scope / Out of Scope)
- Technical Approach
- Acceptance Criteria
- Testing Strategy

### Step 5: Create Linear Issue

Use Linear MCP tool to create the issue with:
- Title format: `[Scope] - [Description]`
- Full markdown description from template
- Team ID, priority, and label IDs

### Step 6: Handle Sub-tasks (If Needed)

**Ask user:** "Should this feature be broken down into sub-tasks?"

**If YES:** Create sub-tasks with `parentId` set to parent feature ID.

**Reference:** See `.linear/guide.md` (lines 396-406) for sub-task creation.

**When to use sub-tasks:**
- ✅ Planned work breakdown known at planning time
- ❌ Bugs discovered during implementation (use "related" instead)

### Step 7: Confirm Creation

Provide user with:
- Issue identifier (e.g., "ENG-123")
- Issue URL
- Summary of what was created
- Next steps

---

## Validation Checklist

**Reference:** See `.linear/templates/feature-template.md` for complete validation checklist.

**Key validations:**
- [ ] Title follows format: `[Scope] - [Description]`
- [ ] All required template sections are complete
- [ ] Priority set via API (1-4)
- [ ] Labels applied via API (Feature + components + domain)
- [ ] Team ID is valid

---

## Common Scenarios

**Reference:** See `.linear/reference/examples.md` for detailed scenarios.

**Quick examples:**
- **Simple feature:** Single issue, no sub-tasks
- **Complex feature:** Parent issue + sub-tasks for planned work breakdown
- **Cross-component:** Multiple component labels on single issue

---

## Error Handling

**Common errors:**
- Missing team ID → Verify Linear MCP tool configuration
- Label not found → List available labels, ask user to create missing ones
- Invalid priority → Default to Medium (3)
- Missing required fields → Ask user for details

---

## Quick Reference

**Priority Values:**
- 1 = Urgent
- 2 = High
- 3 = Medium
- 4 = Low

**Required Labels:**
- Type: "Feature" (exactly 1)
- Component: At least 1 from [frontend, backend, database, infrastructure, mobile, api, other]
- Domain: Optional

**Key References:**
- Linear MCP tool syntax: `.linear/guide.md` (lines 299-310)
- Label selection algorithm: `.linear/reference/labels.md`
- Feature template: `.linear/templates/feature-template.md`
- Sub-task creation: `.linear/guide.md` (lines 396-406)

