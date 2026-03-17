---
name: agent-prompt-engineering-codex-l
description: "Design and harden agent, command, workflow, and tool prompts for reliable weak-model execution. Use when creating or revising repo-local agents, slash commands, workflow docs, prompt packs, or stage-gated autonomous flows, especially to tighten scope, artifact contracts, delegation boundaries, status semantics, or tool-use rules."
---

> Source: local codex skill agent-prompt-engineering

# Agent Prompt Engineering

Use this skill when prompt wording controls how agents coordinate, route work, and use tools.

## Workflow

1. Read the existing prompt, command, or process doc and identify its authority, scope, tool surface, and stage transitions.
2. Read `references/prompt-contracts.md` for the target prompt type.
3. Use `references/anti-patterns.md` to remove status-over-evidence routing, contradictory state semantics, vague output contracts, and impossible read-only delegation.
4. Use `references/examples.md` to rewrite the prompt with explicit boundaries, artifact proofs, and verification rules.
5. Keep prompts short. Move stable procedure into local tools, plugins, or skills when possible.
6. Re-read the final prompt and ask whether a weaker model could execute it without inventing hidden state.

## Rules

- Prefer tool-backed state over raw file choreography.
- Keep ticket status coarse and queue-oriented.
- Put transient approval state in workflow state or explicit artifacts.
- Require verification before stage changes.
- Never instruct read-only agents to mutate repo-tracked files.
- Never claim a file changed unless a write-capable tool actually wrote it.

## References

- `references/prompt-contracts.md`
- `references/anti-patterns.md`
- `references/examples.md`
