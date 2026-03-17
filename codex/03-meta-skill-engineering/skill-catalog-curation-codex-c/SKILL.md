---
name: skill-catalog-curation-codex-c
description: Reviews the library as a whole, merges overlaps, and organizes categories and discoverability. Use this when creating, adapting, refining, installing, testing, or packaging a skill or a task in the "Meta Skill Engineering" family needs repeatable procedure rather than ad hoc prompting. Do not use for one-off edits that do not benefit from a reusable workflow or package contract.
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: codex
  domain: skill-catalog-curation
  maturity: draft
  risk: low
  tags: [skill, catalog, curation]
---

# Purpose

Reviews the library as a whole, merges overlaps, and organizes categories and discoverability.

# When to use this skill

Use this skill when:

- creating, adapting, refining, installing, testing, or packaging a skill
- a task in the "Meta Skill Engineering" family needs repeatable procedure rather than ad hoc prompting
- a plan, ticket, or repo state would benefit from explicit guardrails around skill catalog curation

# Do not use this skill when

- the task is really about one-off edits that do not benefit from a reusable workflow or package contract
- If the task is more specifically about `skill-packaging` or `skill-installation`, prefer that skill instead.
- the relevant files, runtime, or deliverable type are already covered by a more specific active skill

# Operating procedure

1. Normalize the inputs, constraints, and success conditions before generating anything for Skill Catalog Curation.
2. Choose the smallest artifact set that solves the task without speculative extras.
3. Encode repo- or workflow-specific defaults so the output is ready to use, not just illustrative.
4. Validate the generated artifacts against structure, naming, and handoff expectations.
5. Document any remaining assumptions so the next agent or maintainer does not have to reverse-engineer intent.

# Decision rules

- Prefer deterministic structure over clever prose.
- Generate artifacts only when they materially help with input normalization or deterministic structure.
- Keep optional complexity out of the first usable package.
- Validate the shape before polishing the wording.

# Output requirements

1. `Inputs`
2. `Chosen Shape`
3. `Generated Artifacts`
4. `Validation and Handoff`

# References

Read these only when relevant:

- `references/inputs-and-variants.md`
- `references/packaging-checklist.md`
- `references/handoff-notes.md`

# Related skills

- `skill-packaging`
- `skill-installation`
- `skill-provenance`
- `skill-variant-splitting`

# Failure handling

- If the scope is ambiguous, restate the decision boundary before proceeding.
- If the evidence is weak, say so explicitly and lower confidence instead of smoothing it over.
- If the task would be better served by a narrower skill, redirect to it rather than stretching this one.
