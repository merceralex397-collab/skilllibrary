---
name: benchmark-design-codex-c
description: Designs fair, discriminating, reproducible benchmarks for models, skills, or runtimes. Use this when the user or repo work clearly points at benchmark design or a task in the "AI / LLM Training, Architecture, and Research Skills" family needs repeatable procedure rather than ad hoc prompting. Do not use for ordinary software tasks with no model, inference, evaluation, or agent-runtime concerns.
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: codex
  domain: benchmark-design
  maturity: draft
  risk: low
  tags: [benchmark, design]
---

# Purpose

Designs fair, discriminating, reproducible benchmarks for models, skills, or runtimes.

# When to use this skill

Use this skill when:

- the user or repo work clearly points at benchmark design
- a task in the "AI / LLM Training, Architecture, and Research Skills" family needs repeatable procedure rather than ad hoc prompting
- a plan, ticket, or repo state would benefit from explicit guardrails around benchmark design

# Do not use this skill when

- the task is really about ordinary software tasks with no model, inference, evaluation, or agent-runtime concerns
- If the task is more specifically about `inference-kernel-optimization` or `eval-dataset-design`, prefer that skill instead.
- the relevant files, runtime, or deliverable type are already covered by a more specific active skill

# Operating procedure

1. Clarify the runtime goal, model boundaries, and interfaces involved in Benchmark Design.
2. Make schemas, prompt contracts, and tool surfaces explicit before iterating on behavior.
3. Constrain costs, latency, and failure fallbacks alongside quality goals.
4. Use representative eval or review cases instead of relying on one attractive demo.
5. Document the tradeoffs and next experiments needed to improve the system safely.

# Decision rules

- Make schemas and prompts serve the product boundary, not the other way around.
- Prefer measurable eval cases over intuition when runtime boundaries or eval coverage matter.
- Handle fallback and refusal paths explicitly.
- Do not hide cost or latency regressions behind quality anecdotes.

# Output requirements

1. `Runtime Context`
2. `Interfaces and Schemas`
3. `Safety or Cost Controls`
4. `Evaluation Plan`

# References

Read these only when relevant:

- `references/runtime-contracts.md`
- `references/eval-cases.md`
- `references/risk-controls.md`

# Related skills

- `inference-kernel-optimization`
- `eval-dataset-design`
- `safety-alignment`
- `reward-modeling`

# Failure handling

- If the scope is ambiguous, restate the decision boundary before proceeding.
- If the evidence is weak, say so explicitly and lower confidence instead of smoothing it over.
- If the task would be better served by a narrower skill, redirect to it rather than stretching this one.
