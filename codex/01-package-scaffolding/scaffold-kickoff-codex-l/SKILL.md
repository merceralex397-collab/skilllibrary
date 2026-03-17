---
name: scaffold-kickoff-codex-l
description: "Orchestrate the full spec-to-repo kickoff flow for greenfield or early-stage projects. Use when Codex needs one clear starting point that reads a spec pack, normalizes requirements, scaffolds the repository, generates the ticket system, bootstraps the OpenCode team, creates project-local skills, and writes the first handoff documents."
---

> Source: local codex skill scaffold-kickoff

# Scaffold Kickoff

Use this as the default entrypoint for repo planning and scaffold creation.

## Workflow

1. Load `spec-pack-normalizer` and convert the input spec pack into a concise canonical brief.
2. Confirm or infer the project name, slug, destination path, agent prefix, and default OpenCode model string.
3. Use `repo-scaffold-factory` to generate the repo skeleton, docs, ticket pack, `opencode.jsonc`, and `.opencode/` tree.
4. Use `agent-prompt-engineering` if the generated agent or command prompts need project-specific hardening.
5. Review the generated canonical brief, board, and manifest so the scaffold reflects the real project goals rather than template defaults.
6. Use `project-skill-bootstrap` to tighten the local `.opencode/skills/` layer once the stack or domain is known.
7. Run `repo-process-doctor` in audit mode against the freshly generated repo before handoff.
8. Use `handoff-brief` to refresh `START-HERE.md` and the restart surface before stopping.
9. Leave `ticket-pack-builder` for backlog expansion and `review-audit-bridge` for later implementation and QA cycles.

## Required outputs

- a canonical brief that separates facts, assumptions, and open questions
- a scaffolded repo with `README.md`, `AGENTS.md`, `START-HERE.md`, docs, and tickets
- the OpenCode agent, command, tool, plugin, and local skill layer
- a process audit confirming the generated repo did not drift into raw-file stage control
- a short handoff surface that another machine or session can resume from

## Rules

- Prefer this umbrella flow for greenfield work instead of manually starting with the lower-level scaffold skills.
- Keep the generated docs and prompts weak-model friendly: short sections, explicit steps, and obvious source-of-truth files.
- If the repo already exists and only needs the OpenCode layer, switch to `opencode-team-bootstrap` instead of forcing a full scaffold reset.
- When the stack is still unknown, keep the scaffold framework-agnostic and record the unresolved choices in the canonical brief.
- Preserve exact model/provider strings and project names when the source material specifies them.
- Do not ship a repo-local workflow until a doctor pass confirms queue state, approval state, and stage artifacts are separated cleanly.

## Hand-off points

- Use `ticket-pack-builder` only when the initial ticket set needs expansion or standardization after the first scaffold.
- Use `opencode-team-bootstrap` when the repo already has docs and tickets but still needs `.opencode/`.
- Use `review-audit-bridge` once implementation starts and the project needs deterministic review, QA, or audit passes.

This skill is intentionally thin. Its job is to make the starting sequence obvious and route into the existing modular skills.

`repo-scaffold-factory` remains the only source of truth for scaffold template assets. This skill should orchestrate that flow, not fork template logic.
