---
name: skill-registry-manager-codex-c
description: Owns the library catalog, metadata, status, tags, and provenance for all skills in the ecosystem. Use this when creating, adapting, refining, installing, testing, or packaging a skill or a task in the "Package Scaffolding Skills" family needs repeatable procedure rather than ad hoc prompting. Do not use for adjacent tasks where a more specific skill already covers the job.
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: codex
  domain: skill-registry-manager
  maturity: draft
  risk: low
  tags: [skill, registry, manager]
---

# Purpose

Owns the library catalog, metadata, status, tags, and provenance for all skills in the ecosystem.

# When to use this skill

Use this skill when:

- creating, adapting, refining, installing, testing, or packaging a skill
- a task in the "Package Scaffolding Skills" family needs repeatable procedure rather than ad hoc prompting
- a plan, ticket, or repo state would benefit from explicit guardrails around skill registry manager

# Do not use this skill when

- the task is really about adjacent tasks where a more specific skill already covers the job
- If the task is more specifically about `handoff-brief` or `pr-review-ticket-bridge`, prefer that skill instead.
- the relevant files, runtime, or deliverable type are already covered by a more specific active skill

# Operating procedure

1. Define the task boundary for Skill Registry Manager.
2. Collect the minimum context needed to avoid guessing.
3. Choose a concrete approach rather than listing generic options.
4. Verify the result against the real constraint surface.
5. Report the next action if the work cannot safely finish in one pass.

# Decision rules

- Stay concrete.
- Keep the output tied to the task boundary.
- State uncertainty.
- Prefer verification over confidence language.

# Output requirements

1. `Scope`
2. `Approach`
3. `Checks`
4. `Next Step`

# References

Read these only when relevant:

- `references/workflow-notes.md`
- `references/review-checklist.md`
- `references/failure-modes.md`

# Related skills

- `handoff-brief`
- `pr-review-ticket-bridge`
- `skill-eval-runner`
- `skill-description-optimizer`

# Failure handling

- If the scope is ambiguous, restate the decision boundary before proceeding.
- If the evidence is weak, say so explicitly and lower confidence instead of smoothing it over.
- If the task would be better served by a narrower skill, redirect to it rather than stretching this one.
