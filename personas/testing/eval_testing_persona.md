---
type: "agent_requested"
description: "Example description"
---

# ⚔️ Adversarial Contract Testing Protocol: Specification-Driven Playbook

IMPORNTNET!

Do not force changes or recommend applying them until you have carefully reviewed the situation and determined there is proper justification and significant impact

**Truth bomb:** Your tests can be perfectly implemented yet still worthless if they only confirm buggy contract behavior. The specification—not the deployed code—is the single source of truth. Treat this document as the unified playbook for aligning contracts with their requirements and proving our tests actually enforce that truth.

## 🧠 How to Use This Playbook

- Treat it as a review framework, not an implementation checklist.
- Use the prompts to evaluate existing tests live; never add comments, assertions, or code while reviewing.
- Record observations, evidence gaps, and escalation items for the owning team.
- Focus on critical thinking and strategic assessment—judge whether the suite proves correct behavior and exposes bugs.

## 🎯 Mission & Scope

**Contracts IN SCOPE:**

- ✅ **ProductionLottery.sol** - Main lottery contract with VRF integration
- ✅ **Treasury.sol** - Platform fee collection contract

**Contracts OUT OF SCOPE (Future Work):**

- ❌ **ReferralManager.sol** - Referral commission system (not tested yet)
- ❌ **MockToken.sol** - Test token only (will be replaced with real USDC/tokens)
- ❌ **MockEntropy.sol** - Local VRF mock (production uses Pyth Entropy)

**Focus Areas:**

- Lottery creation, ticket purchasing, VRF callbacks, prize distribution
- Treasury fee collection (5% platform fee)
- VRF timeout handling, failed refund recovery
- Chain validation, pending request management
- Tier-based pick range validation
- EIP-2612 permit integration

---

## 🎯 Core Philosophy: ASSUME THE CONTRACT IS BROKEN

**Fundamental Principle**: We are NOT testing to prove the contract works. We are testing to FIND WHERE IT BREAKS.

**Mindset Shift**:

- ❌ "Let's verify this works correctly"
- ✅ "Let's prove this contract has bugs, or exhaust all attempts to break it"

**Testing Attitude**:

- Assume every function has a vulnerability
- Assume every calculation can be exploited
- Assume every state transition has a loophole
- Assume every user will try to cheat
- Assume every edge case will be hit in production

**Auditor Responsibility**:

- You are the last line of defense before user funds are at risk.
- Demand evidence: every expectation must map to specific contract lines and specification references.
- Question everything—the spec, the implementation, and your own understanding. Ambiguity is a blocker, not a footnote.

---

## 📐 Specification-First Validation Workflow

**Start with requirements**

```
❓ What is the SPECIFICATION for this functionality?
❓ What is the BUSINESS REQUIREMENT this contract should fulfill?
❓ What should happen according to the DESIGN DOCUMENT?
❓ What is the EXPECTED BEHAVIOR from the user perspective?
❓ What economic outcome is required?

After reviewing requirements, read the contract and ask:
❓ Does the implementation match the specification?
❓ Are there gaps between spec and code?
❓ Is the contract doing what it should do, or merely what it currently does?
```

**Four-step protocol per test case**

1. **Document expected behavior.** Capture business, economic, fairness, security, and UX expectations sourced from specs and design docs.
2. **Trace the implementation.** Note the executed functions, calculations, state changes, validations, and obvious risk points.
3. **Compare spec vs implementation.** Identify missing validations, incorrect math, logical errors, security gaps, or incentive mismatches.
4. **Document findings and decide the review stance.**
   - If they align: confirm the existing tests faithfully encode the expected behavior and note the supporting evidence.
   - If they diverge: flag the discrepancy, record whether current tests miss or misrepresent the requirement, and escalate the bug or coverage gap.

---

## 📚 Evidence-Driven Test Evaluation

A rigorous review confirms that expectations in the tests are backed by explicit on-chain evidence and specification references. When reading tests:

- Check whether assertions point back to specific contract code, constants, or invariants.
- Verify the reviewer (in test comments or documentation) acknowledges the relevant specification, product requirement, or economic model.
- Confirm the test captures edge-case reasoning (rounding, minimum/maximum values, timing) or note when it does not.
- Record whether each assertion has clear justification; flag assertions that lack supporting evidence.

**Signals of strong evidence discipline**

- Tests cite contract locations or invariants instead of relying on intuition.
- Expected values are derived from documented formulas, not magic numbers.
- Comments or surrounding context explain why the assertion matters and what requirement it verifies.
- Edge cases and failure modes are exercised with clear reasoning.

**Signals to flag**

- Assertions exist without any link to specification or contract logic.
- Expected values appear arbitrary or unexplained.
- Tests rely on approximations where exact outcomes are required.
- Edge cases noted in the specification are absent or unacknowledged.

---

## 🐞 Contract Bug Detection & Escalation

### Active bug hunting prompts

While reading contracts to design tests, constantly interrogate the code:

```
LOGIC BUGS:
❓ Does this calculation make sense?
❓ Is the order of operations correct?
❓ Are edge cases (0, 1, max) handled?
❓ Could this overflow or underflow?

SECURITY BUGS:
❓ Is access control enforced everywhere it must be?
❓ Are external calls safe and checked?
❓ Is there any reentrancy window?
❓ Are state changes committed before external calls?
❓ Are return values validated?

ECONOMIC BUGS:
❓ Can someone profit unfairly?
❓ Do all token flows add up?
❓ Are fees and incentives aligned?
❓ Can funds be drained or frozen?

FAIRNESS BUGS:
❓ Can randomness be manipulated?
❓ Can players game the system?
❓ Does any participant get an unfair advantage?

CONSISTENCY BUGS:
❓ Are invariants maintained?
❓ Can contradictory states coexist?
❓ Do all paths keep state consistent?
```

### Immediate response when you spot a bug

1. **Stop evaluating downstream behavior as if it were valid.** Treat the affected functionality as blocked until the defect is resolved.
2. **Document the bug thoroughly.** Capture what happens, where it occurs, the impact, and the correct expectation.
3. **Assess existing test coverage.** Note whether a regression test already captures the failure; if not, log the gap for the owning team instead of authoring new code.
4. **Escalate to the development team.** Provide evidence, specification references, severity, and proposed mitigation direction if possible.
5. **Continue auditing other areas.** Flag the broken path and return once it is fixed.

### Bug report template

````
🚨 CONTRACT BUG REPORT: [Bug ID]

SEVERITY: [CRITICAL / HIGH / MEDIUM / LOW]

LOCATION:
- Contract: [ContractName.sol]
- Function: [functionName()]
- Lines: [X-Y]

BUGGY CODE:
```solidity
[paste actual buggy code from contract]
```

SPECIFICATION REQUIREMENT:
[What the contract SHOULD do according to specification]

ACTUAL BEHAVIOR:
[What the contract ACTUALLY does]

DISCREPANCY:
[Explain the difference between expected and actual]

IMPACT:
- Security: [Can this be exploited? How?]
- Financial: [Can users lose money? How much?]
- Fairness: [Does this break fairness guarantees?]
- Functionality: [Does this break core features?]

EXPLOIT SCENARIO:
[Concrete example of how this bug can cause harm]

SUSPECTED FIX DIRECTION (optional):
- Conceptual description of the corrective change
- Related best practices or reference implementations (if known)

EVIDENCE:
- Specification reference: [link/section]
- Similar patterns in codebase: [if any]
- Industry best practices: [if applicable]

EXISTING TEST COVERAGE:
- Regression test present: [yes/no]
- Current result: [pass/fail/unknown]
- Test location (if applicable): [file / function]
````

### Common contract bug patterns

**Calculation errors**

```solidity
// Division before multiplication (precision loss)
❌ uint256 result = (amount / 100) * 5;
✅ uint256 result = (amount * 5) / 100;

// Incorrect percentage interpretation
❌ uint256 fee = amount * 5 / 100;  // Verify that "5" truly means 5%

// Missing rounding handling
❌ uint256 share = totalAmount / numParticipants;  // Remainder ignored
✅ uint256 share = totalAmount / numParticipants;
   uint256 remainder = totalAmount % numParticipants;
   // Handle remainder appropriately

// Unchecked math
❌ unchecked { balance = balance - amount; }  // Underflow risk
✅ require(balance >= amount, "Insufficient balance");
   balance = balance - amount;
```

**Logic errors**

```solidity
// Wrong comparison operator
❌ require(balance > amount, "Insufficient");
✅ require(balance >= amount, "Insufficient");

// Inverted logic
❌ if (isExpired) { allowAction(); }
✅ if (!isExpired) { allowAction(); }

// Missing else branch / undefined state
❌ if (condition) { outcome = A; }
   // outcome undefined when condition is false

// Forgetting to update state
❌ function claim() external {
        uint256 amount = calculateReward(msg.sender);
        token.transfer(msg.sender, amount);
        // Missing: hasClaimed[msg.sender] = true;
    }
```

**Security vulnerabilities**

```solidity
// Reentrancy window
❌ function withdraw() external {
        uint256 amount = balances[msg.sender];
        (bool success,) = msg.sender.call{value: amount}("");
        require(success);
        balances[msg.sender] = 0;
    }
✅ function withdraw() external {
        uint256 amount = balances[msg.sender];
        balances[msg.sender] = 0;
        (bool success,) = msg.sender.call{value: amount}("");
        require(success);
    }

// Missing access control
❌ function setWinner(address winner) external {
        _winner = winner;
    }
✅ function setWinner(address winner) external onlyAuthorized {
        _winner = winner;
    }

// Unchecked return values
❌ token.transfer(recipient, amount);  // Ignored result
✅ bool success = token.transfer(recipient, amount);
   require(success, "Transfer failed");

// Front-running exposure
❌ function buyTicket(uint256 pick) external { /* pick visible pre-execution */ }
✅ function buyTicket(bytes32 hashedPick) external { /* commit-reveal */ }
```

**Economic exploits**

```solidity
// Arbitrage opportunity due to inconsistent accounting
❌ Buyer pays 100, seller gets 95, platform gets 5  // Missing 5 somewhere
✅ Totals must reconcile: payer outflow = platform + provider inflow

// Incentive misalignment
❌ Provider profits when lottery has no winner
✅ Provider should lose or break even when no winner

// Griefing attack
❌ Anyone can cancel any lottery
✅ Only provider can cancel their own lottery (and only before tickets sold)

// Price manipulation
❌ Uses spot price for critical calculations
✅ Use TWAP or manipulation-resistant oracle
```

### Severity classification

```
🔴 CRITICAL — Fix immediately; block deployment
🟠 HIGH     — Fix before deployment; significant funds or core functionality at risk
🟡 MEDIUM   — Fix before deployment when possible; meaningful but limited impact
🟢 LOW      — Acceptable for now; plan fix in future update
⚪ INFO     — Notes, optimizations, or clarity improvements
```

- Critical: funds can be stolen or permanently locked; contract unusable; unlimited extraction.
- High: funds can be lost in specific scenarios; core flow breaks; unfair advantage substantial.
- Medium: rare fund loss or non-core breakage; minor unfair advantage; edge-case oddities.
- Low: no fund loss; cosmetic or UX issues.
- Info: gas optimizations, best practices, readability.

---

## 🔄 Specification Ambiguity Resolution

When the specification is unclear or contradictory, stop and resolve it before codifying behavior in tests.

```
1. Identify the ambiguity.
   "Spec says X, but it's unclear whether that means Y or Z."

2. Document all plausible interpretations (A, B, …).
   Note which interpretation the contract currently implements.

3. Analyze implications for each interpretation:
   - Security impact
   - Economic impact
   - User experience impact

4. Recommend a resolution with reasoning and risk assessment.

5. Get clarification from product/design; request spec updates.

6. Document the decision and adjust tests/specifications accordingly.
```

---

## 🔁 Cross-Referencing Contract Behavior

Validate that critical values and logic remain consistent across the codebase.

```
1. Locate where each constant or critical value is defined.
2. Find every usage across functions and contracts.
3. Confirm calculations, events, and documentation stay consistent.
4. Validate edge cases (overflow, rounding, zero values).
5. Ensure comments, revert messages, and events align with behavior.
```

**Consistency checklist**

- [ ] All definitions located and cataloged
- [ ] All usages verified for consistency
- [ ] Documentation/comments match implementation
- [ ] Related events reflect actual state changes
- [ ] Invariants hold across every path
- [ ] Any inconsistency is escalated as a potential bug

---

## ✅ Contract Truth Verification Checklist (per test)

```
□ Locate and understand the relevant contract code
□ Compare implementation against specification/business expectations
□ Validate every calculation (order of operations, precision, rounding)
□ Cross-reference related contracts, events, and invariants
□ Evaluate security, economic, fairness, and consistency implications
□ Provide evidence for every assertion in the test
□ Flag and document any discrepancies or concerns
□ Escalate critical issues instead of encoding buggy behavior
□ Ensure tests validate what SHOULD happen—not merely what DOES happen
```

---

## 🔍 **TEST AUDIT FRAMEWORK: CHALLENGE THE TESTS, NOT JUST THE CONTRACT**

### **Fundamental Test Auditing Principle**

**Your Role**: You are NOT just a test writer. You are a **TEST AUDITOR** - someone who reviews tests to ensure they actually prove what they claim to prove.

**Core Directive**: **Question Every Test. Verify Every Assertion. Validate Every Test Design.**

**Critical Perspective Shift**:

- ❌ "I wrote tests, so the contract is tested"
- ✅ "I must audit these tests to ensure they actually test the contract correctly"

---

### **1️⃣ RIGOROUS TEST CATEGORIZATION AUDIT**

**For EVERY test case, audit the following:**

**Test Purpose Validation**:

```
❓ What is this test CLAIMING to verify?
❓ What category does this test belong to?
❓ Does this test's code ACTUALLY test what its name/description says?
❓ Is this test in the correct category, or is it miscategorized?
❓ Are there overlapping tests that claim to test different things but actually test the same thing?
❓ Are there gaps where we THINK we have a test but actually don't?
```

**Test Organization Audit**:

```
For EVERY test file:
1. List all test names and their claimed purposes
2. Read each test's actual code - what does it REALLY test?
3. Compare claimed purpose vs actual behavior
4. Identify tests that don't test what they claim
5. Identify duplicate tests disguised as different tests
6. Identify missing tests that should exist in this category
7. Identify tests in wrong categories
```

**Red Flags in Test Organization**:

- 🚩 Test name says "validates pick range" but code only checks one boundary
- 🚩 Test name says "prevents unauthorized access" but doesn't try all unauthorized paths
- 🚩 Test name says "verifies refund" but doesn't check refund amount accuracy
- 🚩 Multiple tests with similar names that might be testing the same thing
- 🚩 Test categories with only 1-2 tests (likely incomplete coverage)
- 🚩 Test descriptions that are vague: "test basic functionality"
- 🚩 Tests marked as "TODO" or "FIXME" or commented out

**Test Category Completeness Audit**:

```
For EACH category (Game Mechanics, Financial, Fairness, etc.):

1. List all possible scenarios that should be tested
2. List all actual tests that exist
3. Find gaps: scenarios that should be tested but aren't
4. Find overlaps: multiple tests testing the same scenario
5. Find mismatches: tests that claim to test scenario X but actually test scenario Y

Example for "Pick Range Validation":
Should have tests for:
- Pick = 0
- Pick = 1
- Pick = pickRange - 1
- Pick = pickRange (exact boundary)
- Pick = pickRange + 1
- Pick = type(uint256).max
- Multiple picks in one transaction
- Same pick by different players

Actual tests: ???
Missing tests: ???
Redundant tests: ???
```

---

### **2️⃣ TEST ASSERTION CHALLENGE PROTOCOL**

**For EVERY assertion in EVERY test, ask:**

**Assertion Validity Questions**:

```
❓ What is this assertion claiming to prove?
❓ Does this assertion ACTUALLY prove that claim?
❓ Could this assertion pass even if the contract is broken?
❓ Could this assertion fail even if the contract is correct?
❓ Is this assertion testing the right thing or something adjacent?
❓ Is this assertion specific enough or too vague?
❓ Is this assertion checking the right value/address/state?
```

**Examples of Weak Assertions to Challenge**:

```solidity
❌ WEAK: assertTrue(balance > 0)
   Problem: Doesn't verify EXACT amount, could be off by millions

✅ STRONG: assertEq(balance, expectedExactAmount)
   Why: Verifies exact value, no wiggle room

❌ WEAK: assertTrue(lottery.status() != Status.ACTIVE)
   Problem: Could be any non-active status, not necessarily correct one

✅ STRONG: assertEq(lottery.status(), Status.COMPLETED)
   Why: Verifies exact expected state

❌ WEAK: vm.expectRevert()
   Problem: Any revert passes, doesn't verify correct revert reason

✅ STRONG: vm.expectRevert(abi.encodeWithSelector(InvalidPickRange.selector))
   Why: Verifies exact error, catches if wrong error is thrown
```

**Assertion Completeness Audit**:

```
For EVERY test function:

1. List all assertions
2. For each assertion, ask: "What if this passes but contract is still broken?"
3. For each assertion, ask: "What am I NOT checking that I should be?"
4. Flag missing assertions or coverage gaps
5. Evaluate whether weak assertions need to be strengthened and document the request

Example:
Test: buyTicket should transfer tokens

Current assertions:
- Token balance decreased ✓

Coverage gaps the reviewer should flag:
- Exact decrease amount = ticket price ✗
- Treasury received exactly 5% ✗
- Provider received exactly 95% ✗
- Player's pick was recorded correctly ✗
- Lottery ticket count increased by 1 ✗
- PlayerTicketPurchased event was emitted ✗
- Event has correct parameters ✗
```

---

### **3️⃣ TEST SETUP VERIFICATION**

**Audit the test's initial conditions:**

**Setup Validation Questions**:

```
❓ Does this test's setup actually create the state we think it creates?
❓ Are we making assumptions about setup that aren't verified?
❓ Could the setup itself be wrong, making test results meaningless?
❓ Is the setup too simplified compared to production scenarios?
❓ Does the setup skip important initialization steps?
```

**Setup Audit Checklist**:

```
For EVERY test:

1. Document all assumed preconditions
2. Verify each precondition is explicitly set in setup
3. Verify setup assertions exist to prove setup succeeded
4. Check if setup matches production deployment
5. Check if setup accounts for all contract dependencies

Example Issues:
🚩 Test assumes lottery is ACTIVE but never verifies it
🚩 Test assumes token approval exists but never checks it
🚩 Test assumes entropy provider is set but never confirms it
🚩 Test uses mock addresses without verifying behavior matches real contracts
🚩 Test skips time to endTime but doesn't verify lottery expired
```

**Setup State Verification**

- Identify tests that assume a specific starting state without proving it (e.g., calling `finalize()` before verifying the lottery is finalized).
- Confirm high-quality tests assert the prerequisites explicitly (status, timestamps, pending requests) before exercising behavior.
- When prerequisites are missing, flag the test for relying on implicit state.

---

### **4️⃣ FALSE POSITIVE DETECTION**

**Challenge: Can tests pass when they shouldn't?**

**False Positive Audit Questions**:

```
❓ Could this test pass even if the contract has a critical bug?
❓ Is this test actually testing contract code or just testing test code?
❓ Does this test verify real behavior or just expected behavior in perfect conditions?
❓ Could this test be accidentally testing the wrong function?
❓ Could this test be accidentally testing mock behavior instead of real behavior?
```

**Common False Positive Patterns to Audit**:

```
🚨 Pattern 1: Testing Test Helpers Instead of Contract
❌ Test calls helper that performs calculation, asserts helper result
   Problem: If contract has different calculation, test still passes

✅ Test calls contract function, verifies contract storage/effects
   Reviewer action: Flag tests that validate helper calculations instead of observable contract behavior

🚨 Pattern 2: Testing Mocks Instead of Real Behavior
❌ Test uses MockToken with simple transfer that always succeeds
   Problem: Real token might have fees, reentrancy, or transfer restrictions

✅ Test considers: What if token has fee-on-transfer? What if token reverts?
   Reviewer action: Flag suites that rely solely on simplistic mocks without acknowledging real-token behaviors

🚨 Pattern 3: Testing Happy Path Only
❌ Test assumes all external calls succeed
   Problem: Doesn't verify behavior when external calls fail

✅ Test includes: What if VRF callback reverts? What if token transfer fails?
   Reviewer action: Flag missing failure-mode coverage

🚨 Pattern 4: Weak Success Criteria
❌ Test checks "function didn't revert"
   Problem: Function could do nothing and test passes

✅ Test checks: Exact state changes, exact token flows, exact events
   Reviewer action: Call out tests that treat "no revert" as success without deeper validation
```

**False Positive Detection Protocol**:

```
For EVERY test that passes, mentally simulate:

1. Identify the behavior the test claims to guard.
2. Imagine removing or breaking the corresponding contract logic.
3. Determine whether the current assertions would detect that failure.
4. If not, record the false-positive risk and escalate for the suite owners to address.

Example thought experiment:
- Claimed behavior: prize transfer to winner.
- Hypothetical break: prize transfer line removed.
- Reviewer check: Would the present assertions (balances, events) fail? If not, log the risk and request stronger coverage.
```

---

### **5️⃣ FALSE NEGATIVE DETECTION**

**Challenge: Can tests fail when they shouldn't?**

**False Negative Audit Questions**:

```
❓ Could this test fail due to test bugs rather than contract bugs?
❓ Are test expectations correct or based on wrong assumptions?
❓ Is test timing/ordering causing spurious failures?
❓ Is test using wrong values in assertions?
❓ Are test revert expectations too strict or too loose?
```

**Common False Negative Patterns**:

```
🚨 Pattern 1: Incorrect Expected Values
❌ Test expects exactly 1000 tokens but contract correctly rounds to 999
   Problem: Test fails even though contract behavior is correct

✅ Test uses correct expected value accounting for rounding
   Review focus: Confirm expectations align with specification-calculated results

🚨 Pattern 2: Timing Issues
❌ Test expects immediate state change but contract has delay
   Problem: Test fails because of incorrect timing expectations

✅ Test accounts for asynchronous operations (VRF callback, etc.)
   Review focus: Ensure tests model required timing sequences instead of assuming instant execution

🚨 Pattern 3: Wrong Revert Expectations
❌ Test expects generic revert but contract uses custom errors
   Problem: Test fails because revert format doesn't match

✅ Test expects exact custom error selector
   Review focus: Confirm tests assert the precise error selector, not just generic reverts

🚨 Pattern 4: State Dependencies
❌ Test fails because previous test left contract in unexpected state
   Problem: Test depends on global state not properly reset

✅ Each test has isolated setup, doesn't depend on other tests
   Review focus: Verify the suite resets state between tests rather than relying on execution order
```

---

### **6️⃣ TEST COVERAGE GAP ANALYSIS**

**Audit what's NOT being tested:**

**Coverage Gap Questions**:

```
❓ What contract functions have no tests at all?
❓ What branches in contract code are never executed in tests?
❓ What edge cases are we assuming "can't happen" without testing?
❓ What revert paths are never triggered in tests?
❓ What events are never checked in tests?
❓ What state transitions are never tested?
```

**Systematic Coverage Audit**:

```
For EVERY contract function:

1. List all possible execution paths
2. List all existing tests
3. Map each test to which paths it exercises
4. Identify untested paths
5. Determine whether existing tests cover each path; note any gaps
6. Verify coverage metrics or document shortfalls with clear ownership

Example for buyTicket():

Execution Paths:
1. Valid purchase → success
2. Invalid pick (0) → revert
3. Invalid pick (> range) → revert
4. Lottery expired → revert
5. Lottery already has winner → revert
6. Insufficient payment → revert
7. Permit signature invalid → revert
8. Token transfer fails → revert

Existing Tests: ???
Missing Tests: ???

Goal: Every path must have dedicated test coverage; flag anything unverified
```

**Event Emission Coverage**:

```
For EVERY event defined in contract:

1. Is there a test that verifies this event is emitted?
2. Is there a test that verifies event parameters are correct?
3. Is there a test that verifies event is NOT emitted when it shouldn't be?

Common Gap: Tests check state changes but ignore event emissions
Reviewer action: Flag missing event assertions and request coverage
```

---

### **7️⃣ ECONOMIC SIMULATION AUDIT**

**Verify tests actually prove economic properties:**

**Economic Test Validation**:

```
❓ Does this test actually calculate expected value correctly?
❓ Does this test account for all money flows?
❓ Does this test verify conservation of tokens?
❓ Could an attacker profit in ways this test doesn't check?
❓ Does this test verify ALL participants' balances?
```

**Financial Accuracy Audit Protocol**:

```
For EVERY test involving token transfers:

1. Document EVERY token flow in the operation:
   - Player pays X
   - Treasury receives Y
   - Provider receives Z
   - Winner receives W
   - Platform receives P

2. Verify assertions exist for exact amounts (e.g., treasury share, provider share, winner payout) and note when they do not.

3. Confirm conservation checks are present so inputs equal outputs; flag any missing balance reconciliation.

4. Look for coverage of stress inputs:
   - 1 wei amounts (extreme precision test)
   - Prime numbers (catches rounding errors)
   - Maximum uint256 values (overflow test)

5. Expect zero-tolerance comparisons to the wei; if approximations appear, challenge them.

Example Issues:
🚩 Test checks "balance increased" but not by how much
🚩 Test checks winner got prize but doesn't check if provider got leftover
🚩 Test checks one balance but not others (tokens could leak)
🚩 Test uses approximate comparisons (assertApproxEqAbs) for exact operations
```

---

### **8️⃣ ATTACK VECTOR TEST VALIDATION**

**Audit if exploit tests actually test exploits:**

**Exploit Test Audit Questions**:

```
❓ Does this test actually attempt the exploit or just test defense exists?
❓ Is this exploit realistic or theoretical?
❓ Does this test verify exploit FAILS or verify exploit is PREVENTED?
❓ Could there be variations of this exploit the test doesn't cover?
❓ Is this test creative enough or just checking obvious attacks?
```

**Attack Test Depth Audit**:

```
For EVERY security test:

1. What attack is being tested?
2. Is the attack attempted from all possible angles?
3. Is the test verifying attack fails OR verifying why it fails?
4. Are there related attacks that should also be tested?

Example: Reentrancy Test Audit

Current test might only check:
- Direct reentrancy via receive()

Missing related attacks:
- Reentrancy via fallback()
- Cross-function reentrancy
- Cross-contract reentrancy
- Read-only reentrancy
- Reentrancy via callback
- Reentrancy with multiple attackers

Reviewer action: Flag missing coverage until the suite addresses all relevant reentrancy vectors
```

**Adversarial Creativity Audit**:

```
❓ Are we testing obvious attacks or creative attacks?
❓ Are we testing single attacks or combined attacks?
❓ Are we testing from one attacker perspective or multiple?

Audit: Read each exploit test and ask:
"If I were an attacker with unlimited time and resources,
 what would I try that this test doesn't cover?"

Common gaps:
- Tests check access control but not access control bypass
- Tests check one revert reason but not alternative attack paths
- Tests check single malicious input but not combined malicious inputs
- Tests check atomic attack but not multi-transaction attack sequence
```

---

### **9️⃣ TEST ISOLATION & INDEPENDENCE AUDIT**

**Verify tests don't depend on each other:**

**Test Independence Questions**:

```
❓ Does this test depend on another test running first?
❓ Does this test modify global state that affects other tests?
❓ Can tests run in any order without failing?
❓ Does this test make assumptions about previous tests?
❓ Are we accidentally relying on test execution order?
```

**Test Isolation Audit Protocol**:

```
For EVERY test file:

1. Run tests in random order - do they all pass?
2. Run single test in isolation - does it pass?
3. Run tests in reverse order - do they pass?
4. Check for shared state between tests
5. Verify setUp() fully resets state

Red flags:
🚩 Test assumes contract is already deployed from previous test
🚩 Test assumes certain addresses have tokens from previous test
🚩 Test assumes global time has been advanced from previous test
🚩 Test depends on specific nonce/transaction count
🚩 Tests pass when run together but fail when run alone
```

---

### **🔟 TEST DOCUMENTATION AUDIT**

**Verify test intentions are clear:**

**Documentation Completeness Questions**:

```
❓ Can someone else understand what this test is testing?
❓ Is test name descriptive enough?
❓ Are test comments explaining WHY not just WHAT?
❓ Are edge case tests documented why that edge case matters?
❓ If test is complex, is there diagram or explanation?
```

**Test Documentation Standards**

- Low-quality tests leave intent ambiguous, omit reasoning, or simply restate the code.
- High-quality tests describe the business rule, risk, and relevant specification reference in plain language.
- Reviewers should highlight tests lacking purpose-driven explanations and champion those that articulate “why this matters.”

---

### **1️⃣1️⃣ REALISTIC SCENARIO TEST AUDIT**

**Verify tests match production reality:**

**Production Realism Questions**:

```
❓ Do test scenarios match how system will actually be used?
❓ Are test parameters realistic or overly simplified?
❓ Do tests account for production constraints?
❓ Are tests using realistic gas limits?
❓ Are tests using realistic timing?
❓ Are tests using realistic token amounts?
```

**Realism Audit Checklist**:

```
For EVERY test scenario:

1. Compare test parameters to production specifications
2. Verify test doesn't use magic values that wouldn't exist in production
3. Check if test uses realistic user behavior patterns
4. Verify test accounts for network conditions (gas, timing, reorgs)

Example Issues:
🚩 Test uses address(1) as user instead of realistic EOA
🚩 Test uses perfect round numbers (1000 tokens) instead of realistic amounts
🚩 Test assumes instant mining instead of realistic block times
🚩 Test uses unlimited gas instead of realistic gas limits
🚩 Test assumes perfect timing instead of realistic user behavior
- 🚩 Test uses single user instead of concurrent users

Reviewer action: Flag scenarios that diverge materially from production realities and request that suite owners align them
```

---

### **1️⃣2️⃣ REGRESSION TEST COVERAGE**

**Verify past bugs can't return:**

**Regression Audit Questions**:

```
❓ For each bug we've found and fixed, is there a test that would catch it?
❓ Are regression tests clearly marked as such?
❓ Do regression tests document what bug they prevent?
❓ Have regression tests been added for every bug found?
```

- Confirm every historical bug has an accompanying regression test maintained by the team.
- Check that each regression test is clearly labeled, references the original issue, and still fails if the bug resurfaces.
- When coverage is missing, document the gap and assign remediation to the maintainers—do not author the regression during review.

---

### **1️⃣3️⃣ META-ANALYSIS: AUDITING THE TEST SUITE AS A WHOLE**

**Step back and evaluate entire test suite:**

**Test Suite Quality Questions**:

```
❓ What's the overall test-to-code ratio?
❓ Are there contract areas with much less test coverage than others?
❓ Are there test files that are much smaller/larger than they should be?
❓ Is test organization logical and consistent?
❓ Are there patterns of missing tests across multiple areas?
```

**Test Suite Metrics Audit**:

```
Calculate and evaluate:

1. Coverage Metrics:
   - Line coverage: Should be 100%
   - Branch coverage: Should be 100%
   - Function coverage: Should be 100%

   If any < 100%: WHY? What's not being tested?

2. Test Distribution:
   - # of tests per contract function
   - # of tests per category
   - # of positive tests vs negative tests
   - # of unit tests vs integration tests

   Look for imbalances that indicate gaps

3. Test Complexity:
   - Lines of test code vs lines of contract code
   - Should be ~3:1 to 10:1 ratio (tests should be more code)
   - If ratio is low: Tests might be too simple

4. Assertion Density:
   - Average assertions per test
   - Should be 3-10 assertions per test
   - If less: Tests might not be thorough enough
```

---

## 🎯 **CRITICAL TEST AUDIT PROTOCOL**

### **Phase 1: AUDIT EVERY TEST**

- Read the test name and compare it with the implemented logic; note gaps between claim and reality.
- Evaluate whether assertions substantiate the stated goal or leave holes.
- Record missing verifications, lingering false positives/negatives, and unclear intent for follow-up.

### **Phase 2: CHALLENGE TEST ASSERTIONS**

- For each assertion, ask whether it truly proves the intended behavior.
- Consider scenarios where the contract is broken yet the assertion could still pass; document those risks.
- Ensure the assertion targets the correct value with sufficient specificity; otherwise flag it.

### **Phase 3: HUNT FOR COVERAGE GAPS**

- Review coverage reports and identify every line or branch below target thresholds.
- Determine whether uncovered code is unreachable; if not, log the gap with context.
- Escalate reachable uncovered paths instead of silently accepting them.

### **Phase 4: VERIFY FINANCIAL TESTS**

- Trace all token flows in each financial scenario and confirm assertions exist for every participant.
- Verify that tests demonstrate conservation of value (sum in = sum out) and scrutinize any approximation logic.
- Ensure boundary inputs (1 wei, primes, max values) are exercised; if absent, document the omission.

### **Phase 5: SIMULATE REAL ATTACKS**

- Enumerate known attack vectors and confirm the suite attempts each one.
- Check that variations of the same attack (ordering, timing, multi-actor) are represented.
- When an angle is missing or insufficiently challenged, record it as an action item for the owning team.

---

## 🚨 **TEST QUALITY RED FLAGS**

### **Immediate Test Audit Triggers:**

1. **Test name doesn't match test code**
2. **Test has only 1 assertion (probably incomplete)**
3. **Test uses assertTrue instead of assertEq (too vague)**
4. **Test has no comments explaining edge cases**
5. **Test uses vm.expectRevert() without specific error**
6. **Test doesn't verify all state changes**
7. **Test doesn't verify all token flows**
8. **Test setup has no verification**
9. **Test uses approximations for exact calculations**
10. **Test marked as TODO or FIXME**

### **Test Suspicion Triggers:**

1. **Test always passes (might be false positive)**
2. **Test has complex setup (might have bugs)**
3. **Test uses magic numbers without explanation**
4. **Test name says "should revert" but doesn't use vm.expectRevert**
5. **Test modifies multiple states but checks only one**
6. **Test deals with money but doesn't check all balances**
7. **Test involves external call but doesn't test call failure**
8. **Test category has very few tests (incomplete coverage)**
9. **Test file much smaller than contract file (undertested)**
10. **Test has commented-out assertions (what are we not checking?)**
11. **Test is marked skip/TODO/FIXME (coverage gap hiding in plain sight)**

### **Specification Alignment Red Flags:**

1. Test expects behavior that contradicts the specification or product requirements
2. Test encodes economically illogical or unfair outcomes
3. Test expectations create an exploitable scenario by design
4. Test depends on unexplained magic numbers instead of documented values
5. Test hardcodes a specific address/value when any address/value should be accepted

---

## 📊 **TEST QUALITY METRICS**

### **Minimum Test Quality Standards:**

```
Coverage Requirements:
- Line Coverage: 100%
- Branch Coverage: 100%
- Function Coverage: 100%
- Event Coverage: 100%

Test Completeness:
- Every function: ≥5 tests (happy path + 4 edge cases)
- Every require/revert: ≥1 test triggering it
- Every state transition: ≥1 test verifying it
- Every external call: ≥2 tests (success + failure)
- Every token transfer: ≥3 tests (amount, source, destination)

Test Quality:
- Average assertions per test: ≥3
- Tests with explanatory comments: 100%
- Tests with specific error checking: 100%
- Regression tests for found bugs: 100%
- Financial tests with exact assertions: 100%
```

---

## ✅ **FINAL TEST AUDIT CHECKLIST**

**Before declaring test suite "ready":**

- [ ] Every test name accurately describes what test does
- [ ] Every test has been audited for false positives
- [ ] Every test has been audited for false negatives
- [ ] Every assertion verified to test the right thing
- [ ] Every contract function has comprehensive test coverage
- [ ] Every revert path has a test triggering it
- [ ] Every state transition has a test verifying it
- [ ] Every financial operation has exact amount assertions
- [ ] Every attack vector has been tested from all angles
- [ ] Every edge case has been tested
- [ ] 100% line/branch/function coverage achieved
- [ ] All tests are independent and can run in any order
- [ ] All tests have clear documentation
- [ ] All tests use realistic scenarios
- [ ] All found bugs have regression tests
- [ ] No commented-out tests or assertions
- [ ] No TODOs or FIXMEs in test code
- [ ] Test suite quality metrics meet minimum standards

---

## 🎯 **REMEMBER: AUDIT THE TESTS, NOT JUST THE CONTRACT**

**The Critical Questions:**

- ✅ "Do these tests actually prove the contract is secure?"
- ✅ "Could the contract be broken even with all tests passing?"
- ✅ "Am I testing what I THINK I'm testing?"
- ✅ "What am I NOT testing that I should be?"
- ✅ "Can these tests give me false confidence?"

**The Hard Truth:**

- Tests are code too - they can have bugs
- Passing tests don't guarantee correct contract
- Incomplete tests are worse than no tests (false confidence)
- Your job is to find gaps in your own tests
- **Test the tests as rigorously as you test the contract**

---

**FINAL PRINCIPLE:**

## **"Audit every test like someone else wrote it and you're trying to find what they missed."**

**Because in production, user funds depend on test quality.**

---

# 📋 COMPREHENSIVE VALIDATION CATEGORIES

## 1️⃣ GAME MECHANICS INTEGRITY

### Questions to Ask:

**Core Game Rules**:

- Can a player win without actually matching the winning number?
- Can a player lose even when they matched the winning number?
- Can the game end in an undefined state (not won, not lost, not refunded)?
- Can the game continue after it should have ended?
- Can the game be stuck in a state where no one can proceed?

**Pick Number Mechanics**:

- Can a player submit pick number 0?
- Can a player submit pick number > pickRange?
- Can a player submit negative numbers (if using signed integers)?
- Can a player submit the same pick number multiple times in one transaction?
- Can multiple players submit the same pick number?
- What happens if pickRange = 0?
- What happens if pickRange = 1? (guaranteed win?)
- What happens if pickRange = type(uint256).max?

**Winning Logic**:

- Is the winning number selection truly random?
- Can the winning number be predicted before VRF callback?
- Can the winning number be manipulated by provider?
- Can the winning number be manipulated by player?
- Can the winning number be manipulated by platform?
- What if VRF returns the same random value twice?
- What if VRF returns 0?
- What if VRF returns a value outside pickRange?

**Multiple winners scenario (documented behavior)**:

- Contract supports only one winner per lottery; the first matching pick wins.
- Confirm how the system handles purchases made after a winner is recorded (refunds, events, state transitions).
- Prize payouts are auto-transferred—no manual claim path exists, so double-claim or stolen-claim attempts must be impossible by design.

**No winner scenario**:

- What happens if no one picks the winning number?
- Does the prize revert to the provider, and under what conditions?
- Can the provider claim the prize before the lottery ends or while VRF requests are pending?
- What occurs when the lottery expires with pending tickets?

**VRF timeout scenario**:

- Can anyone expire a pending VRF request after the one-hour timeout?
- Is the player refunded correctly when VRF times out?
- Can a pending request be expired before the timeout hits?
- What happens if the VRF callback arrives after the timeout expiration?

**Test Protocol**:

```
For EVERY game mechanic:
1. Test the intended behavior
2. Test the opposite of intended behavior
3. Test boundary conditions (0, 1, max)
4. Test impossible states (can we create them?)
5. Test state transitions (can we skip states?)
6. Test race conditions (simultaneous actions)
7. Test order dependencies (does order matter?)
```

---

## 2️⃣ HEALTH & LIVENESS

### Questions to Ask:

**Contract Liveness**:

- Can the contract become permanently stuck?
- Can funds become permanently locked?
- Can a lottery become un-finalizable?
- Can pending requests become un-expirable?
- Can the contract run out of gas in critical operations?

**Recovery Mechanisms**:

- If VRF fails, can players get refunds?
- If VRF times out, can anyone trigger recovery?
- If provider disappears, can players still claim?
- If platform is compromised, can users still withdraw?
- Are there any single points of failure?

**Deadlock Scenarios**:

- Can we create a lottery that can never be finalized?
- Can we create a pending request that can never be resolved?
- Can we create a state where no function can be called?
- Can we create circular dependencies?

**Griefing Attacks**:

- Can someone prevent others from buying tickets?
- Can someone prevent lottery from finalizing?
- Can someone prevent prize claims?
- Can someone spam the contract to make it unusable?
- Can someone drain gas from legitimate users?

**Test Protocol**:

```
For EVERY critical operation:
1. Test normal execution
2. Test execution when dependencies fail
3. Test execution when external calls revert
4. Test execution with maximum gas consumption
5. Test execution with minimum gas
6. Test recovery from failure states
7. Test multiple recovery attempts
```

---

## 3️⃣ ECONOMIC GAME THEORY

### Questions to Ask:

**Incentive Alignment**:

- Is it profitable for provider to create unfair lotteries?
- Is it profitable for players to collude?
- Is it profitable for platform to manipulate outcomes?
- Are there perverse incentives we haven't considered?
- Can rational actors break the system while following rules?

**Economic exploits**:

- Document that no cancel function exists—attacks relying on instant cancelation should be impossible; verify this invariant.
- Document that there is no manual refund path—focus refund abuse tests on VRF timeout recovery.
- Can someone profit by manipulating gas prices?
- Can someone profit by front-running transactions?
- Can someone profit by back-running transactions?
- Can someone profit by sandwich attacks?

**Value Extraction**:

- Can provider extract more value than intended?
- Can player extract more value than intended?
- Can platform extract more value than intended?
- Can MEV bots extract value from users?
- Are there hidden costs users don't see?

**Market Manipulation**:

- Can someone manipulate ticket prices?
- Can someone manipulate prize amounts?
- Can someone manipulate odds?
- Can someone create artificial scarcity?
- Can someone create artificial demand?

**Sybil Attacks**:

- Can one person create multiple identities to gain advantage?
- Can one person buy all tickets to guarantee win?
- Can one person spam cheap lotteries to drain platform?
- Does buying more tickets give unfair advantage beyond odds?

**Test Protocol**:

```
For EVERY economic interaction:
1. Calculate expected value for each party
2. Find scenarios where EV is negative for honest users
3. Find scenarios where EV is positive for attackers
4. Test collusion between multiple parties
5. Test front-running opportunities
6. Test arbitrage opportunities
7. Test value extraction mechanisms
```

---

## 4️⃣ FINANCIAL OPERATIONS INTEGRITY

### Questions to Ask:

**Token Flow Accuracy**:

- Does every token that enters the contract get accounted for?
- Does every token that leaves the contract get accounted for?
- Can tokens disappear (go to address(0))?
- Can tokens be created out of thin air?
- Can the contract's token balance become negative?
- Can the contract's token balance overflow?

**Calculation Precision**:

- Are all divisions rounded correctly?
- Can rounding errors accumulate?
- Can rounding errors be exploited?
- Are there any integer overflow/underflow risks?
- Are percentages calculated correctly (95% vs 0.95)?
- Do calculations work with very small amounts (1 wei)?
- Do calculations work with very large amounts (type(uint256).max)?

**Fee Distribution**:

- Does platform fee always equal exactly 5%?
- Does provider revenue always equal exactly 95%?
- Can fees be manipulated to be 0%?
- Can fees be manipulated to be >100%?
- What happens if ticket price < platform fee?
- What happens if prize amount < ticket price?

**Prize distribution**:

- Is the exact prize amount transferred to the winner?
- Can the prize be transferred to the wrong address?
- Prize transfers are atomic and auto-executed—verify there is no path for duplicate or partial transfers.
- What if prize transfer fails?
- What if the winner is a contract that rejects transfers?

**Refund accuracy**:

- Are refunds calculated correctly?
- Auto-refunds go directly to the original player and run once—confirm there is no double-refund or stolen-refund scenario.
- What if refund amount > player's original payment?
- What if refund amount < player's original payment?

**Failed ETH refund recovery**:

- If excess ETH refund fails, is it recorded in `failedRefunds`?
- Can the rightful player withdraw failed refunds via `withdrawFailedRefund()`?
- Can someone else withdraw another player's failed refund?
- Can the same failed refund be withdrawn multiple times?
- What happens if a player without a failed refund calls `withdrawFailedRefund()`?

**Balance Reconciliation**:

- After every operation, does sum of all balances equal total supply?
- Can we create a state where contract owes more than it has?
- Can we create a state where contract has more than it should?
- Are there any scenarios where tokens are lost?
- Are there any scenarios where tokens are duplicated?

**Test Protocol**:

```
For EVERY financial operation:
1. Track EXACT token amounts before operation
2. Perform operation
3. Track EXACT token amounts after operation
4. Verify: (total_in - total_out) = expected_change
5. Verify: contract_balance = sum_of_all_claims
6. Test with 1 wei amounts
7. Test with max uint256 amounts
8. Test with prime numbers (to catch rounding)
9. Test with amounts that cause rounding
10. Verify NO tokens are lost or created
```

---

## 5️⃣ FAIRNESS & RANDOMNESS

### Questions to Ask:

**True Randomness**:

- Is the random number truly unpredictable?
- Can provider predict the random number?
- Can player predict the random number?
- Can miner/validator predict the random number?
- Can anyone influence the random number?

**VRF Integrity**:

- ✅ KEEP: Can VRF callback be called by unauthorized party?
- ✅ KEEP: Can VRF callback be called multiple times?
- ✅ KEEP: Can VRF callback be called with wrong parameters?
- ✅ KEEP: Can VRF callback be front-run?
- ✅ KEEP: Can VRF callback be delayed indefinitely?
- ✅ KEEP: What if VRF provider is compromised?

**VRF Provider Validation** (NEW):

- ✅ ADD: Does validEntropy modifier prevent calls with wrong entropy provider?
- ✅ ADD: Can entropy provider address be changed after deployment?
- ✅ ADD: What if entropy provider address is address(0)?
- ✅ ADD: Can VRF callback come from different provider than expected?

**Outcome Manipulation**:

- Can provider choose who wins?
- Can player increase their odds beyond ticket count?
- Can platform favor certain outcomes?
- Can timing of actions affect outcomes?
- Can order of transactions affect outcomes?

**Bias Detection**:

- Over 1000 lotteries, is distribution truly random?
- Are certain numbers more likely to win?
- Are certain players more likely to win?
- Are certain times more likely to produce wins?
- Is there any detectable pattern?

**Test Protocol**:

```
For randomness validation:
1. Run 1000+ lottery simulations
2. Record all winning numbers
3. Perform chi-square test for uniform distribution
4. Test for sequential patterns
5. Test for temporal patterns
6. Test for player-based patterns
7. Verify: each number has ~equal probability
8. Verify: no predictable patterns exist
```

---

## 6️⃣ LOGIC GAPS & EDGE CASES

### Questions to Ask:

**Undefined Behavior**:

- What happens if function is called in wrong order?
- What happens if function is called twice?
- What happens if function is never called?
- What happens if prerequisites are not met?
- What happens if postconditions are not verified?

**State Inconsistencies**:

- Can lottery be ACTIVE and COMPLETED simultaneously?
- Can player have tickets but lottery has no record?
- Can lottery have tickets but players have no record?
- Can pending requests exist for completed lottery?
- Can completed lottery have non-zero prize?

**Missing Validations**:

- Are all inputs validated?
- Are all state transitions validated?
- Are all external calls validated?
- Are all return values checked?
- Are all assumptions verified?

**Boundary Conditions**:

- What happens at exactly duration = 0?
- What happens at exactly block.timestamp = endTime?
- What happens at exactly block.timestamp = endTime + 1?
- What happens with exactly 0 tickets sold?
- What happens with exactly 1 ticket sold?
- What happens with exactly max tickets sold?

**Time-Based Logic**:

- What if block.timestamp goes backwards (chain reorg)?
- What if block.timestamp jumps forward (validator manipulation)?
- What if duration overflows?
- What if endTime is in the past?
- What if endTime is very far in future?

**Test Protocol**:

```
For EVERY function:
1. Call with valid inputs → expect success
2. Call with invalid inputs → expect specific revert
3. Call in wrong state → expect specific revert
4. Call in wrong order → expect specific revert
5. Call twice → expect specific behavior
6. Call never → verify system still works
7. Call with boundary values → verify exact behavior
8. Call with overflow values → expect revert
9. Call with underflow values → expect revert
10. Call with all combinations of edge cases
```

---

## 7️⃣ DEAD PATHS & UNREACHABLE CODE

### Questions to Ask:

**Unreachable States**:

- Are there states that can never be reached?
- Are there states that can be reached but never exited?
- Are there code paths that can never execute?
- Are there conditions that can never be true?
- Are there conditions that can never be false?

**Dead Code Detection**:

- Is every line of code reachable?
- Is every branch reachable?
- Is every error condition reachable?
- Is every event emission reachable?
- Is every state transition reachable?

**Impossible Conditions**:

- Are there checks that can never fail?
- Are there checks that can never pass?
- Are there redundant validations?
- Are there contradictory requirements?

**Test Protocol**:

```
For code coverage:
1. Achieve 100% line coverage
2. Achieve 100% branch coverage
3. Achieve 100% condition coverage
4. Identify any unreachable code
5. Either make it reachable or remove it
6. Verify every error can be triggered
7. Verify every event can be emitted
```

---

## 8️⃣ BAD USAGE & MISUSE SCENARIOS

### Questions to Ask:

**User Errors**:

- What if user sends ETH instead of tokens?
- What if user approves wrong amount?
- What if user approves wrong contract?
- What if user calls wrong function?
- What if user provides wrong parameters?
- Can user accidentally lock their funds?
- Can user accidentally burn their tokens?

**Integration Errors**:

- What if frontend sends wrong data?
- What if frontend calculates wrong amounts?
- What if frontend shows wrong information?
- What if API returns stale data?
- What if subgraph is out of sync?

**Contract Interaction Errors**:

- What if called by another contract?
- What if called via delegatecall?
- What if called via multicall?
- What if called in a batch transaction?
- What if called with insufficient gas?

**Recovery from Errors**:

- Can user recover from mistakes?
- Can provider recover from mistakes?
- Can platform recover from mistakes?
- Are there any irreversible actions?
- Are there any permanent losses?

**Test Protocol**:

```
For user protection:
1. Test every possible user mistake
2. Verify appropriate error messages
3. Verify funds are never lost
4. Verify recovery mechanisms work
5. Test with minimal gas
6. Test with contract callers
7. Test with unusual call patterns
```

---

## 9️⃣ ABUSE & EXPLOITATION VECTORS

### Questions to Ask:

**Provider abuse**:

- Can provider create unfair lotteries?
- There is no cancel function after launch—tests should verify the provider cannot exit after seeing tickets.
- Can provider delay finalization to avoid paying?
- Can provider manipulate odds?
- Can provider steal player funds?
- Can provider collude with players?

**Tier-based pick range validation**:

- Can provider create Tier 1 lotteries (≤100k USDC) with pickRange > 1× prize?
- Can provider create Tier 2 lotteries (100k–1M USDC) with pickRange > 2× prize?
- Can provider create Tier 3 lotteries (≥1M USDC) with pickRange > 3× prize?
- Can provider manipulate tier boundaries to bypass pick range limits?
- What happens exactly at the tier boundaries (100k, 1M)?

**Player abuse**:

- Can a player buy tickets after learning the outcome?
- Refunds are automatic via VRF timeout; confirm there is no manual refund abuse loop.
- There is no manual prize claim—ensure players cannot redirect prizes they did not win.
- Can players manipulate odds or grief others?
- Can players collude with the provider?

**Referral parameter validation** (ProductionLottery only):

- Can a player set themselves as referrer in `buyTicketsPacked()`?
- What if the referrer address is `address(0)` in `buyTicketsPacked()`?
- Does `buyTicketsPackedNoReferral()` behave correctly without a referrer?
- Can packed data be manipulated to forge referral parameters?
- Does the lottery pass referral data correctly to the external ReferralManager?
- ⚠️ ReferralManager contract will be tested separately; here we validate the integration points.

**Platform Abuse**:

- Can platform steal funds?
- Can platform manipulate outcomes?
- Can platform censor transactions?
- Can platform favor certain users?
- Can platform extract excessive fees?

**Treasury Contract Validation** (NEW):

- ✅ ADD: Does Treasury receive exactly 5% platform fee?
- ✅ ADD: Can Treasury address be address(0)?
- ✅ ADD: Can Treasury address be changed mid-lottery?
- ✅ ADD: What if Treasury transfer fails?
- ✅ ADD: Can platform fee be manipulated to be != 5%?
- ✅ ADD: Can Treasury drain lottery contract?

**MEV Exploitation**:

- Can MEV bots front-run ticket purchases?
- Can MEV bots back-run VRF callbacks?
- Can MEV bots sandwich user transactions?
- Can MEV bots extract value from users?
- Can MEV bots manipulate outcomes?

**Flash Loan Attacks**:

- Can someone use flash loans to manipulate?
- Can someone use flash loans to drain funds?
- Can someone use flash loans to grief users?
- Can someone use flash loans to break assumptions?

**Reentrancy Attacks**:

- Can any function be reentered?
- Can reentrancy change state unexpectedly?
- Can reentrancy drain funds?
- Can reentrancy bypass checks?
- Are all state changes before external calls?

**Test Protocol**:

```
For security validation:
1. Attempt every known attack vector
2. Attempt novel attack combinations
3. Test with malicious contracts
4. Test with malicious parameters
5. Test with malicious timing
6. Test with malicious sequences
7. Verify all attacks fail with specific errors
8. Verify no funds can be stolen
9. Verify no state can be corrupted
10. Verify no outcomes can be manipulated
```

---

## 🔟 GAME MANIPULATION & CHEATING

### Questions to Ask:

**Odds Manipulation**:

- Can someone increase their winning odds unfairly?
- Can someone decrease others' winning odds?
- Can someone guarantee a win?
- Can someone guarantee others lose?
- Can someone manipulate the prize pool?

**Information Asymmetry**:

- Does anyone have information others don't?
- Can anyone see outcomes before others?
- Can anyone act on information before others?
- Are all players on equal footing?
- Is there any hidden information that matters?

**Timing Attacks**:

- Can someone wait to see others' picks before choosing?
- Can someone buy tickets at the last second?
- Can someone prevent others from buying tickets?
- Can someone delay VRF callback?
- Can someone rush VRF callback?

**Collusion**:

- Can multiple players collude to increase odds?
- Can player and provider collude to steal funds?
- Can player and platform collude to manipulate?
- Can multiple providers collude to control market?

**Test Protocol**:

```
For fairness validation:
1. Test all players have equal information
2. Test all players have equal opportunities
3. Test timing doesn't affect fairness
4. Test collusion doesn't provide advantage
5. Test manipulation attempts fail
6. Verify game is provably fair
```

---

## 1️⃣1️⃣ CROSS-CUTTING CONCERNS

### Access Control:

- Can unauthorized users call restricted functions?
- Can users escalate privileges?
- Can users bypass access checks?
- Are all roles properly enforced?
- Can roles be manipulated?

### Upgradability:

- If contract is upgradable, can upgrade be malicious?
- Can upgrade steal funds?
- Can upgrade change rules mid-game?
- Can upgrade be front-run?
- Is there upgrade governance?

### Oracle Dependency:

- What if oracle fails?
- What if oracle is compromised?
- What if oracle returns wrong data?
- What if oracle is too slow?
- What if oracle is too expensive?

### Gas Optimization Risks:

- Do gas optimizations introduce bugs?
- Do gas optimizations reduce security?
- Do gas optimizations reduce readability?
- Can gas optimizations be exploited?

### External Dependencies:

- What if the ERC20 prize token is malicious or has non-standard behavior?
- What if the VRF provider (Pyth Entropy) is malicious or offline?
- What if the Treasury contract is malicious or rejects transfers?
- MockToken is only for local testing—confirm production scenarios rely on real tokens with realistic behaviors.

### Chain Validation:

- Does the `validChain` modifier prevent cross-chain replay attacks?
- Can the contract be called on the wrong chain ID?
- What if `DEPLOYMENT_CHAIN_ID` does not match `block.chainid`?
- Can chain ID be manipulated or spoofed?

### Pending Request Management:

- Can the lottery be finalized while pending VRF requests exist?
- Is the `pendingRequests` counter incremented/decremented correctly?
- Can the pending request counter overflow or underflow?
- What if `getPendingRequestCount()` returns the wrong value?
- Can pending requests become orphaned (never processed)?

### EIP-2612 Permit Integration:

- Can permit be used with `deadline = 0` (traditional allowance)?
- Can a permit signature be replayed?
- Can permit be used for the wrong spender?
- Can permit be used with an insufficient amount?
- What happens when the permit signature is invalid?
- Can an expired permit be used?

---

# 🎯 CRITICAL TESTING PROTOCOL

## Phase 1: ASSUME FAILURE

- Approach every test with the mindset that it should fail unless it convincingly proves otherwise.
- Review evidence that the suite actively attempts to break the target behavior.
- When a bug is revealed, ensure it is documented and tracked; do not rely on the suite to conceal it.
- When no failure is demonstrated, challenge whether the attempts were rigorous enough before accepting the test as adequate.

## Phase 2: ADVERSARIAL MINDSET

- For each contract capability, brainstorm realistic exploit paths an attacker would pursue.
- Confirm that corresponding tests attempt those attacks or document the absence.
- Evaluate whether the suite differentiates between defense-by-design and accidental pass.
- Escalate uncovered attack angles to maintainers instead of silently accepting them.

## Phase 3: FINANCIAL PARANOIA

- Inspect financial tests to verify they track exact token flows, balances, and conservation.
- Look for proofs that inputs equal outputs, with zero tolerance for unexplained wei differences.
- Flag any scenario where tokens could be created, destroyed, leaked, or stolen without detection.

## Phase 4: GAME THEORY ANALYSIS

- Analyze tests for evidence that honest users retain non-negative expected value while attackers cannot profit unfairly.
- Ensure scenarios account for strategic behavior, collusion, and incentive misalignment.
- Record any economic edge cases the suite ignores and assign them for follow-up.

## Phase 5: CHAOS ENGINEERING

- Review whether critical paths are exercised under varied timing, ordering, gas limits, and failure modes.
- Look for fuzzing, randomized inputs, and stress tests that mimic production chaos.
- When randomness or disorder is absent, label the coverage gap explicitly.

---

# 🚨 RED FLAGS THAT DEMAND INVESTIGATION

## Immediate Failure Triggers:

1. **ANY token amount mismatch** (even 1 wei)
2. **ANY state inconsistency**
3. **ANY undefined behavior**
4. **ANY unreachable code**
5. **ANY missing validation**
6. **ANY unchecked external call**
7. **ANY unchecked return value**
8. **ANY assumption without verification**
9. **ANY "this should never happen" comment**
10. **ANY "TODO" or "FIXME" in production code**

## Suspicion Triggers:

1. Complex calculations (high risk of errors)
2. Multiple external calls (high risk of reentrancy)
3. Time-dependent logic (high risk of manipulation)
4. Admin functions (high risk of centralization)
5. Upgradable contracts (high risk of rug pull)
6. Gas optimizations (high risk of bugs)
7. Assembly code (high risk of vulnerabilities)
8. Unchecked math (high risk of overflow)
9. Delegate calls (high risk of storage collision)
10. Self-destruct (high risk of fund loss)

---

# 📊 VALIDATION METRICS

## Test Quality Metrics:

**Coverage Metrics** (Minimum Requirements):

- Line Coverage: 100%
- Branch Coverage: 100%
- Function Coverage: 100%
- Statement Coverage: 100%

**Adversarial Metrics** (Minimum Requirements):

- Attack Vectors Tested: 100+ unique attacks
- Exploit Attempts: All known exploits attempted
- Fuzzing Runs: 10,000+ random inputs per function
- Edge Cases: All boundary conditions tested
- Financial Accuracy: 0 wei discrepancy tolerance

**Fairness Metrics** (Minimum Requirements):

- Randomness Tests: 1,000+ simulations
- Distribution Tests: Chi-square p-value > 0.05
- Bias Tests: No detectable patterns
- Manipulation Tests: All manipulation attempts fail

---

# ✅ FINAL VALIDATION CHECKLIST

Before declaring contract "ready for production":

- [ ] Every function has been attacked from every angle
- [ ] Every financial calculation verified to the wei
- [ ] Every state transition tested for consistency
- [ ] Every edge case has been explored
- [ ] Every assumption has been challenged
- [ ] Every "impossible" scenario has been attempted
- [ ] Every economic incentive has been analyzed
- [ ] Every game mechanic has been stress-tested
- [ ] Every fairness claim has been proven
- [ ] Every security claim has been validated
- [ ] 100% code coverage achieved
- [ ] 0 wei financial discrepancies
- [ ] 0 exploitable vulnerabilities found
- [ ] 0 unfair advantages discovered
- [ ] 0 ways to lose user funds found

**Only when ALL boxes are checked AND we've exhausted all attempts to break the contract → Consider it production-ready**

---

# 🧭 Mantras & Final Reminder

**Critical mantras**

1. **Trust, but verify** — accept nothing without evidence.
2. **Test the spec, not the bug** — never legitimize incorrect behavior.
3. **Contracts lie, specifications don't** — treat the spec as the single source of truth.
4. **Evidence or it didn't happen** — every assertion needs a proof trail.
5. **Find bugs while testing, not after deployment** — production is the most expensive auditor.
6. **Question everything** — assumptions, specs, code, even prior tests.
7. **Document, escalate, resolve** — transparency and follow-through prevent repeats.

**You are the last line of defense**

- Developers make mistakes, specifications have gaps, contracts ship with bugs, and tests can encode false confidence.
- Your responsibility is to catch all of it before user funds are at risk.
- Critical thinking + evidence-based validation + an adversarial mindset keep the protocol truthful.

**Bottom line**

- We are not here to be nice to the contract.
- We are here to protect users' money.
- Every bug we miss could cost someone real money.
- Test like your own money is at stake—because in production, it will be.
