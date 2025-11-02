# Feature: [Scope] - [Brief Description]

---

## 🤖 AI agent Instructions

### Title Format

- **Format:** `[Scope] - [Brief Description]`
- **Example:** `User Authentication - Add OAuth login support`
- **Rule:** No "Feature:" prefix needed (type is set via label)

### Required Fields

1. **Overview:** Brief description of what and why
2. **Scope → In Scope:** Items included in this feature
3. **Scope → Out of Scope:** Items explicitly excluded
4. **Technical Approach:** Components + key changes + dependencies
5. **Acceptance Criteria:** Measurable success criteria
6. **Testing Strategy:** Unit + Integration + E2E coverage

### Linear Native Fields (Use API, NOT Description Text)

- **Priority:** Use `priority` field (0-4), NOT text in description
  - Urgent = `priority: 1`
  - High = `priority: 2`
  - Medium = `priority: 3`
  - Low = `priority: 4`
- **Labels:** Use `labelIds` array, NOT text in description
- **Sub-tasks:** Use `parentId` field when creating sub-task issues, NOT text in description
- **Related Issues:** Use `issueRelationCreate` mutation, NOT text in description

### Label Selection Algorithm

1. Type: `Feature` (required)
2. Component: At least ONE from:
   - **Core:** `frontend`, `backend`, `contract`
   - **Additional:** `database`, `infrastructure`, `api`, `documentation`, `testing`, `security`, `performance`, `other`
3. Domain: Optional, project-specific (e.g., `user-management`, `payments`, `analytics`)

### Sub-tasks vs Related Issues

- **Sub-tasks:** Planned work breakdown known at planning time (use `parentId`)
- **Related Issues:** Bugs discovered during implementation (use `issueRelationCreate` with `type: related`)

### Validation Checklist

- [ ] Title follows `[Scope] - [Description]` format
- [ ] Overview is clear and concise
- [ ] In-scope items are specified
- [ ] Out-of-scope items are specified
- [ ] Technical approach includes components, changes, and dependencies
- [ ] Acceptance criteria are measurable
- [ ] Testing strategy covers unit, integration, and E2E
- [ ] Priority set via API (not in description)
- [ ] Labels applied via API (not in description)
- [ ] Sub-tasks created via API with `parentId` (not in description)

---

## Overview

Brief summary of what this feature accomplishes and why it's needed.

**Example:**

> Add OAuth login support to allow users to authenticate using Google, GitHub, and Microsoft accounts, reducing friction during signup and improving security.

---

## Scope

### In Scope

What IS included in this feature:

- [ ] Item 1
- [ ] Item 2
- [ ] Item 3

**Example:**

- [ ] OAuth integration with Google, GitHub, Microsoft
- [ ] User profile creation from OAuth data
- [ ] Existing user account linking
- [ ] OAuth token refresh mechanism

### Out of Scope

What is NOT included (to be addressed separately):

- Item 1
- Item 2

**Example:**

- Social login for Facebook, Twitter (future iteration)
- Two-factor authentication (separate feature)
- Password reset flow changes (not affected)

---

## Technical Approach

Brief description of the implementation strategy.

### Components Affected

Choose at least ONE:

**Core Components:**
- `frontend` - Web app, UI components, client-side logic
- `backend` - API, services, workers, server-side logic
- `contract` - Smart contracts, blockchain interactions

**Additional Components:**
- `database` - Schema, queries, migrations
- `infrastructure` - CI/CD, monitoring, hosting
- `api` - REST, GraphQL, webhooks
- `documentation` - README, API docs, guides
- `testing` - Test frameworks, CI tests
- `security` - Auth, permissions, encryption
- `performance` - Optimization, caching
- `other` - Specify in description

**Example:**

- `frontend` - Login UI, OAuth callback handling
- `backend` - OAuth provider integration, token management
- `database` - User schema updates for OAuth fields

**Example (Web3):**

- `frontend` - Wallet connection UI
- `contract` - Token contract deployment
- `backend` - Blockchain event indexing

### Key Changes

List the main technical changes required.

**Example:**

- Add OAuth provider configuration (Google, GitHub, Microsoft)
- Implement OAuth callback endpoints
- Update user schema to store OAuth provider and external ID
- Add OAuth token storage and refresh logic
- Update login UI to show OAuth buttons

### Dependencies

Any external dependencies or blockers.

**Example:**

- OAuth app registration with Google, GitHub, Microsoft
- SSL certificate for OAuth callback URLs
- User schema migration approval
- Security review for token storage

---

## Sub-tasks (Optional)

If this feature requires multiple implementation steps, consider breaking it down into sub-tasks.

**AI agent:** Create sub-task issues with `parentId` set to this feature's ID.

### When to Use Sub-tasks

- **Use sub-tasks for:** Planned work breakdown known at planning time
- **Don't use sub-tasks for:** Bugs discovered during implementation (use `related` instead)

### Sub-task Guidelines

- Each sub-task should have its own issue with full acceptance criteria and testing strategy
- Use same labels as parent (`Feature`, component, domain)
- Set `parentId` when creating sub-task issues
- Sub-tasks can be worked on independently or in parallel

### Example Sub-task Breakdown

```graphql
# Parent Feature
mutation {
  issueCreate(input: { title: "User Authentication - Add OAuth login support", teamId: "TEAM-ID", labelIds: ["FEATURE-ID", "FRONTEND-ID", "BACKEND-ID", "USER-MANAGEMENT-ID"] }) {
    issue {
      id
    }
  }
}

# Sub-task 1
mutation {
  issueCreate(input: { title: "Add OAuth provider configuration", parentId: "PARENT-FEATURE-ID", teamId: "TEAM-ID", labelIds: ["FEATURE-ID", "BACKEND-ID"] })
}

# Sub-task 2
mutation {
  issueCreate(input: { title: "Implement OAuth callback endpoints", parentId: "PARENT-FEATURE-ID", teamId: "TEAM-ID", labelIds: ["FEATURE-ID", "BACKEND-ID"] })
}

# Sub-task 3
mutation {
  issueCreate(input: { title: "Update login UI with OAuth buttons", parentId: "PARENT-FEATURE-ID", teamId: "TEAM-ID", labelIds: ["FEATURE-ID", "FRONTEND-ID"] })
}
```

**Example Hierarchy:**

```
Parent: ENG-123 - User Authentication - Add OAuth login support
├── ENG-124: Add OAuth provider configuration
├── ENG-125: Implement OAuth callback endpoints
└── ENG-126: Update login UI with OAuth buttons
```

---

## Acceptance Criteria

Define what "done" means for this feature with measurable criteria.

**Example:**

- [ ] Users can log in using Google, GitHub, or Microsoft accounts
- [ ] OAuth tokens are securely stored and refreshed automatically
- [ ] Existing users can link OAuth accounts to their profiles
- [ ] All unit tests pass (>90% coverage)
- [ ] All integration tests pass
- [ ] E2E tests cover OAuth login flow
- [ ] Documentation updated with OAuth setup instructions
- [ ] Security review completed and approved

---

## Testing Strategy

### Unit Tests

Describe unit test coverage.

**Example:**

- OAuth provider configuration validation
- Token parsing and validation logic
- User profile creation from OAuth data
- Account linking logic

### Integration Tests

Describe integration test coverage.

**Example:**

- OAuth callback endpoint integration
- Token refresh mechanism
- User authentication flow with OAuth
- Account linking with existing users

### E2E Tests

Describe E2E test coverage.

**Example:**

- Complete OAuth login flow (Google, GitHub, Microsoft)
- OAuth account linking for existing users
- OAuth token refresh on expiration
- Error handling for OAuth failures

### Manual Testing

Any manual verification needed.

**Example:**

- Test OAuth login on staging environment
- Verify OAuth provider consent screens
- Test account linking with production-like data
- Verify token refresh behavior

---

## Deployment Notes

### Environment

- **Local:** Development environment
- **Staging:** Staging/test environment
- **Production:** Live production environment

### Migration Required

- **Yes:** [Describe migration steps]
- **No**

**Example (if Yes):**

> Database migration required to add OAuth fields to user schema. Run migration script before deploying backend changes.

### Rollback Plan

How to rollback if issues arise.

**Example:**

> 1. Revert backend deployment to previous version
> 2. Rollback database migration if needed
> 3. Remove OAuth buttons from frontend
> 4. Monitor error logs for any OAuth-related issues

---

## 📋 Linear Native Fields (Set via API)

**AI agent:** Use Linear GraphQL API to set these fields, NOT text in description.

### Priority

```graphql
mutation {
  issueCreate(input: {
    priority: 2  # High priority
    ...
  })
}
```

### Labels

```graphql
mutation {
  issueCreate(input: {
    labelIds: ["FEATURE-ID", "FRONTEND-ID", "BACKEND-ID", "USER-MANAGEMENT-ID"]
    ...
  })
}
```

### Sub-tasks

```graphql
mutation {
  issueCreate(input: { title: "Sub-task title", parentId: "PARENT-FEATURE-ID", teamId: "TEAM-ID", labelIds: ["FEATURE-ID", "COMPONENT-ID"] })
}
```

### Related Issues

```graphql
mutation {
  issueRelationCreate(input: { issueId: "CURRENT-FEATURE-ID", relatedIssueId: "RELATED-ISSUE-ID", type: related })
}
```

---

## Related Documentation

- [AI agent Guide](../guide.md) - GraphQL examples and algorithms
- [Label Schema](../labels.md) - Label selection guidance
- [Workflow Guide](../guide.md) - Complete workflow documentation
