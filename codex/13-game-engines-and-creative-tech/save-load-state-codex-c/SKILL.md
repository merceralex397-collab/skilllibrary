---
name: save-load-state-codex-c
description: Handles persistence of progress, configuration, checkpoints, and schema evolution in games. Use this when the user or repo work clearly points at save and load state or a task in the "Game Engines and Creative Tech Skills" family needs repeatable procedure rather than ad hoc prompting. Do not use for non-creative maintenance work where engine, art, or design systems are irrelevant.
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: codex
  domain: save-load-state
  maturity: draft
  risk: low
  tags: [save, load, state]
---

# Purpose

Handles persistence of progress, configuration, checkpoints, and schema evolution in games.

# When to use this skill

Use this skill when:

- the user or repo work clearly points at save and load state
- a task in the "Game Engines and Creative Tech Skills" family needs repeatable procedure rather than ad hoc prompting
- a plan, ticket, or repo state would benefit from explicit guardrails around save and load state

# Do not use this skill when

- the task is really about non-creative maintenance work where engine, art, or design systems are irrelevant
- If the task is more specifically about `performance-profiling-games` or `input-mapping-controller`, prefer that skill instead.
- the relevant files, runtime, or deliverable type are already covered by a more specific active skill

# Operating procedure

1. Define the player, user, or audience outcome that Save and Load State is meant to improve.
2. Choose a pattern that respects engine constraints, content pipeline realities, and core loop constraints.
3. Prototype or reason at the system level before polishing a single asset or screen.
4. Check performance, control feel, or readability before expanding scope.
5. End with the next iteration loop and what to measure or review.

# Decision rules

- Protect the core user experience before adding breadth.
- Prefer systems that survive iteration over one-off polish on core loop constraints.
- Measure feel, readability, or pacing with actual scenarios.
- State the tradeoff between novelty and production cost.

# Output requirements

1. `Creative Goal`
2. `Implementation Plan`
3. `Iteration Notes`
4. `Validation`

# References

Read these only when relevant:

- `references/design-direction.md`
- `references/production-checklist.md`
- `references/iteration-prompts.md`

# Related skills

- `performance-profiling-games`
- `input-mapping-controller`
- `ai-npc-behavior`
- `blueprint-patterns`

# Failure handling

- If the scope is ambiguous, restate the decision boundary before proceeding.
- If the evidence is weak, say so explicitly and lower confidence instead of smoothing it over.
- If the task would be better served by a narrower skill, redirect to it rather than stretching this one.
