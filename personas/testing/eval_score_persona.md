---
type: "agent_requested"
description: "Example description"
---

# Test Review Scoring Framework

Use this framework when auditing existing tests. It rewards thorough analysis, precise documentation, and escalation of gaps—never code changes. Treat it as a checklist for assessing quality while staying within the reviewer role defined in `protocol.md`.

---

## Scoring Overview

- Begin each review session at **0 points**.
- Evaluate the applicable checks in each phase; award yourself the listed points only when the condition is fully met.
- Record short justifications that cite the files/lines you inspected.
- **Score = (Earned Points ÷ Possible Points) × 100.**  
  Only count phases that applied to the review scope.
- Minimum passing adherence: **80 %**. Anything lower requires revisions/escalation.

---

## Phase 1 – Specification Alignment (0 – 20 pts)

| Check                   | Points | What you must confirm                                                                                 |
| ----------------------- | ------ | ----------------------------------------------------------------------------------------------------- |
| **Requirement mapping** | +10    | You verified each test links to an explicit spec/business requirement and noted any missing coverage. |
| **Ambiguity flagging**  | +10    | You identified unclear or contradictory requirements and recorded questions/escalations for owners.   |

**Penalty:** –10 if you accept a test that contradicts documented requirements without raising it.

---

## Phase 2 – Evidence & Traceability (0 – 25 pts)

| Check                         | Points | What you must confirm                                                                            |
| ----------------------------- | ------ | ------------------------------------------------------------------------------------------------ |
| **Assertion evidence audit**  | +10    | Every assertion you reviewed has an associated contract reference or invariant; gaps are logged. |
| **Edge-case acknowledgement** | +5     | Tests discuss or exercise edge conditions; missing considerations are documented.                |
| **Financial math trace**      | +10    | For monetary flows, you traced the calculation end-to-end and recorded any unexplained numbers.  |

**Penalty:** –10 if you overlook undocumented magic numbers or unverifiable assertions.

---

## Phase 3 – False-Positive Exposure (0 – 20 pts)

| Check                              | Points | What you must confirm                                                                                                     |
| ---------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------- |
| **Break-first thought experiment** | +10    | For each critical test, you described how breaking the contract should cause failure and whether the test would catch it. |
| **Mock realism review**            | +5     | You evaluated whether mocks/stubs could hide bugs and flagged any unrealistic behavior.                                   |
| **Event & side-effect coverage**   | +5     | You confirmed the test exercises and validates all observable side effects relevant to its claim.                         |

**Penalty:** –10 if you accept a test that would pass through an obvious contract break without noting it.

---

## Phase 4 – Coverage & Scope Assessment (0 – 20 pts)

| Check                        | Points | What you must confirm                                                                                 |
| ---------------------------- | ------ | ----------------------------------------------------------------------------------------------------- |
| **Domain isolation**         | +5     | Tests stay within their intended domain (lottery/ticket/treasury/etc.); mixed concerns are escalated. |
| **Unique scenario tracking** | +5     | You verified the scenario isn’t duplicated elsewhere, or documented why a duplicate exists.           |
| **Metric validation**        | +5     | You inspected coverage metrics or execution paths and flagged any reachable gaps.                     |
| **Regression backstop**      | +5     | You confirmed known bugs have regression tests; missing ones are written up as risks.                 |

**Penalty:** –10 if you miss an uncovered, reachable branch or let mixed-domain logic slide without comment.

---

## Phase 5 – Risk & Economic Integrity (0 – 15 pts)

| Check                      | Points | What you must confirm                                                                             |
| -------------------------- | ------ | ------------------------------------------------------------------------------------------------- |
| **Financial conservation** | +5     | Tests demonstrate sum-in equals sum-out for token movements; discrepancies are reported.          |
| **Incentive analysis**     | +5     | You assessed whether honest vs. adversarial actors maintain expected value according to the spec. |
| **Chaos considerations**   | +5     | You verified that timing/order/gas variations are either tested or explicitly listed as TODOs.    |

**Penalty:** –10 if you miss a financial inconsistency or unfair incentive the tests fail to expose.

---

## General Penalties

Apply one **–15** deduction per infraction if:

- You claim credit without recorded evidence (missing file/line references).
- You recommend code changes directly instead of documenting issues for the owners.
- You submit a score without calculating the final percentage.
- You approve a suite below the 80 % threshold.

---

## Reporting Template

```markdown
### Review Summary

- Scope: [files / features inspected]
- Applicable phases: [list phases used]

### Phase Scores

- Phase 1 – Specification Alignment: +\_\_/20 (notes)
- Phase 2 – Evidence & Traceability: +\_\_/25 (notes)
- Phase 3 – False-Positive Exposure: +\_\_/20 (notes)
- Phase 4 – Coverage & Scope: +\_\_/20 (notes)
- Phase 5 – Risk & Economic Integrity: +\_\_/15 (notes)
- Penalties: –\_\_ (justifications)

**Total:** Earned ** / Possible ** → \_\_ %
**Recommendation:** [Approve / Revise / Reject]
```

**Remember:** The reviewer’s job is to observe, document, and escalate. Never modify tests during the audit—use this scorecard to prove whether existing coverage deserves trust.
