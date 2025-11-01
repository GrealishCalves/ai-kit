Act as a senior automation and QA engineer conducting a comprehensive test design review of our mock testing infrastructure. Your task is to evaluate all mock tests (tagged with `@mock`) against the principles defined in @/Users/maxim/repository/chancev4/prompts/test-fail.md.

## 🎯 Objective

Review all test assertions in the specified test file against the actual application behavior to ensure:

1. **Tests catch real problems** when they fail
2. **Assertions have specific, meaningful thresholds** (not vague like `> 0`)
3. **No false negatives** (tests passing when they shouldn't)
4. **Failures provide actionable insights** (clear indication of what's wrong)
5. **Tests represent reality**, not manipulated to pass

---

## 📋 Analysis Framework

### Step 1: Identify All Assertions

For each test in the file:

- List every `expect()` statement
- Document what is being tested
- Note current threshold values
- Identify assertion type (count, boolean, performance, content)

### Step 2: Analyze Against Actual App Behavior

For each assertion:

**Understand the Code:**

- What does the application actually do?
- What values/states does it produce under normal conditions?
- What is the expected range based on codebase analysis?
- What components/hooks/functions are involved?

**Evaluate Current Threshold:**

- Is it too loose? (e.g., `> 0` when expecting 10-20)
- Is it too strict? (e.g., exact match when variance expected)
- Is there a missing upper bound? (could hide leaks/duplicates)
- Is there a missing lower bound? (could hide missing functionality)

**Identify False Negative Risks:**

- Would this pass with broken functionality?
- Does it hide partial failures?
- Is it testing the right thing?
- Could it pass with wrong data?

### Step 3: Determine Realistic Thresholds

Based on actual app behavior:

**Expected Value:**

- Analyze code to determine normal behavior
- Consider timing, async operations, and variance
- Document the expected value under normal conditions

**Reasonable Buffer:**

- **2x buffer** for timing variance
- **2.5x buffer** for async operations with multiple steps
- **No buffer** for critical exact matches (booleans, counts that must be exact)

**Upper and Lower Bounds:**

- **Lower bound**: Catches missing functionality
- **Upper bound**: Catches leaks, duplicates, regressions
- **Both**: Provides clear acceptable range

### Step 4: Recommend Specific Changes

For each problematic assertion:

**Current Code:**

```typescript
// Show the current assertion
```

**Problem:**

- Why it's bad
- What it hides or misses
- Example of false negative

**Recommended Code:**

```typescript
// Show the improved assertion with specific thresholds
```

**Rationale:**

- Expected value based on code analysis
- Why this threshold catches real problems
- What buffer is applied and why

**What Failure Means:**

- Specific problem indicated
- Impact on functionality
- Action required to fix

---

## 🔍 Key Questions to Ask

### For Count Assertions (subscriptions, events, items):

- What is the expected count based on code analysis?
- What would indicate too few? (missing functionality)
- What would indicate too many? (leaks, duplicates)
- Should there be both lower and upper bounds?

### For Performance Assertions (time, cost, resources):

- What is the expected value under normal conditions?
- What buffer accounts for timing variance?
- At what point does it indicate a real regression?
- Is the threshold 2x, 5x, or 10x the expected value?

### For Boolean Assertions (enabled, active, valid):

- Is this a critical invariant?
- Should it be a hard failure (`expect()`) or soft (`expect.soft()`)?
- What does failure indicate?
- Is there additional context needed?

### For Content Assertions (types, values, structure):

- Are we testing existence or correctness?
- Should we validate specific values, not just presence?
- Are we checking the right properties?
- Could it pass with wrong data?

---

## ⚠️ Key Principles

### 1. No Manipulation

- Tests should represent reality, not be adjusted to pass
- If a test fails, fix the code or understand why the threshold is wrong
- Never loosen thresholds just to make tests pass

### 2. Catch Problems Early

- Fail at 2x variance, not 5x or 10x
- Upper bounds prevent leaks from growing unchecked
- Lower bounds prevent silent feature loss

### 3. Actionable Failures

- Every failure should clearly indicate the problem
- Every failure should suggest the fix
- Every failure should be reproducible

### 4. Reality-Based

- Thresholds based on actual app behavior
- Analyzed from codebase, not guessed
- Verified with real test runs

### 5. No False Negatives

- Tests must fail when functionality is broken
- Tests must fail when performance regresses
- Tests must fail when resources leak

### 6. Use Hard Failures for Critical Assertions

- Use `expect()` for critical functionality
- Use `expect.soft()` only for informational checks
- Critical = optimization goals, core functionality, invariants

---

## 📊 Output Format

Provide analysis in this structure:

### 1. Current Problems: Too Loose/Strict/Missing

For each problematic assertion:

```typescript
// ❌ CURRENT
expect.soft(value).toBeGreaterThan(0);
```

**Why This Is Bad:**

- Passes with 1 when expecting 10-20
- No upper bound to catch leaks
- Uses soft assertion for critical check

**What It Hides:**

- Missing functionality (< expected)
- Resource leaks (> expected)
- Performance regressions

### 2. Recommended Thresholds: Specific, Actionable, Honest

For each assertion:

```typescript
// ✅ UPDATED
expect(value).toBeGreaterThanOrEqual(10);
expect(value).toBeLessThanOrEqual(25);
```

**Threshold:** 10-25 (expected 10-20, 2.5x buffer)

**Rationale:**

- App creates 10-20 items based on code analysis
- Upper bound catches leaks/duplicates
- Lower bound catches missing functionality
- Hard failure for critical check

**What Failure Means:**

- `< 10`: Missing items, functionality broken
- `> 25`: Leak, duplication, or regression

**Action:** Changed from soft to hard failure, added specific range

### 3. Summary of Changes

| Assertion  | Old Threshold | New Threshold | Tightness            | Impact              |
| ---------- | ------------- | ------------- | -------------------- | ------------------- |
| Item count | `> 0`         | `10-25`       | 🔥 10x tighter       | Catches leaks       |
| Cost       | `< 1000`      | `< 150`       | 🔥 6.6x tighter      | Catches regressions |
| Events     | `≥ 1`         | `1-5`         | 🔥 Added upper bound | Catches duplicates  |

### 4. What These Changes Catch

**Before (Too Loose):**

- ✅ Passes with 1 item (missing functionality)
- ✅ Passes with 100 items (massive leak)
- ✅ Passes with 900 cost (9x expected)

**After (Honest, Actionable):**

- ❌ Fails with < 10 items → Missing functionality
- ❌ Fails with > 25 items → Leak or duplication
- ❌ Fails with > 150 cost → Performance regression

### 5. Verification

- Run tests with new thresholds
- Confirm they pass with current implementation
- Confirm they would fail with known issues
- Document threshold justification

---

## 🚀 Usage Template

```
I need you to conduct a critical review of the test file at `[TEST_FILE_PATH]`.

Please analyze every assertion against the actual application behavior and provide:

1. **Critical Analysis**: Identify assertions that are too loose, too strict, or missing bounds
2. **Specific Recommendations**: Provide exact threshold values with rationale based on code analysis
3. **Implementation**: Show code changes needed with before/after comparison
4. **Verification**: Explain how to verify thresholds are realistic

Focus on ensuring:
- No false negatives (tests passing when they shouldn't)
- Specific, actionable thresholds (not just `> 0` or `< 1000`)
- Reality-based testing (thresholds match actual app behavior)
- Actionable failures (clear indication of what's wrong)
- Hard failures for critical assertions (use `expect()` not `expect.soft()`)

The goal is to ensure these tests provide **reliable early warning signals** when functionality breaks or regresses, with thresholds that are neither too permissive nor too brittle.

I want to make sure we're not manipulating tests to pass - I want them to represent the truth and catch real problems.
```

---

## ✅ Success Criteria

A successful review results in tests that:

1. ✅ **Catch real problems** - Fail when functionality breaks
2. ✅ **Avoid false negatives** - Don't pass with broken code
3. ✅ **Provide actionable feedback** - Clear indication of what's wrong
4. ✅ **Use realistic thresholds** - Based on actual app behavior (2-2.5x buffer)
5. ✅ **Represent reality** - Not manipulated to pass
6. ✅ **Are maintainable** - Clear rationale for thresholds documented
7. ✅ **Are verifiable** - Pass with current implementation, fail with known issues

---

## 🎓 Philosophy

**Tests are documentation of expected behavior.**

- If a test passes with broken functionality → **the test is lying**
- If a test fails with correct functionality → **the test is wrong**
- If a test provides no actionable insight → **the test is useless**

**Good tests:**

- Tell the truth about the system
- Catch problems early (2x variance, not 5x or 10x)
- Provide clear guidance on fixes
- Are based on reality, not wishful thinking
- Use hard failures for critical checks

**This prompt helps you create good tests.**
