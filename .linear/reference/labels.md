# Linear Labels Reference

Complete label schema for Linear workflow - domain-agnostic and customizable.

---

## Label Categories

Linear issues use 4 label categories:

1. **Type Labels** (Required - ONE)
2. **Component Labels** (Required - AT LEAST ONE)
3. **Domain Labels** (Optional - Project-specific)
4. **Bug Category Labels** (Required for Bugs - ONE)

---

## 1. Type Labels (Required)

Every issue MUST have exactly ONE type label.

| Label         | Description          | Use When                            |
| ------------- | -------------------- | ----------------------------------- |
| `Feature`     | New functionality    | Adding something that doesn't exist |
| `Bug`         | Broken functionality | Something doesn't work as expected  |
| `Improvement` | Enhancement          | Making something better             |

**Examples:**

- ✅ `Feature` - Add user authentication
- ✅ `Bug` - Login button not responding
- ✅ `Improvement` - Optimize database query performance

---

## 2. Component Labels (Required)

Every issue MUST have AT LEAST ONE component label. Multiple components allowed.

| Label            | Description             | Examples                           |
| ---------------- | ----------------------- | ---------------------------------- |
| `frontend`       | User-facing interface   | Web app, mobile app, UI components |
| `backend`        | Server-side logic       | API, services, workers             |
| `database`       | Data storage layer      | Schema, queries, migrations        |
| `infrastructure` | Deployment & operations | CI/CD, monitoring, hosting         |
| `mobile`         | Mobile-specific         | iOS, Android, React Native         |
| `api`            | API layer               | REST, GraphQL, webhooks            |
| `documentation`  | Documentation           | README, API docs, guides           |
| `testing`        | Testing infrastructure  | Test frameworks, CI tests          |
| `security`       | Security-related        | Auth, permissions, encryption      |
| `performance`    | Performance-related     | Optimization, caching              |
| `other`          | Other components        | Specify in description             |

**Examples:**

- ✅ `frontend` - React component rendering issue
- ✅ `backend` + `database` - API endpoint with slow query
- ✅ `infrastructure` - CI/CD pipeline failing

---

## 3. Domain Labels (Optional)

Domain labels are **project-specific**. Customize for your project.

### Generic Examples

| Label             | Description                   | Applicable To              |
| ----------------- | ----------------------------- | -------------------------- |
| `user-management` | User accounts, profiles, auth | Most applications          |
| `payments`        | Payment processing, billing   | E-commerce, SaaS, fintech  |
| `analytics`       | Data analysis, reporting      | Most applications          |
| `notifications`   | Email, push, SMS              | Most applications          |
| `auth`            | Authentication, authorization | Most applications          |
| `data-processing` | ETL, batch jobs, pipelines    | Data-heavy applications    |
| `search`          | Search functionality          | Content-heavy applications |
| `messaging`       | Chat, real-time communication | Social, collaboration apps |
| `media`           | Images, videos, file uploads  | Content platforms          |
| `integrations`    | Third-party integrations      | Most applications          |
| `admin`           | Admin panel, internal tools   | Most applications          |
| `onboarding`      | User onboarding, tutorials    | Most applications          |
| `settings`        | User settings, preferences    | Most applications          |

### Customization Examples

Replace generic labels with your project's domains:

- **E-commerce:** `cart`, `checkout`, `inventory`, `shipping`, `returns`
- **Social Media:** `feed`, `posts`, `comments`, `likes`, `followers`
- **SaaS:** `workspace`, `collaboration`, `billing`, `subscription`
- **Fintech:** `transactions`, `accounts`, `transfers`, `compliance`, `kyc`

---

## 4. Bug Category Labels (For Bugs Only)

If issue type is `Bug`, you MUST add ONE bug category label.

| Label              | Description                    | Examples                              |
| ------------------ | ------------------------------ | ------------------------------------- |
| `logic-error`      | Incorrect business logic       | Wrong calculation, invalid validation |
| `display-error`    | UI rendering issues            | Layout broken, text overflow          |
| `query-error`      | Data fetching issues           | API errors, slow queries, timeouts    |
| `state-sync`       | State synchronization          | Cache inconsistency, stale data       |
| `id-collision`     | ID or key conflicts            | Duplicate IDs, key collisions         |
| `event-handling`   | Event processing issues        | Missing events, wrong handlers        |
| `data-incorrect`   | Data accuracy issues           | Wrong values, data corruption         |
| `data-missing`     | Missing data or fields         | Null values, missing properties       |
| `data-duplication` | Duplicate data                 | Duplicate records, redundant data     |
| `performance`      | Performance issues             | Slow response, high memory usage      |
| `security`         | Security vulnerabilities       | XSS, SQL injection, auth bypass       |
| `integration`      | Third-party integration issues | API failures, webhook errors          |
| `configuration`    | Configuration issues           | Wrong env vars, missing config        |
| `deployment`       | Deployment issues              | Build failures, deployment errors     |
| `other`            | Other bug categories           | Specify in description                |

**Examples:**

- ✅ `logic-error` - User can submit form with invalid email
- ✅ `display-error` - Button text overflows on mobile
- ✅ `query-error` - API returns 500 error on user search
- ✅ `performance` - Dashboard takes 10+ seconds to load

---

## Label Selection Rules

### Required Validations

1. **Type label:** Exactly ONE type label must be present
2. **Component label:** At least ONE component label must be present
3. **Bug category:** If type is `Bug`, exactly ONE bug category label must be present

### Optional Validations

1. **Domain label:** Zero or more domain labels can be present
2. **Multiple components:** Multiple component labels are allowed

---

## AI Agent Label Selection

### Label Selection Process

AI agents should follow this process when selecting labels for Linear issues:

**Step 1: Fetch All Labels**

- Use Linear API to retrieve all labels for the team
- Store labels for matching in subsequent steps

**Step 2: Find Type Label (Required)**

- Match input type (Feature/Bug/Improvement) against label names
- Use case-insensitive matching
- **Error if not found:** Throw error with message "Type label '[type]' not found"

**Step 3: Find Component Labels (Required - At Least One)**

- Match each input component against label names
- Use case-insensitive matching
- Filter out any non-matches
- **Error if none found:** Throw error with message "No component labels found for: [components]"

**Step 4: Find Domain Label (Optional)**

- If domain provided, match against label names using case-insensitive matching
- If not found, skip (domain is optional)

**Step 5: Find Bug Category Label (Required for Bugs Only)**

- If issue type is "Bug":
  - **Error if category missing:** Throw error "Bug category is required for Bug issues"
  - Match bug category against label names using case-insensitive matching
  - **Error if not found:** Throw error "Bug category label '[category]' not found"
- If issue type is not "Bug", skip this step

**Step 6: Collect All Label IDs**

- Start with type label ID
- Add all component label IDs
- Add domain label ID (if found)
- Add bug category label ID (if applicable)
- Return array of label IDs

### Example Results

**Feature Issue:**

- Input: type="Feature", components=["frontend", "backend"], domain="user-management"
- Output: ["Feature-ID", "frontend-ID", "backend-ID", "user-management-ID"]

**Bug Issue:**

- Input: type="Bug", components=["frontend"], domain="auth", bugCategory="logic-error"
- Output: ["Bug-ID", "frontend-ID", "auth-ID", "logic-error-ID"]

---

## Automatic Label Inference

AI agents can infer labels from issue descriptions using keyword matching.

### Component Inference Guidelines

**Process:**

1. Convert issue description to lowercase
2. Check for component-specific keywords
3. Add matching components to list
4. If no matches found, default to "other"

**Keyword Mapping:**

- **frontend:** ui, button, form, page, component, react, vue, angular, svelte
- **backend:** api, server, endpoint, service, worker, controller, middleware
- **database:** query, schema, migration, table, sql, postgres, mongodb
- **mobile:** ios, android, mobile, app, native, react-native
- **infrastructure:** deploy, ci/cd, docker, kubernetes, aws, hosting, devops
- **api:** rest, graphql, webhook, endpoint, request, response
- **performance:** slow, performance, memory, cpu, optimization, cache

**Example:**

- Description: "Login button not responding on mobile"
- Keywords found: "button" (frontend), "mobile" (mobile)
- Inferred components: ["frontend", "mobile"]

### Bug Category Inference Guidelines

**Process:**

1. Convert issue description to lowercase
2. Check for bug category-specific keywords
3. Return first matching category
4. If no matches found, default to "other"

**Keyword Mapping:**

- **logic-error:** validation, calculation, incorrect, wrong logic, invalid
- **display-error:** layout, rendering, ui broken, overflow, alignment, css
- **query-error:** api error, timeout, 500 error, slow query, database error
- **performance:** slow, performance, memory, cpu, lag, freeze
- **security:** xss, injection, auth bypass, vulnerability, exploit
- **state-sync:** cache, stale data, inconsistent, out of sync
- **integration:** third-party, webhook, external api, integration failure

**Example:**

- Description: "API returns 500 error on user search"
- Keywords found: "api error", "500 error"
- Inferred category: "query-error"

---

## Label Validation Rules

AI agents must validate labels before creating Linear issues.

### Validation Process

**Validation 1: Type Label Count**

- Filter labels to find type labels (Feature, Bug, Improvement)
- **Rule:** Exactly 1 type label must be present
- **Error if violated:** "Expected exactly 1 type label, found [count]"

**Validation 2: Component Label Count**

- Filter labels to find component labels (frontend, backend, database, infrastructure, mobile, api, other)
- **Rule:** At least 1 component label must be present
- **Error if violated:** "At least 1 component label is required"

**Validation 3: Bug Category (Conditional)**

- If issue type is "Bug":
  - Filter labels to find bug category labels (logic-error, display-error, query-error, performance, security, etc.)
  - **Rule:** Exactly 1 bug category label must be present
  - **Error if violated:** "Bug issues require exactly 1 bug category label, found [count]"
- If issue type is not "Bug", skip this validation

**Validation 4: Domain Labels (Optional)**

- Domain labels are optional
- Multiple domain labels are allowed
- No validation required

### Validation Examples

**Valid Feature:**

- Labels: ["Feature", "frontend", "backend", "user-management"]
- ✅ Passes: 1 type, 2 components, 1 domain (optional)

**Invalid Feature:**

- Labels: ["Feature", "Bug", "frontend"]
- ❌ Fails: 2 type labels (expected 1)

**Valid Bug:**

- Labels: ["Bug", "frontend", "logic-error"]
- ✅ Passes: 1 type, 1 component, 1 bug category

**Invalid Bug:**

- Labels: ["Bug", "frontend"]
- ❌ Fails: Missing bug category label

---

## Quick Reference

### Label Hierarchy

```
Issue
├── Type (required, ONE)
│   ├── Feature
│   ├── Bug
│   └── Improvement
├── Component (required, AT LEAST ONE)
│   ├── frontend
│   ├── backend
│   ├── database
│   ├── infrastructure
│   ├── mobile
│   ├── api
│   └── other
├── Domain (optional, project-specific)
│   ├── user-management
│   ├── payments
│   ├── analytics
│   └── ...
└── Bug Category (required if Bug, ONE)
    ├── logic-error
    ├── display-error
    ├── query-error
    ├── performance
    └── ...
```

---

## Label Groups

Label groups allow you to organize related labels into hierarchical structures. This is useful for creating categorized label systems.

### Overview

**What are Label Groups?**

- Parent labels that contain child labels
- Created with `isGroup: true` property
- Cannot be assigned to issues directly
- Used for organizational purposes only

**Use Cases:**

- Organize bug categories under "Bug Type" group
- Group component labels under "Component" group
- Create priority hierarchies under "Priority" group
- Organize domain labels by feature area

### Creating Label Groups

**Step 1: Create Parent Group**

Use Linear MCP to create a group label:

```
Query: "Create a label group named 'Bug Type' with description 'Bug categorization'"
Parameters:
- name: "Bug Type"
- description: "Bug categorization"
- isGroup: true
- teamIds: ["team-uuid"]  # Required
```

**Step 2: Add Child Labels**

Add existing labels to the group:

```
Query: "Add label 'logic-error' to parent group 'Bug Type'"
Parameters:
- name: "logic-error"
- parentId: "bug-type-group-uuid"  # UUID of parent group
```

### Important Constraints

**1. Reserved Label Names**

Cannot use these names for groups (Linear reserved):

- "priority"
- "status"
- "estimate"
- "project"
- "cycle"

**Error:** "Label name is reserved"

**2. Team Scope Matching**

Parent and child labels must have matching team scope:

- ✅ Both workspace-level (team: null)
- ✅ Both same team (team: "team-uuid")
- ❌ Parent workspace, child team-specific
- ❌ Parent team-specific, child workspace

**Error:** "Parent label team does not match child label team"

**3. Parent Must Be Group**

Parent label must have `isGroup: true`:

**Error:** "Parent label is not a group"

### Label Group Examples

**Example 1: Bug Type Hierarchy**

```
Bug Type (group)
├── logic-error
├── display-error
├── query-error
├── performance
└── security
```

**Example 2: Component Hierarchy**

```
Component (group)
├── frontend
├── backend
├── database
├── infrastructure
└── mobile
```

**Example 3: Priority Hierarchy**

```
Priority (group)
├── critical
├── high
├── medium
└── low
```

### AI Agent Usage

**Creating Label Groups:**

```typescript
// Step 1: Create parent group
const groupResult = await linear({
  query: "Create label group 'Component' for team CHA",
  is_read_only: false,
  summary: "Create Component label group",
});

// Step 2: Get group UUID from result
const groupId = groupResult.data.labelCreate.label.id;

// Step 3: Add child labels
await linear({
  query: `Add label 'frontend' to parent group ${groupId}`,
  is_read_only: false,
  summary: "Add frontend to Component group",
});
```

**Querying Label Groups:**

```typescript
// Get all label groups
const groups = await linear({
  query: "Get all label groups for team CHA",
  is_read_only: true,
  summary: "Fetch label groups",
});

// Get labels in a specific group
const childLabels = await linear({
  query: "Get all labels in 'Bug Type' group",
  is_read_only: true,
  summary: "Fetch Bug Type child labels",
});
```

### Common Errors & Solutions

**Error 1: "Label name is reserved"**

```
❌ Problem: Trying to create group named "priority"
✅ Solution: Use different name like "Priority Level" or "Task Priority"
```

**Error 2: "Parent label team does not match child label team"**

```
❌ Problem: Parent is workspace-level, child is team-specific
✅ Solution: Create both at same scope (both workspace or both team)
```

**Error 3: "Parent label is not a group"**

```
❌ Problem: Trying to add child to regular label
✅ Solution: Create parent with isGroup: true first
```

### Best Practices

**1. Plan Hierarchy First**

- Design label structure before creating groups
- Decide on workspace vs team scope upfront
- Avoid deep nesting (max 2 levels recommended)

**2. Use Descriptive Names**

- ✅ "Bug Type" instead of "Bugs"
- ✅ "Component Area" instead of "Components"
- ✅ "Priority Level" instead of "Priority"

**3. Consistent Scope**

- Keep all related labels at same scope (workspace or team)
- Use workspace-level for organization-wide labels
- Use team-level for team-specific labels

**4. Avoid Over-Organization**

- Don't create groups for 2-3 labels
- Groups are useful for 5+ related labels
- Keep structure simple and flat when possible

---

**Last Updated:** 2025-10-31
**Maintained By:** R&D Team
