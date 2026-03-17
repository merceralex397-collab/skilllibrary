---
name: tailscale-private-networking-codex-c
description: Applies secure private networking patterns for multi-machine systems without exposing unnecessary surfaces. Use this when the user or repo work clearly points at tailscale and private networking or a task in the "Cloud, Platform, and DevOps Skills" family needs repeatable procedure rather than ad hoc prompting. Do not use for application logic changes that do not affect delivery, runtime, or operator workflow.
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: codex
  domain: tailscale-private-networking
  maturity: draft
  risk: low
  tags: [tailscale, private, networking]
---

# Purpose

Applies secure private networking patterns for multi-machine systems without exposing unnecessary surfaces.

# When to use this skill

Use this skill when:

- the user or repo work clearly points at tailscale and private networking
- a task in the "Cloud, Platform, and DevOps Skills" family needs repeatable procedure rather than ad hoc prompting
- a plan, ticket, or repo state would benefit from explicit guardrails around tailscale and private networking

# Do not use this skill when

- the task is really about application logic changes that do not affect delivery, runtime, or operator workflow
- If the task is more specifically about `queues-cron-workers` or `cost-monitoring`, prefer that skill instead.
- the relevant files, runtime, or deliverable type are already covered by a more specific active skill

# Operating procedure

1. Define the target environment, promotion path, and operator constraints for Tailscale and Private Networking.
2. Run a preflight over secrets, configuration, dependencies, and rollback options.
3. Apply the change in the smallest reversible unit possible.
4. Verify runtime health, logs, and expected outputs immediately after the change.
5. Record rollback triggers and follow-up monitoring so the handoff is operationally useful.

# Decision rules

- Prefer reversible changes and small blast radius.
- If environment promotion cannot be verified, treat the rollout as incomplete.
- Record rollback triggers before or during rollout, not after.
- Do not treat successful deploy logs as proof of healthy behavior.

# Output requirements

1. `Environment`
2. `Change Plan`
3. `Verification`
4. `Rollback or Monitoring`

# References

Read these only when relevant:

- `references/preflight-checklist.md`
- `references/verification-checks.md`
- `references/rollback-notes.md`

# Related skills

- `queues-cron-workers`
- `cost-monitoring`
- `self-hosting-ops`
- `cloudflare-worker-patterns`

# Failure handling

- If the scope is ambiguous, restate the decision boundary before proceeding.
- If the evidence is weak, say so explicitly and lower confidence instead of smoothing it over.
- If the task would be better served by a narrower skill, redirect to it rather than stretching this one.
