---
name: swarm-patterns-codex-c
description: Defines reusable collaboration patterns for many agents working on one outcome with bounded overlap. Use this when the user or repo work clearly points at swarm patterns or a task in the "Agentic Orchestration and Autonomy" family needs repeatable procedure rather than ad hoc prompting. Do not use for small single-agent tasks where delegation overhead would be larger than the work.
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: codex
  domain: swarm-patterns
  maturity: draft
  risk: low
  tags: [swarm, patterns]
---

# Purpose

Defines reusable collaboration patterns for many agents working on one outcome with bounded overlap.

# When to use this skill

Use this skill when:

- the user or repo work clearly points at swarm patterns
- a task in the "Agentic Orchestration and Autonomy" family needs repeatable procedure rather than ad hoc prompting
- a plan, ticket, or repo state would benefit from explicit guardrails around swarm patterns

# Do not use this skill when

- the task is really about small single-agent tasks where delegation overhead would be larger than the work
- If the task is more specifically about `goal-decomposition` or `manager-hierarchy-design`, prefer that skill instead.
- the relevant files, runtime, or deliverable type are already covered by a more specific active skill

# Operating procedure

1. Identify the next blocking step and keep it on the critical path for the lead agent.
2. Split supporting work by responsibility, with explicit artifact boundaries around critical path control and delegate contracts.
3. Define stop conditions, handoff shape, and rejoin points before launching delegates.
4. Reintegrate results and resolve conflicts before issuing another round of delegation.
5. Escalate when evidence is weak, overlapping, or missing rather than advancing on status alone.

# Decision rules

- Do not delegate the immediate blocker unless the lead agent is truly better spent elsewhere.
- Refuse overlapping write scopes when critical path control could collide across delegates.
- Make every delegation request produce a tangible artifact or answer.
- Pause the workflow when a delegate returns status without evidence.

# Output requirements

1. `Critical Path`
2. `Delegates or Stages`
3. `Artifact Contracts`
4. `Rejoin Criteria`

# References

Read these only when relevant:

- `references/delegate-contracts.md`
- `references/checkpoint-rules.md`
- `references/failure-escalation.md`

# Related skills

- `goal-decomposition`
- `manager-hierarchy-design`
- `collaboration-checkpoints`
- `multi-agent-debugging`

# Failure handling

- If the scope is ambiguous, restate the decision boundary before proceeding.
- If the evidence is weak, say so explicitly and lower confidence instead of smoothing it over.
- If the task would be better served by a narrower skill, redirect to it rather than stretching this one.
