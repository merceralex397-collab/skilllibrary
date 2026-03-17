---
name: linear-address-issue-codex-c
description: A tracker-specific micro-skill for resolving or updating Linear issues cleanly. Use this when the user or repo work clearly points at linear address issue or a task in the "External Reference Seed Skills" family needs repeatable procedure rather than ad hoc prompting. Do not use for blind copy-import of seed material without adapting it into a usable skill or workflow.
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: codex
  domain: linear-address-issue
  maturity: draft
  risk: low
  tags: [linear, address, issue]
---

# Purpose

A tracker-specific micro-skill for resolving or updating Linear issues cleanly.

# When to use this skill

Use this skill when:

- the user or repo work clearly points at linear address issue
- a task in the "External Reference Seed Skills" family needs repeatable procedure rather than ad hoc prompting
- a plan, ticket, or repo state would benefit from explicit guardrails around linear address issue

# Do not use this skill when

- the task is really about quick factual lookups with no synthesis, comparison, or decision support
- If the task is more specifically about `cargo-lock-manager` or `gh-address-comments`, prefer that skill instead.
- the relevant files, runtime, or deliverable type are already covered by a more specific active skill

# Operating procedure

1. Define the exact question and decision boundary for Linear Address Issue.
2. Collect grounded sources that let you compare source comparison, decision relevance, and uncertainty statements.
3. Separate observations from interpretation so the recommendation stays defensible.
4. State uncertainty, missing data, and assumptions explicitly.
5. Conclude with a recommendation that is actionable under the current evidence.

# Decision rules

- Prefer comparable sources over a larger pile of weak ones.
- Keep claims proportional to the evidence behind source comparison and decision relevance.
- Separate recommendation from raw observation.
- State what would change the decision.

# Output requirements

1. `Question`
2. `Sources`
3. `Comparison`
4. `Recommendation and Confidence`

# References

Read these only when relevant:

- `references/source-plan.md`
- `references/comparison-matrix.md`
- `references/claim-discipline.md`

# Related skills

- `cargo-lock-manager`
- `gh-address-comments`
- `frontend-webapp-builder`
- `misc-helper`

# Failure handling

- If the scope is ambiguous, restate the decision boundary before proceeding.
- If the evidence is weak, say so explicitly and lower confidence instead of smoothing it over.
- If the task would be better served by a narrower skill, redirect to it rather than stretching this one.
