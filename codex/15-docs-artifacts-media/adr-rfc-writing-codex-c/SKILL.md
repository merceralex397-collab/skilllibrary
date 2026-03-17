---
name: adr-rfc-writing-codex-c
description: Writes architecture decision records and request-for-comment docs that capture options and rationale. Use this when the user or repo work clearly points at adr and rfc writing or a task in the "Docs, Artifacts, and Media Skills" family needs repeatable procedure rather than ad hoc prompting. Do not use for implementation tasks where no artifact or communication deliverable is needed.
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: codex
  domain: adr-rfc-writing
  maturity: draft
  risk: low
  tags: [adr, rfc, writing]
---

# Purpose

Writes architecture decision records and request-for-comment docs that capture options and rationale.

# When to use this skill

Use this skill when:

- the user or repo work clearly points at adr and rfc writing
- a task in the "Docs, Artifacts, and Media Skills" family needs repeatable procedure rather than ad hoc prompting
- a plan, ticket, or repo state would benefit from explicit guardrails around adr and rfc writing

# Do not use this skill when

- the task is really about implementation tasks where no artifact or communication deliverable is needed
- If the task is more specifically about `document-writing` or `research-synthesis`, prefer that skill instead.
- the relevant files, runtime, or deliverable type are already covered by a more specific active skill

# Operating procedure

1. Clarify the audience, artifact type, and decision the document for ADR and RFC Writing must support.
2. Collect only the source material needed to preserve fidelity and avoid drift.
3. Structure the output so readers can scan the key decision, evidence, and next step quickly.
4. Check tone, completeness, and formatting before treating the document as done.
5. Call out assumptions or unresolved facts instead of smoothing them over in prose.

# Decision rules

- Optimize for reader decisions, not word count.
- Preserve source fidelity when audience and artifact shape or source fidelity are disputed.
- Flag uncertainty instead of paraphrasing it away.
- Use structure to make review fast.

# Output requirements

1. `Audience`
2. `Artifact Structure`
3. `Draft or Final Output`
4. `Review Notes`

# References

Read these only when relevant:

- `references/artifact-structure.md`
- `references/quality-checklist.md`
- `references/audience-adjustments.md`

# Related skills

- `document-writing`
- `research-synthesis`
- `runbook-writing`
- `release-notes`

# Failure handling

- If the scope is ambiguous, restate the decision boundary before proceeding.
- If the evidence is weak, say so explicitly and lower confidence instead of smoothing it over.
- If the task would be better served by a narrower skill, redirect to it rather than stretching this one.
