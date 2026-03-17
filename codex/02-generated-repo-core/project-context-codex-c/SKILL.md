---
name: project-context-codex-c
description: Load a repository's source-of-truth documents in a deterministic order before planning, editing, or reviewing. Use when starting work in an unfamiliar repo, resuming a session, or when the user asks for a repo-aware plan, fix, or review and the authoritative docs may be spread across README, AGENTS, specs, tickets, and config files. Do not use when the task is already narrowly scoped and the relevant files are known.
---

# Purpose

Use this skill to avoid coding off the wrong documents.

The main job is to find the smallest authoritative context surface that makes the task safe. Do not bulk-load the repo. Read the source-of-truth docs in priority order, then stop.

# When to use this skill

Use this skill when:

- you are entering a repo or subsystem cold
- the task depends on repo-specific rules, specs, or workflows
- you need to know which docs outrank other docs
- the user asks for a repo-aware plan before implementation

Strong trigger phrases:

- "understand this repo first"
- "what should I read before changing this"
- "get the project context"
- "rehydrate the repo state"

# Do not use this skill when

- the task is already limited to a known file and no repo conventions matter
- the source of truth is already established in the current conversation
- you are doing a tiny mechanical edit with no architecture or workflow risk

# Operating procedure

1. Start with authority, not convenience.
   Search for explicit instruction files first: `AGENTS.md`, `CLAUDE.md`, repo-local command docs, `START-HERE.md`, `README.md`, ticket indexes, and active spec folders.

2. Resolve precedence.
   Prefer the nearest explicit project authority over historical notes, draft plans, or random docs. If two docs conflict, record the conflict and follow the higher-precedence document.

3. Build a minimal reading set.
   Read only what is required to answer:
   - what is the task
   - what files or subsystems are in scope
   - what repo rules govern changes
   - what validation the repo expects

4. Stop when the task is grounded.
   Once the authoritative surface is clear, move on. Do not keep reading for reassurance.

5. Summarize the context surface.
   State the docs read, what each controls, and what remains uncertain.

# Decision rules

- Prefer project-local instructions over general docs.
- Prefer current workflow docs over archive or postmortem material.
- Prefer root-level entry docs first, then subsystem docs for the specific area being changed.
- If a spec pack exists for the task, treat it as authoritative for requirements but not necessarily for coding conventions.
- If the repo has no clear authority docs, say that explicitly and fall back to README + code inspection.

# Output requirements

Return:

1. `Authority Surface`
2. `In-Scope Docs Read`
3. `Open Uncertainties`
4. `Next Files to Inspect`

# Scripts

Use `scripts/context_inventory.py` to generate a ranked list of likely authority files before you start opening them.

Example:

```bash
python scripts/context_inventory.py --root . --format text
```

# References

- Read `references/doc-precedence.md` for authority order.
- Read `references/context-stop-rules.md` to avoid over-reading.
- Read `references/session-reentry-checklist.md` when resuming interrupted work.

# Failure handling

- If no authority files exist, say the repo lacks a strong context surface and switch to code inspection.
- If you find conflicting instructions, name the conflict and follow the highest-precedence source.
- If the task scope is unclear, stop after the authority sweep and clarify scope before deep reading.
