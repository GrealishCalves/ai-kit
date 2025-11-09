# Linear MCP - Common Errors & Solutions

**Purpose:** Document common errors encountered when using Linear MCP and their solutions  
**Last Updated:** 2025-10-31

---

## Label Groups

### ❌ Error: "parent label is not a group"

**Error Message:**

```json
{
  "message": "parent label is not a group",
  "extensions": {
    "type": "invalid input",
    "code": "INPUT_ERROR",
    "userPresentableMessage": "Please refresh the page and try again."
  }
}
```

**Cause:** Attempting to assign a parent label that was not created with `isGroup: true`

**Solution:** Create the parent label as a group first

```graphql
# ❌ WRONG - Create regular label then try to use as parent
mutation {
  issueLabelCreate(input: { name: "Type", color: "#95a2b3" }) {
    issueLabel {
      id
    }
  }
}

# ✅ CORRECT - Create label as a group
mutation {
  issueLabelCreate(
    input: {
      name: "Type"
      color: "#95a2b3"
      isGroup: true # Must set this!
    }
  ) {
    issueLabel {
      id
      isGroup
    }
  }
}
```

---

### ❌ Error: "parent label team mismatch"

**Error Message:**

```json
{
  "message": "parent label team mismatch",
  "extensions": {
    "type": "invalid input",
    "code": "INPUT_ERROR",
    "userPresentableMessage": "Cannot add a label to a group from a different team."
  }
}
```

**Cause:** Parent label and child label belong to different teams

**Solution:** Ensure both labels are at the same scope (both workspace-level OR both team-specific)

```graphql
# ❌ WRONG - Parent is team-specific, child is workspace-level
mutation CreateGroup {
  issueLabelCreate(
    input: {
      name: "Type"
      isGroup: true
      teamId: "team-uuid" # Team-specific
    }
  ) {
    issueLabel {
      id
    }
  }
}

mutation AddChild {
  issueLabelUpdate(
    id: "workspace-label-uuid" # Workspace-level (team: null)
    input: { parentId: "group-uuid" }
  ) {
    issueLabel {
      id
    }
  }
}

# ✅ CORRECT - Both workspace-level
mutation CreateGroup {
  issueLabelCreate(
    input: {
      name: "Type"
      isGroup: true
      # No teamId = workspace-level
    }
  ) {
    issueLabel {
      id
    }
  }
}

mutation AddChild {
  issueLabelUpdate(id: "workspace-label-uuid", input: { parentId: "group-uuid" }) {
    issueLabel {
      id
    }
  }
}
```

**Rule:** Check label's `team` field before grouping

- `team: null` = Workspace-level
- `team: { id: "..." }` = Team-specific

---

### ❌ Error: "reserved label name"

**Error Message:**

```json
{
  "message": "reserved label name",
  "extensions": {
    "type": "invalid input",
    "code": "INPUT_ERROR",
    "userPresentableMessage": "The label name \"priority\" is reserved."
  }
}
```

**Cause:** Attempting to create a label with a reserved name

**Reserved Names:**

- `priority`
- `status`
- `state`
- `assignee`
- `project`

**Solution:** Use a different name

```graphql
# ❌ WRONG
mutation {
  issueLabelCreate(input: { name: "Priority", isGroup: true }) {
    issueLabel {
      id
    }
  }
}

# ✅ CORRECT
mutation {
  issueLabelCreate(input: { name: "Priority Level", isGroup: true }) {
    issueLabel {
      id
    }
  }
}
```

---

## Issue Relationships

### ❌ Error: "relatedIssueId must be a UUID"

**Error Message:**

```json
{
  "message": "Argument Validation Error",
  "extensions": {
    "validationErrors": [
      {
        "property": "relatedIssueId",
        "constraints": {
          "isUuid": "relatedIssueId must be a UUID"
        }
      }
    ]
  }
}
```

**Cause:** Using issue identifier (e.g., "CHA-73") instead of UUID

**Solution:** Query the issue first to get its UUID

```graphql
# ❌ WRONG - Using identifier
mutation {
  issueRelationCreate(
    input: {
      type: "blocks"
      issueId: "uuid-here"
      relatedIssueId: "CHA-73" # ❌ Identifier won't work
    }
  ) {
    issueRelation {
      id
    }
  }
}

# ✅ CORRECT - Get UUID first, then use it
query GetIssueUUID {
  issue(id: "CHA-73") {
    id # Returns UUID: "76aff500-be96-4ad9-a467-c5391a0e2cc4"
  }
}

mutation CreateRelation {
  issueRelationCreate(
    input: {
      type: "blocks"
      issueId: "b42d0772-f61b-4494-bcb8-77f52b7eb6b7"
      relatedIssueId: "76aff500-be96-4ad9-a467-c5391a0e2cc4" # ✅ UUID
    }
  ) {
    issueRelation {
      id
    }
  }
}
```

**Via Linear MCP Natural Language:**

```
# ❌ WRONG
Create a blocking relationship where CHA-73 blocks CHA-74

# ✅ CORRECT
Get issue CHA-73 with its id field
Get issue CHA-74 with its id field
Create an issue relation where issue [uuid-73] blocks issue [uuid-74] using issueRelationCreate mutation
```

---

## Projects

### ❌ Error: Missing teamIds

**Error Message:**

```json
{
  "message": "Argument Validation Error",
  "extensions": {
    "validationErrors": [
      {
        "property": "teamIds",
        "constraints": {
          "arrayMinSize": "teamIds must contain at least 1 elements"
        }
      }
    ]
  }
}
```

**Cause:** Creating a project without specifying at least one team

**Solution:** Always include `teamIds` array with at least one team

```graphql
# ❌ WRONG
mutation {
  projectCreate(input: { name: "My Project", description: "Project description" }) {
    project {
      id
    }
  }
}

# ✅ CORRECT
mutation {
  projectCreate(
    input: {
      name: "My Project"
      description: "Project description"
      teamIds: ["18615646-967d-4c0f-9e37-5a63857e8964"] # Required!
    }
  ) {
    project {
      id
    }
  }
}
```

---

## Documents

### ❌ Error: Missing projectId

**Error Message:**

```json
{
  "message": "Argument Validation Error",
  "extensions": {
    "validationErrors": [
      {
        "property": "projectId",
        "constraints": {
          "isNotEmpty": "projectId should not be empty"
        }
      }
    ]
  }
}
```

**Cause:** Creating a document without associating it with a project

**Solution:** Documents must belong to a project

```graphql
# ❌ WRONG
mutation {
  documentCreate(input: { title: "My Document", content: "Document content" }) {
    document {
      id
    }
  }
}

# ✅ CORRECT
mutation {
  documentCreate(
    input: {
      title: "My Document"
      content: "Document content"
      projectId: "feb5441a-8504-4fc0-a1b0-ac6146fb8b22" # Required!
    }
  ) {
    document {
      id
    }
  }
}
```

---

## Best Practices

### 1. Always Query First for UUIDs

When working with relationships or updates, always query the entity first to get its UUID:

```graphql
# Step 1: Get UUIDs
query {
  issue(id: "CHA-73") {
    id
  }
  teams {
    nodes {
      id
      name
    }
  }
  issueLabels {
    nodes {
      id
      name
      team {
        id
      }
    }
  }
}

# Step 2: Use UUIDs in mutations
mutation {
  issueRelationCreate(input: { issueId: "uuid-from-step-1", relatedIssueId: "uuid-from-step-1" }) {
    issueRelation {
      id
    }
  }
}
```

### 2. Check Label Scope Before Grouping

```graphql
query CheckLabelScope {
  issueLabels {
    nodes {
      id
      name
      team {
        id
        name
      }
      # team: null = workspace-level
      # team: {...} = team-specific
    }
  }
}
```

### 3. Create Groups Before Children

```graphql
# Step 1: Create group
mutation {
  issueLabelCreate(input: { name: "Type", isGroup: true }) {
    issueLabel {
      id
    }
  }
}

# Step 2: Add children
mutation {
  issueLabelUpdate(id: "child-label-uuid", input: { parentId: "group-uuid-from-step-1" }) {
    issueLabel {
      id
      parent {
        name
      }
    }
  }
}
```

### 4. Verify Team Membership

Before creating projects or team-specific labels, verify team IDs:

```graphql
query GetTeams {
  teams {
    nodes {
      id
      name
      key
    }
  }
}
```

---

## Error Handling Checklist

Before making a mutation, verify:

- [ ] **UUIDs** - Using UUIDs, not identifiers (CHA-73)
- [ ] **Required Fields** - All required fields present (teamIds, projectId, etc.)
- [ ] **Team Scope** - Labels are at same scope (workspace or team)
- [ ] **Group Flag** - Parent labels have `isGroup: true`
- [ ] **Reserved Names** - Not using reserved label names
- [ ] **Relationships** - Both issues exist and are accessible

---

**End of Common Errors Guide**
