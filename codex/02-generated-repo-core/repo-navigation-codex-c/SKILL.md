---
name: repo-navigation-codex-c
description: Map a repository's directory roles so the agent can distinguish canonical source, generated output, operational scripts, tests, archives, and user-owned areas before making changes. Use when the repo is large, layered, scaffolded, or has multiple output surfaces, and when the user asks where to work or which folders are safe to edit. Do not use for small single-folder projects where the structure is obvious.
---

# Purpose

Use this skill to answer: where does work belong, what is generated, and what should not be edited casually.

# When to use this skill

Use this skill when:

- the repo has multiple top-level directories with different roles
- you need to tell canonical source from generated output
- the user asks where an implementation should live
- you suspect there are protected, derived, or archive paths

# Do not use this skill when

- the repo layout is trivial
- the exact working directory is already obvious from the task and files opened

# Operating procedure

1. Identify top-level surfaces.
   List top-level directories and map their probable role: source, generated output, test harness, docs, tooling, archive, or user-specific area.

2. Confirm editability.
   Look for signals that a path is generated, vendored, mirrored, exported, or build output. Avoid editing derived paths unless the task explicitly targets them.

3. Find canonical implementation roots.
   Determine which directories are meant for actual source changes, which are for supporting docs, and which are for artifacts or exports.

4. State path rules.
   Summarize:
   - safe places to edit
   - places that are read-only by convention
   - places that are generated and should be updated indirectly

# Decision rules

- Treat `dist`, `build`, generated artifact folders, and exported bundles as derived until proven otherwise.
- Treat top-level instruction docs and task packs as normative context, not implementation destinations.
- Treat user-specific folders as scoped surfaces; do not write outside the user’s assigned area if the repo structure suggests ownership boundaries.

# Output requirements

Return:

1. `Canonical Paths`
2. `Derived or Generated Paths`
3. `Context or Policy Paths`
4. `Recommended Edit Surface`

# References

- Read `references/path-role-heuristics.md` to classify ambiguous directories.
- Read `references/generated-surface-signals.md` when deciding if a directory is derived.
- Read `references/ownership-boundaries.md` when a repo is split by user or agent area.

# Failure handling

- If you cannot prove whether a directory is generated, mark it uncertain and prefer the upstream source path.
- If multiple paths seem canonical, explain the split by responsibility instead of forcing one answer.
