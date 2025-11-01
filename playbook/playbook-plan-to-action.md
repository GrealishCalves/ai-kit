# Guideline to Thought Processes — Plan Review & Execution Playbook

Use this playbook once the AI (or teammate) delivers an initial plan. It helps you evaluate the proposal, align on scope, and steer implementation without repeating the missteps seen in the three transcripts.

---

## 1. Stress-test the plan
- Does the plan restate the original goal, scope, and non-goals accurately?
- Are there hidden assumptions or missing dependencies?
- Which alternative approaches were considered, and why was this one chosen?
- What evidence supports feasibility (prior art, docs, known limitations)?

## 2. Validate scope and impact
- Map each step back to the agreed objectives—does anything drift out of bounds?
- Identify required approvals, tooling changes, or new artifacts before work begins.
- Clarify success metrics and checkpoints for each major step.

## 3. Define guardrails for execution
- Set incremental validation checkpoints (lint, targeted tests, dry runs).
- Decide what signals should trigger a pause or escalation.
- Establish how to document unexpected findings or open questions mid-stream.

## 4. Align on collaboration
- Confirm who owns each step and which stakeholders need updates.
- Specify how to surface uncertainties during implementation (e.g., ask before adding dependencies).
- Agree on communication cadence—status pings, blockers, interim demos.

## 5. Spot red flags early
- Plan introduces new tooling/infrastructure without justification.
- Success criteria drift toward “perfect” instead of the defined target.
- Proposed demos or proof-of-concepts never touch the real workflow.

Use this guide to negotiate revisions before work starts, or as a checklist while the plan is executed. It keeps implementation anchored to the original need and prevents silent scope creep.
