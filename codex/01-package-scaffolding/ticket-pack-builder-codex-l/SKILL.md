---
name: ticket-pack-builder-codex-l
description: "Create a GitHub-native ticket system with an index, machine-readable manifest, board, and per-ticket templates. Use when a repo needs task decomposition that is easy for weaker models and autonomous agents to follow without re-planning the whole project each session."
---

> Source: local codex skill ticket-pack-builder

# Ticket Pack Builder

Use this skill to create or refine the repo-local work queue.

## Rules

- Keep the manifest machine-readable.
- Keep the board human-readable.
- Keep each ticket file short, linked, and stage-aware.
- Record dependencies explicitly.
- Put acceptance criteria and artifacts on each ticket.

## Use with the scaffold factory

The full scaffold template already includes a ticket pack. Use this skill when you need to:

- regenerate the ticket system
- expand the backlog after the initial scaffold
- tighten acceptance criteria
- standardize ticket structure across repos

Use the reference in `references/ticket-system.md` and the starter template in `assets/templates/TICKET.template.md`.

Do not treat this as an alternate scaffold root. Use it to refine or expand ticketing after the main scaffold exists.
