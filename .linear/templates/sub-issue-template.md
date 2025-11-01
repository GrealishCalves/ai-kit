# Sub-Issue: [Brief Description]

---

## 🤖 AI agent Instructions

### Title Format

- **Format:** `[Brief Description]`
- **Example:** `Add OAuth provider configuration`
- **Rule:** No prefix needed (inherits type from parent)

### Required Fields

1. **Overview:** Brief description of this sub-task
2. **Parent Context:** Reference to parent issue
3. **Scope:** What this sub-task covers
4. **Technical Approach:** Specific implementation details
5. **Acceptance Criteria:** Measurable success criteria for this sub-task
6. **Testing Strategy:** Unit + Integration tests specific to this sub-task

### Linear Native Fields (Use API, NOT Description Text)

- **Parent ID:** Use `parentId` field when creating sub-issue (REQUIRED)
- **Priority:** Inherits from parent (optional override)
- **Labels:** Inherits type from parent, specify component labels
- **Assignee:** Use `assigneeId` field, NOT text in description

### Label Selection Algorithm

1. Type: Inherits from parent (Feature/Bug/Improvement)
2. Component: At least ONE from `frontend`, `backend`, `database`, `infrastructure`, `mobile`, `api`, `other`
3. Domain: Optional, inherits from parent or specify if different

### When to Use Sub-Issues

✅ **Use sub-issues for:**
- Planned work breakdown known at planning time
- Tasks that are part of the same feature/epic
- Work that must complete before parent can be marked "Done"

❌ **Don't use sub-issues for:**
- Bugs discovered during implementation (use `related` instead)
- Independent work that can be done separately
- Work discovered after parent issue is created

### Validation Checklist

- [ ] Title is clear and concise
- [ ] Parent issue is specified via `parentId`
- [ ] Overview explains what this sub-task accomplishes
- [ ] Scope is clearly defined
- [ ] Technical approach is specific to this sub-task
- [ ] Acceptance criteria are measurable
- [ ] Testing strategy is defined
- [ ] Component labels are applied via API

---

## Overview

Brief summary of what this sub-task accomplishes and how it contributes to the parent issue.

**Example:**

> Configure OAuth provider credentials for Google, GitHub, and Microsoft to enable OAuth authentication flow in the main application.

---

## Parent Context

**Parent Issue:** [Parent Issue ID and Title]

**Example:**

> **Parent:** ENG-123 - User Authentication - Add OAuth login support
>
> This sub-task handles the OAuth provider configuration, which is the first step in implementing OAuth authentication. The parent issue cannot be completed until all OAuth providers are properly configured.

---

## Scope

What this sub-task covers:

- [ ] Item 1
- [ ] Item 2
- [ ] Item 3

**Example:**

- [ ] Register OAuth applications with Google, GitHub, Microsoft
- [ ] Configure OAuth callback URLs for each provider
- [ ] Store OAuth client IDs and secrets securely
- [ ] Add OAuth provider configuration to environment variables
- [ ] Document OAuth setup process

---

## Technical Approach

### Components Affected

Choose at least ONE:

- `frontend` - Web app, mobile app, UI components
- `backend` - API, services, workers
- `database` - Schema, queries, migrations
- `infrastructure` - CI/CD, monitoring, hosting
- `mobile` - iOS, Android apps
- `api` - REST, GraphQL, webhooks
- `other` - Specify in description

**Example:**

- `backend` - OAuth provider configuration
- `infrastructure` - Environment variable management

### Key Changes

List the specific changes for this sub-task.

**Example:**

- Register OAuth apps with Google Cloud Console, GitHub Developer Settings, Microsoft Azure
- Configure callback URLs: `https://app.example.com/auth/oauth/callback`
- Add environment variables: `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, etc.
- Create OAuth configuration module in backend
- Add OAuth provider validation logic

### Dependencies

Any dependencies specific to this sub-task.

**Example:**

- Access to Google Cloud Console, GitHub Developer Settings, Microsoft Azure
- SSL certificate for callback URLs
- Environment variable management system (e.g., Vault, AWS Secrets Manager)

---

## Acceptance Criteria

Define what "done" means for this sub-task with measurable criteria.

**Example:**

- [ ] OAuth applications registered with all 3 providers (Google, GitHub, Microsoft)
- [ ] Callback URLs configured correctly for each provider
- [ ] Client IDs and secrets stored securely in environment variables
- [ ] OAuth configuration module implemented and tested
- [ ] Unit tests pass (>90% coverage)
- [ ] Documentation updated with OAuth setup instructions

---

## Testing Strategy

### Unit Tests

Describe unit test coverage specific to this sub-task.

**Example:**

- OAuth provider configuration validation
- Client ID and secret parsing
- Callback URL validation
- Environment variable loading

### Integration Tests

Describe integration test coverage specific to this sub-task.

**Example:**

- OAuth configuration module integration with auth service
- Environment variable loading from secrets manager
- OAuth provider validation with real credentials (staging)

### Manual Verification

Any manual checks needed for this sub-task.

**Example:**

- Verify OAuth apps are registered correctly in provider consoles
- Test callback URLs are accessible and return 200 OK
- Verify environment variables are loaded correctly in staging

---

## 📋 Linear Native Fields (Set via API)

**AI agent:** Use Linear GraphQL API to set these fields, NOT text in description.

### Parent ID (REQUIRED)

```graphql
mutation {
  issueCreate(input: {
    title: "Add OAuth provider configuration"
    parentId: "PARENT-ISSUE-UUID"  # REQUIRED for sub-issues
    teamId: "TEAM-ID"
    labelIds: ["FEATURE-ID", "BACKEND-ID"]
  }) {
    issue { id }
  }
}
```

### Labels

```graphql
mutation {
  issueCreate(input: {
    labelIds: ["FEATURE-ID", "BACKEND-ID", "INFRASTRUCTURE-ID"]
    ...
  })
}
```

### Assignee

```graphql
mutation {
  issueCreate(input: {
    assigneeId: "USER-ID"
    ...
  })
}
```

---

## Related Documentation

- [AI agent Guide](../guide.md) - GraphQL examples and algorithms
- [Label Schema](../labels.md) - Label selection guidance
- [Workflow Guide](../guide.md) - Complete workflow documentation
- [Parent Template](./feature-template.md) - Feature template for parent issues

