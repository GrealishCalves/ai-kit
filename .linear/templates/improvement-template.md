# Improvement: [Component] - [Brief Description]

---

## 🤖 AI agent Instructions

### Title Format

- **Format:** `[Component] - [Brief Description]`
- **Example:** `Database - Optimize user query performance`
- **Rule:** No "Improvement:" prefix needed (type is set via label)

### Required Fields

1. **Overview:** Brief description of what improves
2. **Motivation:** Current state + problem + benefit
3. **Scope:** Items included in this improvement
4. **Technical Approach:** Components + key changes + breaking changes
5. **Success Metrics:** Measurable metrics
6. **Testing Strategy:** Regression + performance + manual verification

### Linear Native Fields (Use API, NOT Description Text)

- **Priority:** Use `priority` field (0-4), NOT text in description
  - Urgent = `priority: 1`
  - High = `priority: 2`
  - Medium = `priority: 3`
  - Low = `priority: 4`
- **Labels:** Use `labelIds` array, NOT text in description
- **Related Issues:** Use `issueRelationCreate` mutation, NOT text in description

### Label Selection Algorithm

1. Type: `Improvement` (required)
2. Component: At least ONE from `frontend`, `backend`, `database`, `infrastructure`, `mobile`, `api`, `other`
3. Domain: Optional, project-specific (e.g., `user-management`, `payments`, `analytics`)

### Validation Checklist

- [ ] Title follows `[Component] - [Description]` format
- [ ] Overview is clear and concise
- [ ] Motivation includes current state, problem, and benefit
- [ ] Scope items are specified
- [ ] Technical approach includes components, changes, and breaking changes status
- [ ] Success metrics are measurable
- [ ] Testing strategy covers regression, performance, and manual verification
- [ ] Priority set via API (not in description)
- [ ] Labels applied via API (not in description)

---

## Overview

Brief summary of what this improvement accomplishes.

**Example:**

> Optimize database queries for user search to reduce response time from 2 seconds to under 200ms, improving user experience and reducing server load.

---

## Motivation

Describe the current state, the problem, and the expected benefit.

**Example:**

**Current State:** User search queries currently use full table scans without indexes, resulting in slow response times (2-3 seconds) when searching across 100k+ users.

**Problem:**

- Slow search response times frustrate users
- High database CPU usage during peak hours
- No pagination support leads to memory issues
- Search results are not ranked by relevance

**Benefit:**

- Search response time reduced to <200ms
- Database CPU usage reduced by 60%
- Pagination support for large result sets
- Relevance-ranked search results

---

## Scope

What is included in this improvement:

- [ ] Item 1
- [ ] Item 2
- [ ] Item 3

**Example:**

- [ ] Add database indexes on user search fields (name, email, username)
- [ ] Implement full-text search using database native capabilities
- [ ] Add pagination support (limit/offset)
- [ ] Add relevance ranking algorithm
- [ ] Update API endpoint to support new query parameters

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

- `database` - Add indexes, full-text search
- `backend` - Update search API endpoint
- `api` - New query parameters for pagination and ranking

### Key Changes

List the main technical changes required.

**Example:**

- Add GIN index on `users.name`, `users.email`, `users.username` fields
- Implement full-text search using PostgreSQL `tsvector` and `tsquery`
- Add pagination parameters (`limit`, `offset`) to search API
- Implement relevance ranking using `ts_rank` function
- Update API response to include total count and pagination metadata

### Breaking Changes

- **Yes:** [Describe breaking changes and migration path]
- **No**

**Example (if Yes):**

> API response structure changes to include pagination metadata. Clients must update to handle new response format:
>
> ```json
> {
>   "data": [...],
>   "pagination": { "total": 1000, "limit": 20, "offset": 0 }
> }
> ```

---

## Success Metrics

How do we measure success? Provide measurable metrics.

**Example:**

- **Response Time:** Reduce average search response time from 2s to <200ms (90% improvement)
- **Database CPU:** Reduce database CPU usage during search by 60%
- **User Satisfaction:** Increase search satisfaction score from 3.2 to 4.5 (out of 5)
- **Test Coverage:** Maintain >90% test coverage for search functionality

---

## Testing Strategy

### Regression Tests

Ensure existing functionality still works.

**Example:**

- All existing search queries return correct results
- Search filters (by role, status, etc.) still work
- Search permissions are enforced correctly
- No performance degradation for other database operations

### Performance Tests

Measure performance improvements.

**Example:**

- Benchmark search response time with 100k, 500k, 1M users
- Load test search endpoint with 100 concurrent requests
- Measure database CPU usage before and after optimization
- Test pagination performance with large result sets

### Manual Verification

Any manual checks needed.

**Example:**

- Verify search results are relevant and ranked correctly
- Test pagination UI with large result sets
- Verify search works across different user roles
- Test edge cases (empty search, special characters, etc.)

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
    labelIds: ["IMPROVEMENT-ID", "DATABASE-ID", "BACKEND-ID"]
    ...
  })
}
```

### Related Issues

```graphql
mutation {
  issueRelationCreate(input: { issueId: "CURRENT-IMPROVEMENT-ID", relatedIssueId: "RELATED-ISSUE-ID", type: related })
}
```

---

## Related Documentation

- [AI agent Guide](../guide.md) - GraphQL examples and algorithms
- [Label Schema](../labels.md) - Label selection guidance
- [Workflow Guide](../guide.md) - Complete workflow documentation
