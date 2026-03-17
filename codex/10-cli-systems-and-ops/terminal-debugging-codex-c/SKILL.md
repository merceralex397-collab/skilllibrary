---
name: terminal-debugging-codex-c
description: Uses focused terminal inspection to diagnose environment, process, path, and runtime problems. Use this when the user or repo work clearly points at terminal debugging or a task in the "CLI, Systems, and Ops Skills" family needs repeatable procedure rather than ad hoc prompting. Do not use for application logic changes that do not affect delivery, runtime, or operator workflow.
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: codex
  domain: terminal-debugging
  maturity: draft
  risk: low
  tags: [terminal, debugging]
---

# Purpose

Uses focused terminal inspection to diagnose environment, process, path, and runtime problems.

# When to use this skill

Use this skill when:

- the user or repo work clearly points at terminal debugging
- a task in the "CLI, Systems, and Ops Skills" family needs repeatable procedure rather than ad hoc prompting
- a plan, ticket, or repo state would benefit from explicit guardrails around terminal debugging

# Do not use this skill when

- the task is really about application logic changes that do not affect delivery, runtime, or operator workflow
- If the task is more specifically about `cli-development-python` or `bash`, prefer that skill instead.
- the relevant files, runtime, or deliverable type are already covered by a more specific active skill

# Operating procedure

1. Define the target environment, promotion path, and operator constraints for Terminal Debugging.
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

- `cli-development-python`
- `bash`
- `tui-development`
- `cobra-go`

# Failure handling

- If the scope is ambiguous, restate the decision boundary before proceeding.
- If the evidence is weak, say so explicitly and lower confidence instead of smoothing it over.
- If the task would be better served by a narrower skill, redirect to it rather than stretching this one.
