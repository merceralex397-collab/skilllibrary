---
name: ticket-execution-codex-c
description: Execute ticket-based work using a deterministic stage order, proof requirements, and handoff discipline. Use when working from issues, tickets, epics, or scoped tasks where the repo or team expects a plan-first workflow with evidence before advancing. Do not use for freeform exploration with no task container.
---

# Purpose

Use this skill to keep ticket work from drifting into unbounded hacking.

# When to use this skill

Use this skill when:

- the work is scoped by a ticket or issue
- the repo expects stages or receipts for progress
- you need to decide whether a ticket is ready to move forward
- the work should produce a handoff or reviewable state

# Do not use this skill when

- there is no task container at all
- the work is purely exploratory research with no execution commitment

# Operating procedure

1. Read the ticket and attached context.
2. Confirm scope, acceptance surface, and blockers.
3. Build or confirm a plan.
4. Implement in bounded stages.
5. Validate each stage before advancing.
6. Prepare handoff or review output with residual risks.

# Stage contract

Use this flow:

1. `understand`
2. `plan`
3. `implement`
4. `validate`
5. `handoff`

Do not collapse `implement` and `validate` into one invisible step.

# Decision rules

- If scope is unclear, stay in `understand`.
- If blockers exist, do not pretend to be in `implement`.
- If validation is missing, do not mark the work complete.
- If the ticket changed materially mid-flight, return to `plan`.

# Output requirements

Return:

1. `Current Stage`
2. `Evidence`
3. `Blockers`
4. `Next Transition`
5. `Residual Risk`

# References

- Read `references/stage-definitions.md` for stage boundaries.
- Read `references/proof-requirements.md` for what counts as evidence.
- Read `references/handoff-minimum.md` for close-out expectations.

# Failure handling

- If the ticket lacks enough detail to proceed, stop in `understand` and state what is missing.
- If validation fails, return to the earlier stage that must be repaired.
- If the user interrupts with new scope, restate the new boundary before continuing.
