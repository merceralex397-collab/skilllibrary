# Agent Skills Library

A curated library of **350 agent skills** across 17 categories, designed for selective installation into AI coding agents (GitHub Copilot, OpenAI Codex, Gemini CLI, Claude Code, OpenCode).

## Purpose

This is **not** a monolithic install. Each skill is a self-contained playbook that can be installed individually based on project needs. Pick the skills relevant to your current work — don't install everything.

## Categories

| # | Category | Skills | Description |
|---|----------|--------|-------------|
| 01 | [Package Scaffolding](01-package-scaffolding/) | 20 | Project bootstrapping, repo scaffolding, Scafforge workflow skills |
| 02 | [Generated Repo Core](02-generated-repo-core/) | 32 | Core engineering: testing, security, deployment, error handling, auth |
| 03 | [Meta Skill Engineering](03-meta-skill-engineering/) | 18 | Creating, evaluating, packaging, and maintaining skills themselves |
| 04 | [Planning, Review & Critique](04-planning-review-and-critique/) | 18 | Thinking techniques: premortem, steelman, FMEA, root cause analysis |
| 05 | [Agentic Orchestration](05-agentic-orchestration-and-autonomy/) | 20 | Multi-agent coordination, delegation, verification, workflow state |
| 06 | [Agent Role Candidates](06-agent-role-candidates/) | 16 | Specialized agent roles: planner, implementer, QA, security review |
| 07 | [MCP](07-mcp/) | 20 | Model Context Protocol: servers, clients, tools, auth, deployment |
| 08 | [Web, Frontend & Design](08-web-frontend-and-design/) | 26 | React, Next.js, Vue, Svelte, Tailwind, Playwright, accessibility |
| 09 | [Backend, API & Data](09-backend-api-and-data/) | 20 | FastAPI, Flask, PostgreSQL, Redis, GraphQL, microservices |
| 10 | [CLI, Systems & Ops](10-cli-systems-and-ops/) | 20 | Go CLI, Bash, Docker, systemd, tmux, Linux admin |
| 11 | [AI/LLM Runtime](11-ai-llm-runtime-and-integration/) | 28 | Ollama, vLLM, RAG, embeddings, model routing, inference serving |
| 12 | [AI/LLM Training & Research](12-ai-llm-training-architecture-and-research/) | 23 | LoRA, DPO, DeepSpeed, MoE, training infrastructure |
| 13 | [Game Engines & Creative](13-game-engines-and-creative-tech/) | 17 | Unity, Unreal, Godot, Blender, game AI, multiplayer |
| 14 | [Cloud & DevOps](14-cloud-platform-devops/) | 19 | AWS, GCP, Cloudflare, Firebase, Terraform, Docker, Vercel |
| 15 | [Docs, Artifacts & Media](15-docs-artifacts-media/) | 33 | PDF, DOCX, PPTX, XLSX, ADR writing, release notes, runbooks |
| 16 | [Business & Research](16-business-research-and-optional-domains/) | 12 | Business Model Canvas, competitive analysis, user research |
| 17 | [External Reference Seeds](17-external-reference-seeds/) | 8 | Prior art research, package evaluation, technology radar |

**Total: 350 skills**

## Skill Format

Each skill folder contains at minimum a `SKILL.md` with:

```yaml
---
name: skill-name
description: "Routing description with trigger phrases and DO NOT USE boundaries"
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  domain: category
  maturity: draft|stable
  risk: low
  tags: [specific, tags]
---
```

Body sections: Purpose, When to use, Do NOT use when, Operating procedure, Decision rules, Output requirements, References, Related skills, Failure handling.

Some skills also include `references/`, `evals/`, `overlays/`, `scripts/`, and `manifest.yaml`.

## Installation

Skills are installed individually. Example with the Skills CLI:

```bash
# Browse available skills
npx skills find react

# Install a specific skill
npx skills add owner/repo@skill-name
```

Or copy a skill folder directly into your agent's skills directory (e.g., `.copilot/skills/`, `.opencode/skills/`).

## Scafforge Integration

Category 01 contains the full Scafforge workflow:

1. **scaffold-kickoff** → entry point (greenfield / retrofit / refinement)
2. **spec-pack-normalizer** → canonical brief from messy specs
3. **repo-scaffold-factory** → base repo structure
4. **opencode-team-bootstrap** → agent team generation
5. **ticket-pack-builder** → implementation-ready ticket system
6. **project-skill-bootstrap** → project-local skills
7. **agent-prompt-engineering** → prompt hardening
8. **repo-process-doctor** → workflow audit
9. **handoff-brief** → session handoff surface

These skills preserve all Scafforge workflow logic while being portable across agent clients.

## Quality Standards

Every skill has been through the skill-improver process:
- **No boilerplate**: Zero generic "consider best practices" content
- **Concrete procedures**: Every step is an actionable command or decision
- **Real references**: Official documentation URLs, not blog posts
- **Specific triggers**: Description field routes accurately to the right tasks
- **Failure handling**: Named failure modes with specific fixes

## License

Apache-2.0
