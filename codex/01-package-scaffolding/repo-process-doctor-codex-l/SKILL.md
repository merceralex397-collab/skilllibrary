---
name: repo-process-doctor-codex-l
description: "Audit existing repositories for agent-workflow drift and repair opportunities. Use when a repo has custom OpenCode agents, commands, process docs, or ticket systems and you need to diagnose loops, contradictory status semantics, raw-file stage control, missing workflow-state tools, unsafe read-only delegation, or other workflow smells learned from real agent failures."
---

> Source: local codex skill repo-process-doctor

# Repo Process Doctor

Use this skill to inspect an existing repository before repairing its agent workflow.

## Workflow

1. Run `scripts/audit_repo_process.py <repo-root> --format both`.
2. Read the report and map each finding to `references/process-smells.md`.
3. Use `references/repair-playbook.md` to choose the migration target.
4. Use `references/safe-stage-contracts.md` when rewriting prompts, commands, or ticket docs.
5. Prefer tool-backed queue state, workflow-state approval gates, and registered stage artifacts over raw-file stage control.
6. Audit first. Only propose or apply repairs after the repo's failure modes are explicit.

## Required outputs

- concise diagnosis
- root cause
- exact files to patch
- target safer pattern

## Rules

- do not preserve contradictory status semantics just because they already exist
- do not keep multi-surface workflow state if one surface can become derived
- prefer deterministic migration to the current scaffold contract
- treat read-only mutation paths as blockers, not as acceptable shortcuts

## References

- `references/process-smells.md`
- `references/repair-playbook.md`
- `references/safe-stage-contracts.md`
