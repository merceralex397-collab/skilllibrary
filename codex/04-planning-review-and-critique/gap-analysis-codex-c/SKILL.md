---
name: gap-analysis-codex-c
description: Compares current repo or plan state against target state and lists missing pieces cleanly. Use this when the user or repo work clearly points at gap analysis or a task in the "Planning, Review, and Critique" family needs repeatable procedure rather than ad hoc prompting. Do not use for ordinary implementation work where no review, critique, or comparison is needed.
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: codex
  domain: gap-analysis
  maturity: draft
  risk: low
  tags: [gap, analysis]
---

# Purpose

Compares current repo or plan state against target state and lists missing pieces cleanly.

# When to use this skill

Use this skill when:

- the user or repo work clearly points at gap analysis
- a task in the "Planning, Review, and Critique" family needs repeatable procedure rather than ad hoc prompting
- a plan, ticket, or repo state would benefit from explicit guardrails around gap analysis

# Do not use this skill when

- the task is really about ordinary implementation work where no review, critique, or comparison is needed
- If the task is more specifically about `blocker-extraction` or `acceptance-criteria-hardening`, prefer that skill instead.
- the relevant files, runtime, or deliverable type are already covered by a more specific active skill

# Operating procedure

1. Define the decision, plan, or implementation surface being reviewed for Gap Analysis.
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

- `blocker-extraction`
- `acceptance-criteria-hardening`
- `skeptic-pass`
- `failure-mode-analysis`

# Failure handling

- If the scope is ambiguous, restate the decision boundary before proceeding.
- If the evidence is weak, say so explicitly and lower confidence instead of smoothing it over.
- If the task would be better served by a narrower skill, redirect to it rather than stretching this one.
