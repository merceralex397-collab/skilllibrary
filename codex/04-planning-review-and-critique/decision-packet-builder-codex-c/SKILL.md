---
name: decision-packet-builder-codex-c
description: Batches unresolved major choices with options, consequences, and recommendations instead of drip-feeding questions. Use this when the user or repo work clearly points at decision packet builder or a task in the "Planning, Review, and Critique" family needs repeatable procedure rather than ad hoc prompting. Do not use for ordinary implementation work where no review, critique, or comparison is needed.
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: codex
  domain: decision-packet-builder
  maturity: draft
  risk: low
  tags: [decision, packet, builder]
---

# Purpose

Batches unresolved major choices with options, consequences, and recommendations instead of drip-feeding questions.

# When to use this skill

Use this skill when:

- the user or repo work clearly points at decision packet builder
- a task in the "Planning, Review, and Critique" family needs repeatable procedure rather than ad hoc prompting
- a plan, ticket, or repo state would benefit from explicit guardrails around decision packet builder

# Do not use this skill when

- the task is really about ordinary implementation work where no review, critique, or comparison is needed
- If the task is more specifically about `contradiction-finder` or `scope-pressure-test`, prefer that skill instead.
- the relevant files, runtime, or deliverable type are already covered by a more specific active skill

# Operating procedure

1. Define the decision, plan, or implementation surface being reviewed for Decision Packet Builder.
2. Gather the minimum evidence needed to judge evidence collection, risk ordering, and assumption tracking.
3. Compare the evidence against explicit criteria instead of vague preference.
4. Rank findings by user impact, reversibility, and probability of causing rework.
5. End with a concrete recommendation and the smallest validation needed before advance.

# Decision rules

- Prefer evidence over intent when evidence collection and risk ordering disagree.
- Call out missing proof before debating style.
- Treat reversible issues differently from expensive-to-undo risks.
- Do not soften high-severity findings just because the implementation is unfinished.

# Output requirements

1. `Findings`
2. `Evidence`
3. `Recommendation`
4. `Verification or Open Questions`

# References

Read these only when relevant:

- `references/evidence-rubric.md`
- `references/output-shape.md`
- `references/failure-signals.md`

# Related skills

- `contradiction-finder`
- `scope-pressure-test`
- `tradeoff-analysis`
- `architecture-review`

# Failure handling

- If the scope is ambiguous, restate the decision boundary before proceeding.
- If the evidence is weak, say so explicitly and lower confidence instead of smoothing it over.
- If the task would be better served by a narrower skill, redirect to it rather than stretching this one.
