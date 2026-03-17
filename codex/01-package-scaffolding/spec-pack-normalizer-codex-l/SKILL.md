---
name: spec-pack-normalizer-codex-l
description: "Normalize one or more source specs, notes, pasted chats, and Markdown planning documents into a single canonical brief, implementation context, and constraints summary. Use when Codex is given messy or multi-file project requirements and needs to turn them into a clean source of truth before scaffolding, ticketing, or implementation."
---

> Source: local codex skill spec-pack-normalizer

# Spec Pack Normalizer

Use this skill to convert source material into a deterministic brief that weaker models can follow safely.

## Workflow

1. Read the primary source files first, then supporting notes.
2. Extract only durable facts, constraints, desired outcomes, and explicit non-goals.
3. Resolve obvious duplication and contradiction by preferring the most specific or latest source.
4. Produce a canonical brief, assumptions list, open questions list, and implementation context summary.
5. Keep the brief structured and signposted so it can be used by scaffolding, ticketing, and OpenCode agents without rereading the entire source pack.

## Output shape

Use the schema in `references/brief-schema.md`.

Minimum sections:

- Project summary
- Goals
- Non-goals
- Constraints
- Repo/output requirements
- Tooling/model constraints
- Acceptance signals
- Assumptions
- Open questions

## Rules

- Prefer Markdown source packs as the primary input.
- Separate facts from assumptions explicitly.
- Do not hide contradictions; call them out in `Open questions`.
- Keep the final brief concise enough to be loaded by weaker models.
- Preserve exact product names, provider strings, and model identifiers when they are specified.
