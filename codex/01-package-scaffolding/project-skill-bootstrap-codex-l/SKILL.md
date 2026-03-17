---
name: project-skill-bootstrap-codex-l
description: "Create deterministic project-local OpenCode skills for repo context, navigation, execution, stack standards, and handoff. Use when a newly scaffolded or evolving project needs local procedural guidance that is more specific than the global Codex skill pack."
---

> Source: local codex skill project-skill-bootstrap

# Project Skill Bootstrap

Use this skill to create the repo-local `.opencode/skills/` layer.

## Goals

- make project context easy to reload
- encode stack/domain standards locally
- reduce ambiguity for weaker models
- keep prompts short by moving stable procedure into local skills
- keep project-local workflow skills aligned with ticket tools and workflow state

## Default local skill pack

- `project-context`
- `repo-navigation`
- `stack-standards`
- `ticket-execution`
- `docs-and-handoff`

Use `references/local-skill-catalog.md` and the starter template in `assets/templates/SKILL.template.md`.

When creating or revising project-local workflow skills, use `agent-prompt-engineering`
to avoid raw-file stage control, contradictory status semantics, or impossible
read-only delegation contracts.

Use this after the main scaffold exists. It should refine repo-local procedural guidance, not replace `repo-scaffold-factory` or `opencode-team-bootstrap`.
