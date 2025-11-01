# Definition of Done (DoD)

Simple checklist-style evidence format for closing Linear issues.

---

## General Rule

Before marking any issue as "Done", add a final comment with evidence using this pattern:

```markdown
## ✅ Done

**Changed:**

- File paths and what changed

**Tested:**

- Test results or verification steps

**Verified:**

- How you confirmed it works
```

---

## Comment Pattern

### Required Structure

```markdown
## ✅ Done

**Changed:**
[List files and changes]

**Tested:**
[Test results or manual verification]

**Verified:**
[Proof it works - screenshots, logs, output]
```

### Pattern Rules

1. Start with `## ✅ Done`
2. Include all 3 sections: Changed, Tested, Verified
3. Use bullet points for clarity
4. Keep it concise - evidence not essays

---

## Examples

### Feature Example

```markdown
## ✅ Done

**Changed:**

- `src/auth/oauth.ts` - OAuth provider integration
- `src/components/LoginButton.tsx` - Added provider buttons

**Tested:**

- Unit tests: 45/45 passed
- E2E tests: 8/8 passed

**Verified:**

- Screenshot: login-page.png
- Login flow works in 1.2s
```

### Bug Fix Example

```markdown
## ✅ Done

**Changed:**

- `src/utils/validation.ts` - Fixed email regex

**Tested:**

- Added test case for edge case
- All validation tests pass (12/12)

**Verified:**

- Email validation now accepts plus signs
- Tested with user@example+test.com ✅
```

### Improvement Example

```markdown
## ✅ Done

**Changed:**

- `src/api/cache.ts` - Implemented Redis caching

**Tested:**

- Load tests: 500ms → 50ms response time
- Cache hit rate: 85%

**Verified:**

- Performance metrics in production
- Redis dashboard shows active caching
```

---

## Quick Checklist

Before closing an issue, ensure your comment includes:

- [ ] File paths that changed
- [ ] What was modified/added/fixed
- [ ] Test results or verification steps
- [ ] Proof it works (screenshot, log, output, metrics)

---

## AI Agent Usage

When closing Linear issues via MCP:

1. Collect evidence during work
2. Format final comment using pattern above
3. Add comment to issue
4. Mark issue as "Done"

**GraphQL Example:**

```graphql
mutation AddDoDComment {
  commentCreate(input: { issueId: "ISSUE-ID", body: "## ✅ Done\n\n**Changed:**\n- file.ts - description\n\n**Tested:**\n- results\n\n**Verified:**\n- proof" }) {
    success
  }
}
```
