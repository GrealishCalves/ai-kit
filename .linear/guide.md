# Linear Workflow Guide

Complete guide for using Linear with AI agents via MCP (Model Context Protocol).

---

## 📋 Table of Contents

1. [Quick Start](#quick-start)
2. [Issue Types & Naming](#issue-types--naming)
3. [Labels](#labels)
4. [Projects](#projects)
5. [Templates](#templates)
6. [AI agent Workflow](#ai-agent-workflow)
7. [Relationships](#relationships)
8. [Status Management](#status-management)
9. [Comments](#comments)

---

## Quick Start

### For AI agents

1. **Read** [AI agent Workflow](#ai-agent-workflow) section
2. **Use** Linear MCP GraphQL API
3. **Apply** label selection algorithm
4. **Create** issue with native fields

---

## Issue Types & Naming

### Title Formats

| Type            | Format                             | Example                                       |
| --------------- | ---------------------------------- | --------------------------------------------- |
| **Feature**     | `[Scope] - [Description]`          | `User Authentication - Add OAuth login`       |
| **Bug**         | `Bug: [Component] - [Description]` | `Bug: Frontend - Login button not responding` |
| **Improvement** | `[Component] - [Description]`      | `Database - Optimize user query performance`  |

### Rules

- ✅ Keep titles concise and scannable
- ✅ Include component in title
- ❌ Don't include priority/status in title (use Linear fields)
- ❌ Don't include git commit info in title

---

## Labels

### Required Labels

Every issue MUST have:

1. **ONE type label:** `Feature`, `Bug`, or `Improvement`
2. **AT LEAST ONE component label:**
   - **Core components:** `frontend`, `backend`, `contract`
   - **Additional components:** `database`, `infrastructure`, `api`, `documentation`, `testing`, `security`, `performance`, `other`

### Optional Labels

- **Domain labels:** `user-management`, `payments`, `analytics`, `notifications`, `auth`, `data-processing` (project-specific)
- **Bug category** (bugs only): `logic-error`, `display-error`, `query-error`, `state-sync`, `performance`, `security`

### Label Details

See [labels.md](./reference/labels.md) for complete label schema and selection algorithms.

---

## Projects

Projects are units of work with clear outcomes and completion dates. They organize issues across teams and provide progress tracking.

### When to Use Projects

**Use Projects for:**

- ✅ Multi-issue initiatives with clear goals
- ✅ Cross-team collaboration
- ✅ Time-bound deliverables
- ✅ Feature rollouts or major improvements

**Don't Use Projects for:**

- ❌ Single issues (use labels instead)
- ❌ Ongoing maintenance work
- ❌ Ad-hoc bug fixes

### Project Properties

| Field         | Type   | Required       | Description                                             |
| ------------- | ------ | -------------- | ------------------------------------------------------- |
| `name`        | String | ✅ Yes         | Project name (concise, descriptive)                     |
| `description` | String | ⚠️ Recommended | Project goals and scope                                 |
| `teamIds`     | Array  | ✅ Yes         | Teams involved (at least one)                           |
| `state`       | Enum   | Auto           | `backlog`, `started`, `paused`, `completed`, `canceled` |
| `leadId`      | UUID   | Optional       | Project lead (user ID)                                  |
| `startDate`   | Date   | Optional       | Project start date                                      |
| `targetDate`  | Date   | ⚠️ Recommended | Target completion date                                  |

### Creating Projects via MCP

**GraphQL Mutation:**

```graphql
mutation CreateProject {
  projectCreate(input: { name: "User Authentication System", description: "Implement OAuth 2.0 login with social providers", teamIds: ["team-uuid-here"], targetDate: "2025-12-31" }) {
    success
    project {
      id
      name
      url
    }
  }
}
```

**Natural Language (via Linear MCP):**

```
Create a project named "User Authentication System" with description "Implement OAuth 2.0 login with social providers" for team ID [team-uuid] with target date 2025-12-31
```

### Adding Issues to Projects

**When Creating Issue:**

```graphql
mutation CreateIssue {
  issueCreate(input: { title: "Implement Google OAuth provider", teamId: "team-uuid", projectId: "project-uuid", labelIds: ["feature-label-uuid"], priority: 2 }) {
    success
    issue {
      id
      identifier
      project {
        name
      }
    }
  }
}
```

**Updating Existing Issue:**

```graphql
mutation AddToProject {
  issueUpdate(id: "issue-uuid", input: { projectId: "project-uuid" }) {
    success
    issue {
      identifier
      project {
        name
      }
    }
  }
}
```

### Project Documents

Projects can contain documents for specs, PRDs, and status updates. See [Project Documents](#project-documents) section below.

### Best Practices

**Naming:**

- ✅ Use clear, outcome-focused names
- ✅ Example: "User Authentication System" (not "Auth Work")
- ✅ Example: "Payment Integration v2" (not "Payments")

**Scope:**

- ✅ Define clear success criteria in description
- ✅ Set realistic target dates
- ✅ Assign project lead for accountability

**Organization:**

- ✅ Group related issues under projects
- ✅ Use project milestones for phases
- ✅ Update project state as work progresses

---

## Project Documents

Project documents are collaborative documents within projects for specs, PRDs, status updates, and technical designs.

### When to Use Documents

**Use Documents for:**

- ✅ Technical specifications
- ✅ Product requirements (PRDs)
- ✅ Architecture decisions
- ✅ Project status updates
- ✅ Implementation plans

**Don't Use Documents for:**

- ❌ Issue descriptions (use issue body instead)
- ❌ Code snippets (use GitHub/GitLab)
- ❌ External documentation (link instead)

### Document Properties

| Field       | Type   | Required | Description       |
| ----------- | ------ | -------- | ----------------- |
| `title`     | String | ✅ Yes   | Document title    |
| `content`   | String | ✅ Yes   | Markdown content  |
| `projectId` | UUID   | ✅ Yes   | Parent project ID |

### Creating Documents via MCP

**GraphQL Mutation:**

```graphql
mutation CreateDocument {
  documentCreate(input: { title: "OAuth Implementation Spec", content: "# OAuth 2.0 Implementation\n\n## Overview\n...", projectId: "project-uuid" }) {
    success
    document {
      id
      title
      url
      project {
        name
      }
    }
  }
}
```

**Natural Language (via Linear MCP):**

```
Create a document titled "OAuth Implementation Spec" with content "# OAuth 2.0 Implementation..." for project ID [project-uuid]
```

### Document Features

**Markdown Support:**

- ✅ Full Markdown syntax
- ✅ Code blocks with syntax highlighting
- ✅ Tables, lists, headings
- ✅ Links and images

**Collaboration:**

- ✅ Inline comments
- ✅ Version history
- ✅ Real-time editing
- ✅ @mentions

### Best Practices

**Structure:**

- ✅ Use clear headings hierarchy
- ✅ Include table of contents for long docs
- ✅ Add context and background

**Content:**

- ✅ Keep specs up-to-date as project evolves
- ✅ Link to related issues
- ✅ Include diagrams/mockups when helpful

**Organization:**

- ✅ One document per major topic
- ✅ Use consistent naming convention
- ✅ Archive outdated documents

---

## Templates

All templates are in `templates/` directory:

- **[feature-template.md](./templates/feature-template.md)** - New functionality
- **[bug-template.md](./templates/bug-template.md)** - Broken functionality
- **[improvement-template.md](./templates/improvement-template.md)** - Enhancements
- **[sub-issue-template.md](./templates/sub-issue-template.md)** - Planned work breakdown (sub-tasks)

Each template includes:

- AI agent instructions
- Required fields checklist
- Domain-agnostic examples
- GraphQL mutation examples
- Validation checklist

---

## AI agent Workflow

**🚨 CRITICAL COMMENT RULE FOR AI AGENTS:**

**❌ NEVER put technical details, specifications, or implementation details in comments.**

**✅ ALL technical content MUST go in the issue description.**

**Comments are ONLY for:**

1. Brief status updates during active work (2-3 sentences max)
2. DoD (Definition of Done) when marking issue as Done
3. Brief blocker notes

See [Comments](#comments) section for complete rules.

---

### Linear MCP Tools

| Tool                           | Purpose                      |
| ------------------------------ | ---------------------------- |
| `linear_create_issue`          | Create new issue or sub-task |
| `linear_update_issue`          | Modify existing issue        |
| `linear_search_issues`         | Find issues by criteria      |
| `linear_get_issue`             | Retrieve issue details       |
| `linear_create_comment`        | Add comment to issue         |
| `linear_create_issue_relation` | Link two issues              |
| `linear_create_project`        | Create new project           |
| `linear_create_document`       | Create project document      |

### GraphQL Issue Creation

```graphql
mutation IssueCreate {
  issueCreate(input: { title: "Bug: Frontend - Login button not responding", description: "...", teamId: "TEAM-ID", priority: 2, labelIds: ["BUG-ID", "FRONTEND-ID", "LOGIC-ERROR-ID"], stateId: "STATE-TODO-ID", assigneeId: "USER-ID" }) {
    success
    issue {
      id
      identifier
      url
    }
  }
}
```

### Priority Values

| Priority | Value | Use When                            |
| -------- | ----- | ----------------------------------- |
| Urgent   | `1`   | Critical, blocking, security issues |
| High     | `2`   | Important, affects many users       |
| Medium   | `3`   | Normal priority                     |
| Low      | `4`   | Nice to have, minor issues          |

### Label Selection Process

AI agents should follow this process when selecting labels:

**Step 1: Fetch All Labels**

- Use Linear API to retrieve all labels for the team

**Step 2: Find Type Label (Required)**

- Match input type (Feature/Bug/Improvement) against label names using case-insensitive matching
- Error if not found: "Type label '[type]' not found"

**Step 3: Find Component Labels (Required - At Least One)**

- Match each input component against label names using case-insensitive matching
- Error if none found: "No component labels found for: [components]"

**Step 4: Find Domain Label (Optional)**

- If domain provided, match against label names
- Skip if not found (domain is optional)

**Step 5: Find Bug Category Label (Required for Bugs Only)**

- If issue type is "Bug":
  - Error if category missing: "Bug category is required for Bug issues"
  - Match bug category against label names
  - Error if not found: "Bug category label '[category]' not found"

**Step 6: Collect All Label IDs**

- Combine: type label ID + component label IDs + domain label ID (if found) + bug category label ID (if applicable)
- Return array of label IDs for use in Linear API

### State Resolution Process

AI agents should follow this process when resolving state names to state IDs:

**Step 1: Fetch All States**

- Use Linear API to retrieve all workflow states for the team

**Step 2: Try Exact Match**

- Match input state name against state names using case-insensitive matching
- If found, return the state ID

**Step 3: Fallback to Default State**

- If no exact match found, look for default states: "Backlog" or "Todo"
- Return the first default state ID found

**Step 4: Ultimate Fallback**

- If no default state found, return the first state ID from the list
- This ensures a valid state is always returned

### Creating Sub-tasks

```graphql
mutation CreateSubTask {
  issueCreate(input: { title: "Sub-task title", parentId: "PARENT-ISSUE-ID", teamId: "TEAM-ID", labelIds: ["FEATURE-ID", "COMPONENT-ID"] }) {
    success
    issue {
      id
      identifier
    }
  }
}
```

### Linking Related Issues

```graphql
mutation LinkIssues {
  issueRelationCreate(
    input: {
      issueId: "CURRENT-ISSUE-ID"
      relatedIssueId: "RELATED-ISSUE-ID"
      type: related # or: blocks, duplicate
    }
  ) {
    success
  }
}
```

> **Note:** As of 2025-10-31, the Linear MCP tool has a known bug with `issueRelationCreate` mutation (GraphQL validation error). This is a tool issue, not a documentation issue. The mutation syntax above is correct per Linear API documentation.

---

## Relationships

Linear supports multiple relationship types to connect related issues. Understanding when to use each type is critical for maintaining clear issue dependencies.

### Relationship Types Overview

| Relationship Type | Field/Type     | Direction      | Use Case                                   |
| ----------------- | -------------- | -------------- | ------------------------------------------ |
| **Sub-tasks**     | `parentId`     | Parent → Child | Planned work breakdown (known at planning) |
| **Related**       | `related`      | Bidirectional  | General association between issues         |
| **Blocks**        | `blocks`       | Directional    | Issue A blocks Issue B from progressing    |
| **Blocked By**    | `blockedBy`    | Directional    | Issue A is blocked by Issue B              |
| **Duplicate**     | `duplicate`    | Directional    | Issue A duplicates Issue B (close A)       |
| **Duplicated By** | `duplicatedBy` | Directional    | Issue A is duplicated by Issue B (close B) |

### 1. Sub-tasks (Parent-Child Hierarchy)

**When to Use:**

- Breaking down large features into smaller tasks
- Work breakdown is known at planning time
- Need hierarchical structure visible in Linear UI

**How to Create:**

Use `parentId` field when creating child issue:

```typescript
// Create parent issue first
const parent = await linear({
  query: "Create issue 'User Authentication' for team CHA",
  is_read_only: false,
  summary: "Create parent issue",
});

// Create child issue with parentId
await linear({
  query: `Create issue 'Add OAuth provider' with parent ${parent.id}`,
  is_read_only: false,
  summary: "Create child issue",
});
```

**Visual Structure:**

```
Parent: CHA-123 - User Authentication
├── CHA-124: Add OAuth provider configuration
├── CHA-125: Implement OAuth callback endpoints
└── CHA-126: Update login UI with OAuth buttons
```

**Best Practices:**

- ✅ Use for planned work breakdown
- ✅ Create parent first, then children
- ✅ Keep hierarchy shallow (max 2 levels)
- ❌ Don't use for bugs discovered during implementation
- ❌ Don't create deep nesting (3+ levels)

### 2. Related Issues

**When to Use:**

- General association between issues
- Bugs discovered during feature implementation
- Issues that share context but aren't blocking
- Cross-team dependencies without strict blocking

**How to Create:**

```typescript
// Get issue UUID first (required)
const issueA = await linear({
  query: "Get issue CHA-100",
  is_read_only: true,
  summary: "Fetch issue A",
});

// Create relationship using UUID
await linear({
  query: `Create related relationship between CHA-100 and CHA-101 using UUID ${issueA.id}`,
  is_read_only: false,
  summary: "Link related issues",
});
```

**Important:** Must use UUID, not issue identifier (CHA-100)

**Example Scenarios:**

```
Feature: CHA-100 - Payment Integration
Related: CHA-101 - Bug: Payment validation error (discovered during implementation)

Feature: CHA-200 - User Dashboard
Related: CHA-201 - Feature: Analytics API (shares data source)
```

**Best Practices:**

- ✅ Use for bugs found during feature work
- ✅ Use for issues sharing context
- ✅ Use when strict blocking isn't needed
- ❌ Don't use when one issue blocks another (use blocks/blockedBy)
- ❌ Don't use for planned work breakdown (use sub-tasks)

### 3. Blocks / Blocked By

**When to Use:**

- Issue A must be completed before Issue B can start
- Hard dependencies between issues
- Critical path planning
- Cross-team blocking dependencies

**How to Create:**

```typescript
// Get blocker issue UUID
const blocker = await linear({
  query: "Get issue CHA-50",
  is_read_only: true,
  summary: "Fetch blocker issue",
});

// Create blocking relationship
await linear({
  query: `Create blocks relationship: CHA-50 blocks CHA-51 using UUID ${blocker.id}`,
  is_read_only: false,
  summary: "Create blocking relationship",
});
```

**Direction Matters:**

- **blocks:** CHA-50 blocks CHA-51 (CHA-50 must finish first)
- **blockedBy:** CHA-51 is blocked by CHA-50 (same relationship, opposite direction)

**Example Scenarios:**

```
Blocker: CHA-50 - Database migration
Blocked: CHA-51 - Update API to use new schema

Blocker: CHA-100 - OAuth provider setup
Blocked: CHA-101 - Implement OAuth login flow
```

**Best Practices:**

- ✅ Use for hard dependencies
- ✅ Use when one issue must complete before another starts
- ✅ Use for critical path planning
- ❌ Don't overuse (creates rigid dependencies)
- ❌ Don't use for soft dependencies (use related instead)

### 4. Duplicate / Duplicated By

**When to Use:**

- Issue A is a duplicate of Issue B
- Consolidating duplicate reports
- Closing redundant issues

**How to Create:**

```typescript
// Get original issue UUID
const original = await linear({
  query: "Get issue CHA-200",
  is_read_only: true,
  summary: "Fetch original issue",
});

// Mark CHA-201 as duplicate of CHA-200
await linear({
  query: `Create duplicate relationship: CHA-201 duplicates CHA-200 using UUID ${original.id}`,
  is_read_only: false,
  summary: "Mark as duplicate",
});

// Close duplicate issue
await linear({
  query: "Update CHA-201 state to Canceled",
  is_read_only: false,
  summary: "Close duplicate",
});
```

**Direction Matters:**

- **duplicate:** CHA-201 duplicates CHA-200 (close CHA-201, keep CHA-200)
- **duplicatedBy:** CHA-200 is duplicated by CHA-201 (same relationship, opposite direction)

**Best Practices:**

- ✅ Always close the duplicate issue
- ✅ Keep the original issue (first reported or more detailed)
- ✅ Add comment explaining why it's a duplicate
- ❌ Don't leave duplicate issues open
- ❌ Don't create duplicate relationships without closing one

### AI Agent Implementation

**Step 1: Query Issue to Get UUID**

All relationship mutations require UUIDs, not issue identifiers:

```typescript
// ❌ WRONG: Using issue identifier
await linear({
  query: "Create blocks relationship: CHA-50 blocks CHA-51",
  // This will fail: "relatedIssueId must be a UUID"
});

// ✅ CORRECT: Query first to get UUID
const blocker = await linear({
  query: "Get issue CHA-50",
  is_read_only: true,
  summary: "Fetch blocker UUID",
});

const blockerUUID = blocker.issue.id; // Extract UUID

await linear({
  query: `Create blocks relationship using UUID ${blockerUUID} for CHA-50 blocks CHA-51`,
  is_read_only: false,
  summary: "Create blocking relationship",
});
```

**Step 2: Create Relationship**

Use natural language with UUID included:

```typescript
// Blocks relationship
await linear({
  query: `Create blocks relationship: CHA-50 blocks CHA-51 using UUID ${uuid}`,
  is_read_only: false,
  summary: "Create blocking relationship",
});

// Related relationship
await linear({
  query: `Create related relationship between CHA-100 and CHA-101 using UUID ${uuid}`,
  is_read_only: false,
  summary: "Link related issues",
});

// Duplicate relationship
await linear({
  query: `Create duplicate relationship: CHA-201 duplicates CHA-200 using UUID ${uuid}`,
  is_read_only: false,
  summary: "Mark as duplicate",
});
```

### Common Errors & Solutions

**Error 1: "relatedIssueId must be a UUID"**

```
❌ Problem: Using issue identifier (CHA-50) instead of UUID
✅ Solution: Query issue first to get UUID, then use UUID in relationship mutation
```

**Error 2: "Issue not found"**

```
❌ Problem: Invalid issue identifier or UUID
✅ Solution: Verify issue exists and identifier is correct
```

**Error 3: "Circular dependency detected"**

```
❌ Problem: Creating blocking relationship that creates a cycle
✅ Solution: Review dependency chain and remove circular dependencies
```

### Relationship Selection Guide

**Use Sub-tasks when:**

- Work breakdown is known at planning time
- Need hierarchical structure in Linear UI
- Tasks are part of same feature/epic

**Use Related when:**

- Issues share context but no strict dependency
- Bugs discovered during feature implementation
- Cross-team collaboration without blocking

**Use Blocks/Blocked By when:**

- Hard dependency exists (A must finish before B starts)
- Critical path planning needed
- Cross-team blocking dependencies

**Use Duplicate when:**

- Issue is exact duplicate of another
- Consolidating duplicate reports
- Closing redundant issues

---

## Status Management

### Workflow States

| State           | Type        | Description               | When to Use                                                     |
| --------------- | ----------- | ------------------------- | --------------------------------------------------------------- |
| **Triage**      | `triage`    | Inbox for new issues      | Issues from integrations, external sources, or non-team members |
| **Backlog**     | `backlog`   | Not yet prioritized       | Issues accepted but not scheduled                               |
| **Todo**        | `unstarted` | Ready to work on          | Issues ready to be picked up                                    |
| **In Progress** | `started`   | Currently being worked on | Active development                                              |
| **In Review**   | `started`   | Under review (PR open)    | Code review in progress                                         |
| **Done**        | `completed` | Completed and merged      | Work finished and deployed                                      |
| **Canceled**    | `canceled`  | Won't be done             | Duplicate, invalid, or rejected                                 |

### Triage Workflow

**What is Triage?**

Triage is a special inbox state for issues that need review before being accepted into the team's workflow. Issues automatically enter Triage when:

- Created via integrations (Slack, support tools, GitHub sync)
- Created by non-team members
- Created via public forms

**Triage Process:**

1. **Review** - Team reviews new issues in Triage
2. **Validate** - Confirm issue is valid and actionable
3. **Enrich** - Add labels, priority, assignee, project
4. **Move** - Transition to Backlog or Todo (or Cancel if invalid)

**GraphQL Example:**

```graphql
query GetTriageIssues {
  issues(filter: { state: { type: { eq: "triage" } } }) {
    nodes {
      id
      identifier
      title
      state {
        name
        type
      }
    }
  }
}
```

**Moving from Triage:**

```graphql
mutation MoveFromTriage {
  issueUpdate(id: "issue-uuid", input: { stateId: "backlog-state-uuid", labelIds: ["feature-label-uuid", "frontend-label-uuid"], priority: 3 }) {
    success
    issue {
      identifier
      state {
        name
      }
    }
  }
}
```

### State Transition Rules

- ✅ Bug status is independent of parent feature status
- ✅ Move issues through states as work progresses
- ✅ Use "In Review" when PR is open
- ✅ Use "Done" only when merged and deployed
- ✅ Process Triage issues regularly (daily or weekly)
- ✅ Don't skip Triage for external issues

---

## Comments

**CRITICAL RULE:** Comments are ONLY for quick progress updates during active work and DoD (Definition of Done) when completing issues.

**❌ NEVER put in comments:**

- Technical specifications or implementation details
- Contract rules, validation logic, or business requirements
- Code examples, file references, or architecture decisions
- Detailed checklists or acceptance criteria

**✅ ALL technical details MUST go in the issue description**, not comments.

### Comment Types and Use Cases

| Comment Type      | When to Use                                       | Required Format            |
| ----------------- | ------------------------------------------------- | -------------------------- |
| **Status Update** | Brief progress updates during active work         | `## 📊 Status Update`      |
| **DoD Comment**   | REQUIRED when marking issue as Done with evidence | `## ✅ Definition of Done` |
| **Blocker**       | Reporting blocking issues (concise)               | `## 🚫 Blocker`            |

**Removed Comment Types (Use Issue Description Instead):**

- ❌ Context Sharing - Put in issue description
- ❌ Question - Ask in issue description or separate issue
- ❌ Decision - Document in issue description
- ❌ General Comment - Update issue description

### 1. Status Update Comments

**Purpose:** Brief, concise progress updates during active work. Keep it short - 2-3 sentences max.

**Format:**

```markdown
## 📊 Status Update

- ✅ Completed: [Brief what's done]
- 🔄 In Progress: [Brief what's being worked on]
- ⏳ Next: [Brief next step]
```

**Example:**

```markdown
## 📊 Status Update

- ✅ Completed: Validation functions implemented
- 🔄 In Progress: Adding UI error messages
- ⏳ Next: Testing edge cases
```

**Rules:**

- ✅ Keep it concise (2-3 sentences max)
- ✅ Focus on progress, not technical details
- ❌ NO code examples, file paths, or technical specs
- ❌ NO detailed explanations (put in issue description)

### 2. Definition of Done (DoD) Comments

**DoD comments are REQUIRED when marking an issue as "Done".** This is the final comment before closing an issue.

**Pattern:**

```markdown
## ✅ Definition of Done - [Issue Type]

### 1. Implementation Evidence

[Files changed, key changes]

### 2. Test Evidence

[Test results, coverage]

### 3. Deployment Evidence

[Deployment status, environment]

### 4. Verification Evidence

[Screenshots, metrics, links]

---

**Status:** [status]
```

**Complete Template:** See [dod-template.md](./templates/dod-template.md) for detailed requirements and examples.

**DoD Principles:**

1. Evidence-based (file paths, test output, screenshots)
2. Measurable (metrics, coverage, performance)
3. Verifiable (anyone can verify completion)
4. Complete (all sections filled or marked N/A)

### 3. Blocker Comments

**Purpose:** Report blocking issues that prevent progress. Keep it concise.

**Format:**

```markdown
## 🚫 Blocker

**Blocker:** [Brief description]
**Action Needed:** [What needs to happen]
**Owner:** [@mention if applicable]
```

**Example:**

```markdown
## 🚫 Blocker

**Blocker:** Waiting for API key approval
**Action Needed:** Follow up with platform support
**Owner:** @john-doe
```

**Rules:**

- ✅ Keep it brief (1-2 sentences)
- ✅ Focus on what's blocking and what's needed
- ❌ NO detailed technical explanations

### Comment Best Practices

**✅ DO:**

- Keep comments brief and concise (2-3 sentences max for status updates)
- Use status updates to communicate progress during active work
- ALWAYS add DoD comment before marking issue as Done
- Use @mentions for visibility when needed
- Link to PRs when relevant

**❌ DON'T:**

- Put technical specifications in comments (use issue description)
- Add code examples, file paths, or architecture details in comments
- Write long explanations or detailed checklists in comments
- Use comments for questions or decisions (update issue description instead)
- Skip DoD comment when marking Done

**CRITICAL ANTI-PATTERNS:**

- ❌ Adding "Technical Implementation Details" comments
- ❌ Adding "Context" comments with code snippets
- ❌ Adding "Related Issue" reference comments
- ❌ Adding specification details or contract rules in comments
- ❌ Using comments as a substitute for proper issue description

### AI Agent Implementation

**CRITICAL RULES FOR AI AGENTS:**

1. **NEVER create comments with technical details** - All specs go in issue description
2. **ONLY create comments for:**
   - Brief status updates during active work (2-3 sentences max)
   - DoD comment when marking issue as Done
   - Blocker comments (brief, concise)
3. **If you need to add technical details** - Update the issue description, NOT comments

**Creating Status Update Comments:**

```typescript
// ✅ CORRECT: Brief status update
await linear({
  query: `Add status update comment to issue CHA-100: "Completed validation functions. In progress: UI error messages. Next: Testing edge cases."`,
  is_read_only: false,
  summary: "Add status update comment",
});

// ❌ WRONG: Technical details in comment
await linear({
  query: `Add comment with technical implementation details, contract rules, code examples...`,
  // DON'T DO THIS - Update issue description instead
});
```

**Creating DoD Comments:**

```typescript
// ✅ CORRECT: DoD comment when marking Done
await linear({
  query: `Add DoD comment to issue CHA-100 with implementation evidence, test results, and deployment status`,
  is_read_only: false,
  summary: "Add DoD comment",
});
```

### Comment Workflow Summary

**During Development:**

1. **Start work** → Optional: Add brief status update comment
2. **Make progress** → Optional: Add brief status update comments
3. **Hit blockers** → Add brief blocker comment
4. **Need to add technical details** → Update issue description, NOT comments

**Before Marking Done:**

1. **Complete work** → Verify all acceptance criteria met
2. **Run tests** → Ensure all tests pass
3. **Deploy** → Deploy to appropriate environment
4. **Add DoD comment** → REQUIRED with evidence
5. **Update state** → Mark issue as "Done"

---

## Best Practices

### For All Issues

1. **Use templates** - Ensures consistency
2. **Apply correct labels** - Type + Component (+ Domain optional)
3. **Set priority** - Via Linear API, not in description
4. **Link related work** - Use relationships, not text
5. **Keep titles concise** - Scannable at a glance

### For AI agents

1. **Use Linear native fields** - priority, labels, relationships via API
2. **Never use raw text** - Don't write "Priority: High" in description
3. **Follow label algorithm** - Fetch → Match → Collect → Validate
4. **Validate before creating** - Check required fields
5. **Handle errors gracefully** - Label not found, team not found, etc.

### For Features

1. **Define scope clearly** - What's in, what's out
2. **Break down large features** - Use sub-tasks for planned work
3. **Set acceptance criteria** - Define "done"
4. **Plan testing strategy** - Unit, integration, E2E

### For Bugs

1. **Provide reproduction steps** - Minimum 3 steps
2. **Identify root cause** - File, line number, specific issue
3. **Propose fix** - Approach + files to change
4. **Link to parent feature** - If discovered during feature work

### For Improvements

1. **Explain motivation** - Current state, problem, benefit
2. **Define success metrics** - How to measure improvement
3. **Consider breaking changes** - Migration path if needed
4. **Plan regression tests** - Ensure existing functionality works

---

## Quick Reference

### Creating an Issue (AI agent)

```graphql
mutation {
  issueCreate(input: { title: "Bug: Frontend - Login button not responding", description: "...", teamId: "TEAM-ID", priority: 2, labelIds: ["BUG-ID", "FRONTEND-ID", "LOGIC-ERROR-ID"], stateId: "STATE-TODO-ID" }) {
    success
    issue {
      id
      identifier
      url
    }
  }
}
```

---

## Additional Resources

- **[labels.md](./reference/labels.md)** - Complete label schema and selection algorithms
- **[examples.md](./reference/examples.md)** - Real-world scenario examples
- **[common_errors.md](./reference/common_errors.md)** - Common errors and solutions
- **[templates/](./templates/)** - Issue templates

---

**Last Updated:** 2025-10-31  
**Maintained By:** R&D Team
