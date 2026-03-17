---
name: firebase-sdk-codex-c
description: Applies client/server Firebase SDK usage conventions and integration cautions. Use this when the user or repo work clearly points at firebase sdk or a task in the "Backend, API, and Data Skills" family needs repeatable procedure rather than ad hoc prompting. Do not use for pure frontend styling or content-only tasks.
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: codex
  domain: firebase-sdk
  maturity: draft
  risk: low
  tags: [firebase, sdk]
---

# Purpose

Applies client/server Firebase SDK usage conventions and integration cautions.

# When to use this skill

Use this skill when:

- the user or repo work clearly points at firebase sdk
- a task in the "Backend, API, and Data Skills" family needs repeatable procedure rather than ad hoc prompting
- a plan, ticket, or repo state would benefit from explicit guardrails around firebase sdk

# Do not use this skill when

- the task is really about pure frontend styling or content-only tasks
- If the task is more specifically about `bigquery` or `firebase-rules`, prefer that skill instead.
- the relevant files, runtime, or deliverable type are already covered by a more specific active skill

# Operating procedure

1. Map the service boundary, data flow, and failure surface affected by Firebase SDK.
2. Choose the simplest backend pattern that protects boundary contracts and validation rules.
3. Specify validation, error handling, and operational expectations before coding edge cases ad hoc.
4. Check persistence, retries, and compatibility impacts where relevant.
5. Finish with the narrowest meaningful verification command or test plan.

# Decision rules

- Keep contracts and persistence rules explicit before optimizing.
- Prefer idempotent, observable behavior when boundary contracts or persistence expectations are at stake.
- Do not couple error handling to the happy path.
- State migration and compatibility costs early.

# Output requirements

1. `System Surface`
2. `Chosen Pattern`
3. `Edge Cases`
4. `Validation`

# References

Read these only when relevant:

- `references/implementation-patterns.md`
- `references/validation-checklist.md`
- `references/failure-modes.md`

# Related skills

- `bigquery`
- `firebase-rules`

# Failure handling

- If the scope is ambiguous, restate the decision boundary before proceeding.
- If the evidence is weak, say so explicitly and lower confidence instead of smoothing it over.
- If the task would be better served by a narrower skill, redirect to it rather than stretching this one.
