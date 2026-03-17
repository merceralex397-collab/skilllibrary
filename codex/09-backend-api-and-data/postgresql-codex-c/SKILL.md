---
name: postgresql-codex-c
description: Defines schema, migration, indexing, transaction, and query expectations for PostgreSQL-backed work. Use this when the user or repo work clearly points at postgresql or a task in the "Backend, API, and Data Skills" family needs repeatable procedure rather than ad hoc prompting. Do not use for pure frontend styling or content-only tasks.
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: codex
  domain: postgresql
  maturity: draft
  risk: low
  tags: [postgresql]
---

# Purpose

Defines schema, migration, indexing, transaction, and query expectations for PostgreSQL-backed work.

# When to use this skill

Use this skill when:

- the user or repo work clearly points at postgresql
- a task in the "Backend, API, and Data Skills" family needs repeatable procedure rather than ad hoc prompting
- a plan, ticket, or repo state would benefit from explicit guardrails around postgresql

# Do not use this skill when

- the task is really about pure frontend styling or content-only tasks
- If the task is more specifically about `express-node` or `go-api-service`, prefer that skill instead.
- the relevant files, runtime, or deliverable type are already covered by a more specific active skill

# Operating procedure

1. Map the service boundary, data flow, and failure surface affected by PostgreSQL.
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

- `express-node`
- `go-api-service`
- `sqlite`
- `orm-patterns`

# Failure handling

- If the scope is ambiguous, restate the decision boundary before proceeding.
- If the evidence is weak, say so explicitly and lower confidence instead of smoothing it over.
- If the task would be better served by a narrower skill, redirect to it rather than stretching this one.
