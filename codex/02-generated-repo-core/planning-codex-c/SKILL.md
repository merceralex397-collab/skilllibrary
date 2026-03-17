---
name: planning-codex-c
description: Turn an objective, ticket, bug report, or vague request into a bounded execution plan with dependencies, unknowns, validation, and stop conditions before implementation starts. Use when the user asks for a plan, design, or implementation approach, or when a task is large enough that coding immediately would create avoidable rework. Do not use for trivial one-file edits or for reviewing an already-written plan.
---

# Purpose

Use this skill to produce a plan that is executable rather than aspirational.

# When to use this skill

Use this skill when:

- the user asks for a plan
- the task spans multiple files, systems, or stages
- the objective is clear but the execution path is not
- the risk of rework is high if coding starts immediately

# Do not use this skill when

- the task is tiny and the next step is obvious
- a real plan already exists and needs critique; use plan review
- the main issue is choosing between explicit alternatives; use tradeoff analysis

# Operating procedure

1. Frame the objective.
   Restate the goal, constraints, and what success must produce.

2. Find the unknowns.
   Separate known facts from assumptions and blockers.

3. Break the work into stages.
   Use stages that can each be verified. Avoid giant single-step plans.

4. Attach receipts.
   Every stage should end with evidence: tests, logs, file outputs, screenshots, or explicit review.

5. Surface blockers early.
   Do not bury missing access, missing data, or unclear requirements in later steps.

6. End with the next concrete action.

# Decision rules

- Prefer plans that can fail early and cheaply.
- Prefer vertical slices over horizontal churn when the task is uncertain.
- Keep stages dependency-aware.
- If a stage has no proof, it is not complete.

# Output requirements

Return:

1. `Goal`
2. `Constraints`
3. `Unknowns and Blockers`
4. `Execution Plan`
5. `Validation`

# References

- Read `references/stage-design.md` for good stage boundaries.
- Read `references/blocker-types.md` to classify unknowns.
- Read `references/receipt-examples.md` for proof patterns.

# Failure handling

- If the goal is underspecified, stop after framing and list the missing decisions.
- If the work is too large, split the plan into a first milestone and defer later milestones.
