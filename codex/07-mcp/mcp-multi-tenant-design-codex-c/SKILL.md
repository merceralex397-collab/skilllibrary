---
name: mcp-multi-tenant-design-codex-c
description: Designs isolation, auth, state partitioning, and limits when one MCP service serves multiple tenants or users. Use this when building, debugging, securing, or packaging an MCP server or MCP tool surface or a task in the "MCP Skills" family needs repeatable procedure rather than ad hoc prompting. Do not use for generic API integration work that does not involve MCP tools, resources, prompts, or host contracts.
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: codex
  domain: mcp-multi-tenant-design
  maturity: draft
  risk: low
  tags: [mcp, multi, tenant, design]
---

# Purpose

Designs isolation, auth, state partitioning, and limits when one MCP service serves multiple tenants or users.

# When to use this skill

Use this skill when:

- building, debugging, securing, or packaging an MCP server or MCP tool surface
- a task in the "MCP Skills" family needs repeatable procedure rather than ad hoc prompting
- a plan, ticket, or repo state would benefit from explicit guardrails around mcp multi-tenant design

# Do not use this skill when

- the task is really about generic API integration work that does not involve MCP tools, resources, prompts, or host contracts
- If the task is more specifically about `mcp-schema-contracts` or `mcp-migration-retrofit`, prefer that skill instead.
- the relevant files, runtime, or deliverable type are already covered by a more specific active skill

# Operating procedure

1. Clarify the host, transport, and surface area involved in MCP Multi-Tenant Design.
2. Define tool, resource, or prompt contracts before implementation details spread.
3. Check permissions, auth, and failure semantics together instead of as afterthoughts.
4. Verify the server from both the implementation side and the host-consumer side.
5. Capture compatibility notes and debugging hooks for the next integration pass.

# Decision rules

- Define contracts before implementation sprawl.
- Prefer conservative permissions until tool and resource shape is proven necessary.
- Test both happy-path invocation and host-visible failure behavior.
- Keep transport and auth assumptions explicit in the output.

# Output requirements

1. `Runtime Surface`
2. `Contracts`
3. `Risk Controls`
4. `Validation Plan`

# References

Read these only when relevant:

- `references/interface-contracts.md`
- `references/test-cases.md`
- `references/risk-controls.md`

# Related skills

- `mcp-schema-contracts`
- `mcp-migration-retrofit`
- `mcp-chatgpt-app-bridge`
- `mcp-builder`

# Failure handling

- If the scope is ambiguous, restate the decision boundary before proceeding.
- If the evidence is weak, say so explicitly and lower confidence instead of smoothing it over.
- If the task would be better served by a narrower skill, redirect to it rather than stretching this one.
