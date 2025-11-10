# Guideline to Thought Processes — Post-Implementation Playbook

Use this checklist after the feature or fix is delivered. It ensures the work truly solved the original problem, captures lessons, and sets up next steps—closing the loop that was often missed in the three transcripts.

---

## 1. Verify the outcome

- Run the exact workflows or tests the requester cares about.
- Compare before/after behavior: did the specific pain point disappear without manual tweaks?
- Confirm no safeguards (tests, lint rules, policies) were disabled or bypassed.

## 2. Validate alignment with expectations

- Do the changes match the agreed scope, artifacts, and success criteria?
- Were any out-of-scope modifications introduced? If so, address or roll them back.

## 3. Capture residual risks and follow-ups

- List remaining uncertainties, technical debt, or decisions deferred to the requester.
- Identify monitoring, rollback plans, or future work needed to stabilize the change.
- Document assumptions that proved false so they are not repeated.

## 4. Reflect on the process

- Which steps went off-script (scope creep, unplanned tooling) and why?
- Where did communication break down (missed instructions, late questions)?
- What signals indicated risk earlier, and how can we catch them next time?

## 5. Close the loop with the requester

- Provide a concise summary focused on outcomes, validation evidence, and residual risks.
- Offer clear next actions (verify, gather feedback, schedule integration, or revert).
- Invite feedback on the approach so future collaborations start stronger.

This playbook keeps the finish line honest: it protects against premature “done” declarations and turns each engagement into a learning asset for the next one.
