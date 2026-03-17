---
name: review-audit-bridge-codex-l
description: "Structure repo review, implementation audit, security review, and QA passes so they produce short, actionable findings rather than vague commentary. Use when Codex or OpenCode needs a deterministic review sequence after planning or implementation."
---

> Source: local codex skill review-audit-bridge

# Review Audit Bridge

Use this skill to keep reviews evidence-based and stage-aware.

## Sequence

1. Restate the target change or ticket.
2. Read the approved plan and implementation artifacts.
3. Check correctness first, then regressions, then test gaps, then maintainability.
4. Produce prioritized findings with file references when applicable.
5. End with residual risks and missing validation only after findings.

Use `references/review-sequence.md` as the contract.
