---
name: stack-standards-codex-c
description: Extract the actual coding, validation, and operational conventions for the active stack from a real repository instead of guessing generic best practices. Use when starting implementation in a repo whose language, framework, test runner, linting, formatting, or release habits must be inferred from files like package manifests, pyproject, cargo manifests, config files, CI, and existing source. Do not use to invent a new stack from scratch.
---

# Purpose

Use this skill to answer: what does this repo actually consider standard for its stack?

The goal is not to recite global best practices. The goal is to infer the project’s real language, framework, validation commands, and conventions from evidence in the repo.

# When to use this skill

Use this skill when:

- the task requires writing code that matches existing stack conventions
- the repo has a real toolchain but no explicit standards doc
- you need to know test, lint, format, or runtime commands before editing
- the user asks how the stack works in this repo

# Do not use this skill when

- the task is greenfield technology selection
- the stack is already explicit and fully documented in a current standards doc
- the task is pure planning with no stack-specific implementation risk

# Operating procedure

1. Detect the stack from evidence.
   Inspect package manifests, lockfiles, config files, CI, and existing source roots. Do not infer from folder names alone.

2. Extract implementation conventions.
   Look for:
   - languages and versions
   - frameworks and libraries
   - folder structure
   - code style and naming patterns
   - test runner and validation commands
   - build, start, or deploy commands

3. Prefer observed practice over aspirational docs.
   If the README says one thing but the code and CI say another, flag the mismatch and prefer the enforced path.

4. Summarize the actionable rules.
   Produce a working stack profile with commands and conventions that another agent could follow immediately.

# Decision rules

- CI, lockfiles, and project config outweigh marketing docs.
- Existing source patterns outweigh broad ecosystem advice unless those patterns are obviously broken and the user asked for improvement.
- If multiple subprojects exist, split standards by package or service instead of forcing a single global rule set.
- If no validation tooling exists, say that rather than fabricating commands.

# Output requirements

Return:

1. `Detected Stack`
2. `Validation Commands`
3. `Code Patterns to Match`
4. `Mismatches or Gaps`

# Scripts

Use `scripts/detect_stack.py` for a quick evidence pass over common manifests and config files.

Example:

```bash
python scripts/detect_stack.py --root . --format text
```

# References

- Read `references/evidence-sources.md` to prioritize stack signals.
- Read `references/profile-template.md` for the output structure.
- Read `references/mismatch-handling.md` when docs and code disagree.

# Failure handling

- If the repo has several independent stacks, emit separate profiles.
- If the stack cannot be proven, say which files are missing and what evidence would settle it.
- If the repo is in transition, capture both the enforced current path and the intended target path.
