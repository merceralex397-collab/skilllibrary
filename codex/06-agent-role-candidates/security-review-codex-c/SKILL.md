---
name: security-review-codex-c
description: Reviews trust boundaries, secrets handling, validation, and attack surfaces in a read-only lane. Use this when the user or repo work clearly points at security review or a task in the "Agent Role Candidate Skills" family needs repeatable procedure rather than ad hoc prompting. Do not use for tasks outside the narrow security review role.
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: codex
  domain: security-review
  maturity: draft
  risk: low
  tags: [security, review]
---

# Purpose

Reviews trust boundaries, secrets handling, validation, and attack surfaces in a read-only lane.

# When to use this skill

Use this skill when:

- the user or repo work clearly points at security review
- a task in the "Agent Role Candidate Skills" family needs repeatable procedure rather than ad hoc prompting
- a plan, ticket, or repo state would benefit from explicit guardrails around security review

# Do not use this skill when

- the task is really about tasks outside the narrow security review role
- If the task is more specifically about `ticket-creator` or `code-review`, prefer that skill instead.
- the relevant files, runtime, or deliverable type are already covered by a more specific active skill

# Operating procedure

1. Restate the narrow role this skill is allowed to perform for Security Review.
2. Collect evidence only from the files, logs, or artifacts that affect that role.
3. Produce the role-specific output without drifting into adjacent responsibilities.
4. Call out handoff gaps early when another role must act before this one can continue.
5. Return concise evidence-backed conclusions, not generic commentary.

# Decision rules

- Stay inside the role boundary even when adjacent work looks tempting.
- Escalate when the role cannot complete safely without missing scope discipline.
- Hand off evidence, not just conclusions.
- Prefer a smaller accurate output over a broad vague one.

# Output requirements

1. `Assigned Scope`
2. `Evidence Collected`
3. `Recommended Action`
4. `Handoff Notes`

# References

Read these only when relevant:

- `references/handoff-contract.md`
- `references/success-criteria.md`
- `references/anti-patterns.md`

# Related skills

- `ticket-creator`
- `code-review`
- `qa-validation`
- `docs-handoff`

# Failure handling

- If the scope is ambiguous, restate the decision boundary before proceeding.
- If the evidence is weak, say so explicitly and lower confidence instead of smoothing it over.
- If the task would be better served by a narrower skill, redirect to it rather than stretching this one.
