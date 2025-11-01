# Bug: [Component] - [Brief Description]

---

## 🤖 AI agent Instructions

### Title Format

- **Format:** `Bug: [Component] - [Brief Description]`
- **Example:** `Bug: Frontend - Login button not responding on mobile`
- **Rule:** Must follow format exactly

### Required Fields

1. **Summary:** Clear, concise description of the bug
2. **Impact → Severity:** Choose severity level (Critical/High/Medium/Low)
3. **Impact → Affected Users:** Specific user group (e.g., "All mobile users on iOS 15+")
4. **Steps to Reproduce:** Numbered steps to reproduce the issue
5. **Root Cause:** File path, line number, and specific issue
6. **Proposed Fix:** Approach + files to change + tests to add

### Linear Native Fields (Use API, NOT Description Text)

- **Priority:** Use `priority` field (0-4), NOT text in description
  - Critical = `priority: 1`
  - High = `priority: 2`
  - Medium = `priority: 3`
  - Low = `priority: 4`
- **Labels:** Use `labelIds` array, NOT text in description
- **Related Issues:** Use `issueRelationCreate` mutation, NOT text in description
- **Assignee:** Use `assigneeId` field, NOT text in description

### Label Selection Algorithm

1. Type: `Bug` (required)
2. Component: At least ONE from `frontend`, `backend`, `database`, `infrastructure`, `mobile`, `api`, `other`
3. Bug Category: ONE from `logic-error`, `display-error`, `query-error`, `state-sync`, `performance`, `security`, `other`
4. Domain: Optional, project-specific (e.g., `user-management`, `payments`, `analytics`)

### Validation Checklist

- [ ] Title follows `Bug: [Component] - [Description]` format
- [ ] Summary is clear and concise
- [ ] Severity is specified (Critical/High/Medium/Low)
- [ ] Steps to reproduce are provided
- [ ] Root cause includes file path and line number
- [ ] Proposed fix includes specific files and tests
- [ ] Priority set via API (not in description)
- [ ] Labels applied via API (not in description)

---

## Summary

Clear, concise description of the bug.

**Example:**

> Login button does not respond to clicks on mobile devices, preventing users from accessing their accounts.

---

## Impact

### Severity

Choose severity level:

- **Critical:** System down, data loss, security breach
- **High:** Major feature broken, affects many users
- **Medium:** Feature degraded, workaround available
- **Low:** Minor issue, cosmetic problem

### Affected Users

Be specific about who is impacted (e.g., "All mobile users on iOS 15+", "Users with admin permissions").

### Workaround

Describe if a workaround is available, or state "No workaround available".

---

## Steps to Reproduce

Provide numbered steps that anyone can follow to reproduce the bug.

**Example:**

1. Open app on iOS 15+ device
2. Navigate to login screen
3. Tap login button
4. **Expected:** Login form submits
5. **Actual:** Button does not respond

---

## Expected Behavior

What should happen when following the steps above?

**Example:**

- Login button should respond to tap
- Login form should submit credentials
- User should be redirected to dashboard

---

## Actual Behavior

What actually happens when following the steps above?

**Example:**

- Login button does not respond to tap
- No visual feedback on button press
- User remains on login screen

---

## Root Cause

Technical explanation of why this bug occurs.

### Required Information

- **File(s) Affected:** Full path to file(s)
- **Line(s):** Specific line numbers
- **Issue:** Exact code issue

**Example:**

- **File:** `src/components/LoginButton.tsx`
- **Line:** 42
- **Issue:** Missing `onClick` handler for mobile touch events. Component only handles mouse events, not touch events.

---

## Proposed Fix

How to fix this bug.

### Approach

Brief description of the fix strategy.

**Example:**

> Add touch event handler to LoginButton component to support mobile devices.

### Files to Change

List all files that need to be modified.

**Example:**

- `src/components/LoginButton.tsx` - Add `onTouchStart` handler
- `src/components/LoginButton.test.tsx` - Add test for touch events

### Tests to Add/Update

List all tests that need to be added or updated.

**Example:**

- Add unit test: "should respond to touch events on mobile"
- Update integration test: "should submit login form on mobile tap"

---

## Verification

How to verify the fix works.

**Checklist:**

- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] E2E tests pass (if applicable)
- [ ] Manual verification on affected platform
- [ ] No regressions in related functionality

**Manual Verification Steps:**

1. Deploy fix to staging
2. Test on iOS 15+ device
3. Verify login button responds to tap
4. Verify login form submits correctly

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
    labelIds: ["BUG-ID", "FRONTEND-ID", "LOGIC-ERROR-ID"]
    ...
  })
}
```

### Related Issues

```graphql
mutation {
  issueRelationCreate(input: { issueId: "CURRENT-BUG-ID", relatedIssueId: "PARENT-FEATURE-ID", type: related })
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

- [AI agent Guide](../ guide.md) - GraphQL examples and algorithms
- [Label Schema](../labels.md) - Label selection guidance
- [Workflow Guide](../guide.md) - Complete workflow documentation
