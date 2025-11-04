# Critical Self-Review Playbook

## Overview

Healthy skepticism catches real issues. Evidence-based verification prevents false confidence. Industry best practices validation ensures production-ready code.

## When to Use

- After completing any implementation
- Before marking tasks as complete
- Before creating pull requests
- Before deploying to production

## Core Principles

1. **Challenge Own Work** - Look for flaws, not confirmations
2. **Evidence-Based Verification** - Use terminal tools to gather evidence
3. **Industry Best Practices** - Validate against official documentation
4. **Significant Impact Only** - Only suggest changes with proper justification
5. **Skeptical Mindset** - Start with uncertainty, not confidence

## Workflow

### Phase 1: Self-Review Checklist

Before marking complete, verify:

- [ ] All code paths are tested
- [ ] All functions are used (no dead code)
- [ ] All security controls are implemented
- [ ] All configuration is centralized
- [ ] All logging is sanitized
- [ ] All error handling is comprehensive
- [ ] All timeouts are implemented
- [ ] All edge cases are handled

### Phase 2: Evidence-Based Verification

Use terminal tools to gather evidence:

```bash
# Find all function usages
rg "functionName" --type ts

# Find all hardcoded values
rg "[0-9]{5,}" --type ts

# Find all TODO/FIXME comments
rg "TODO|FIXME" --type ts

# Find all console.log statements
rg "console\.log" --type ts

# Find all unused imports
rg "^import.*from" --type ts | rg -v "used"

# List all files in directory
fd --type f --extension ts
```

### Phase 3: Industry Best Practices Validation

Use Context7 MCP to verify:

- Official documentation patterns
- Security best practices
- Performance optimization techniques
- Error handling standards
- Testing strategies

### Phase 4: Findings Classification

Classify findings by severity:

- **CRITICAL** - Security vulnerabilities, data loss, system crashes
- **HIGH** - Functional bugs, performance issues, incorrect behavior
- **MEDIUM** - Code quality issues, maintainability concerns, minor bugs
- **LOW** - Style inconsistencies, documentation gaps, minor improvements

### Phase 5: Recommendations

For each finding:

1. **Describe the issue** - What is wrong?
2. **Explain the impact** - Why does it matter?
3. **Provide solution** - How to fix it?
4. **Show code example** - Demonstrate the fix
5. **Justify significance** - Why is this change important?

## Anti-Patterns

❌ **Don't:**
- Assume code is correct without verification
- Look for confirmations instead of flaws
- Suggest changes without proper justification
- Create functions "just in case" (YAGNI)
- Skip verification for "simple" changes

✅ **Do:**
- Challenge own work after implementation
- Use terminal tools for evidence-based verification
- Only suggest changes with significant impact
- Remove unused functions immediately
- Verify all code paths are tested and used

## Success Metrics

- All findings identified before production
- Zero CRITICAL/HIGH findings after review
- Confidence level: 90%+ after review
- All recommendations have proper justification
- All code paths tested and verified

## Example: Goldsky CLI Migration

### Initial Implementation (Before Review)

Confidence: 70%

### Critical Self-Review Findings

1. **CRITICAL** - validateEnvironment() never called
   - **Issue:** Function created but never invoked
   - **Impact:** Environment validation skipped, could deploy with missing vars
   - **Solution:** Add validateEnvironment() call in deploy.js
   - **Evidence:** `rg "validateEnvironment" --type js` shows 0 usages

2. **HIGH** - Command injection vulnerability
   - **Issue:** Using execSync with string interpolation
   - **Impact:** Potential command injection attack
   - **Solution:** Replace with spawn() using array arguments
   - **Evidence:** `rg "execSync.*\$\{" --type js` shows vulnerable pattern

3. **MEDIUM** - Unused functions (redactSensitiveData, validatePath)
   - **Issue:** Functions created but never used
   - **Impact:** Dead code increases maintenance burden
   - **Solution:** Remove unused functions
   - **Evidence:** `rg "redactSensitiveData|validatePath" --type js` shows 0 usages

4. **MEDIUM** - Hardcoded chain IDs
   - **Issue:** Chain IDs hardcoded in network mapping
   - **Impact:** Configuration drift, not single source of truth
   - **Solution:** Generate dynamically from config/networks.json
   - **Evidence:** `rg "84532|8453" --type js` shows hardcoded values

5. **MEDIUM** - Missing fetch() error handling
   - **Issue:** No try/catch around fetch() calls
   - **Impact:** Unhandled promise rejections
   - **Solution:** Add comprehensive error handling
   - **Evidence:** `rg "await fetch" --type js` shows unprotected calls

6. **LOW** - override: false causing silent failures
   - **Issue:** Deployment fails silently if version exists
   - **Impact:** Confusing user experience
   - **Solution:** Change to override: true with warnings
   - **Evidence:** User feedback from testing

7. **LOW** - checkGoldskyAuth() race condition
   - **Issue:** Timeout and cleanup not properly handled
   - **Impact:** Process could hang
   - **Solution:** Improve timeout and cleanup logic
   - **Evidence:** Code review of spawn() usage

### After Review (All Findings Fixed)

Confidence: 95%

## Key Takeaways

1. **Critical self-review catches real issues** - 7 findings (1 CRITICAL, 1 HIGH, 3 MEDIUM, 2 LOW)
2. **Evidence-based verification builds confidence** - Terminal tools provide proof
3. **Industry best practices validation** - Context7 MCP ensures production-ready code
4. **Significant impact only** - Only suggest changes with proper justification
5. **Skeptical mindset** - Start with uncertainty, challenge own work
6. **Verify before marking complete** - All code paths tested and used

## Checklist Template

```markdown
## Critical Self-Review

### Code Paths
- [ ] All functions are used (no dead code)
- [ ] All code paths are tested
- [ ] All edge cases are handled

### Security
- [ ] No command injection vulnerabilities
- [ ] All logging is sanitized
- [ ] All API keys are protected
- [ ] All .env files have correct permissions
- [ ] All timeouts are implemented

### Configuration
- [ ] All configuration is centralized
- [ ] No hardcoded values
- [ ] All config functions are used
- [ ] Dynamic configuration (environment-driven)

### Error Handling
- [ ] All async operations have try/catch
- [ ] All errors are logged properly
- [ ] All user-facing errors are clear
- [ ] All edge cases are handled

### Testing
- [ ] All critical paths are tested
- [ ] All fixes have automated tests
- [ ] All tests pass
- [ ] End-to-end workflows verified

### Documentation
- [ ] All changes are documented
- [ ] All Linear issues updated
- [ ] All DoD sections completed
- [ ] All evidence provided

### Confidence Level
- [ ] 90%+ confidence in implementation
- [ ] All findings addressed
- [ ] Production-ready
```

## Review Criteria (8 Categories)

1. **Consistency** - Naming, style, error handling
2. **Security** - No secrets, sanitized logging, input validation
3. **Maintainability** - Self-documenting code, SRP
4. **Separation of Concerns** - Clear boundaries, proper abstraction
5. **Native Solutions First** - Native CLI/terminal commands over wrappers
6. **Best Practices** - Verify against official docs
7. **Complexity & Simplicity** - Avoid deep nesting >3 levels, functions <50 lines
8. **Over-Abstraction** - Question wrapper value, prefer direct native tool usage

## Output Format

```markdown
## File: path/to/file.ts

### Assessment: Pass / Needs Improvement / Fail

### Findings

#### Consistency
- ✅ Naming conventions consistent
- ❌ Error handling inconsistent (lines 45-67)

#### Security
- ✅ No secrets in code
- ❌ Logging not sanitized (line 89)

#### Maintainability
- ✅ Self-documenting code
- ❌ Function too long (>50 lines, line 120-180)

### Recommendations

1. **Standardize error handling** (MEDIUM)
   - Lines 45-67 use different error patterns
   - Impact: Inconsistent user experience
   - Solution: Use consistent try/catch pattern
   ```typescript
   try {
     // operation
   } catch (error) {
     logger.error('Operation failed', { error });
     throw error;
   }
   ```

2. **Sanitize logging** (HIGH)
   - Line 89 logs API key in URL
   - Impact: Security vulnerability
   - Solution: Redact sensitive data
   ```typescript
   logger.info('Request sent', { url: redactUrl(url) });
   ```

### Risk Level: 6/10

### Priority: High
```

