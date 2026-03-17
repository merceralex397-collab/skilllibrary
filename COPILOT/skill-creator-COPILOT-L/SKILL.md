---
name: skill-creator
description: "Creates new SKILL.md files from scratch. Use when asked to build a new skill, generate a skill for a domain, or author a skill from a description. Trigger phrases: 'create a skill for X', 'write a skill that does X', 'I need a skill for X'. Do NOT use when adapting or refining an existing skill — use skill-adaptation or skill-refinement instead."
source: github.com/anthropics/skills
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  category: meta-skill-engineering
  priority: P0
  maturity: draft
  risk: low
  tags: [skill-creation, authoring, meta, scaffolding]
---

# Purpose
Generates a complete, high-quality SKILL.md from a domain description or user intent. The output skill must meet routing, procedural, and quality standards — not a generic template fill. Follows the Anthropic skills format used in github.com/anthropics/skills.

# When to use this skill
Use when:
- User says "create a skill for X" or "write a skill that does Y"
- A gap in the skill library is identified and needs to be filled
- A new domain-specific workflow needs to be codified as a reusable skill
- Bootstrapping a skill from a proposal stub or brief description

Do NOT use when:
- An existing skill just needs its description fixed — use `skill-description-optimizer`
- An existing skill needs adapting to a new stack — use `skill-adaptation`
- An existing skill needs quality improvement — use `skill-refinement`

# Operating procedure
1. **Clarify domain and scope**: Identify the exact task the skill will perform. Name one thing it does — not a family of things. If scope is ambiguous, ask before writing.
2. **Research the domain**: Fetch real documentation (official docs, specs, canonical repos) for the domain. Skills without domain knowledge produce generic steps.
3. **Write the description field first**: This is routing logic. It must include: exact trigger phrases, what the skill does in ≤2 sentences, and explicit DO NOT USE conditions with named alternatives. Test it mentally: would a router select this skill for the right prompts and reject it for the wrong ones?
4. **Write the operating procedure**: Every step must be a concrete action. No "consider", no "think about". If a step requires a specific tool, command, or pattern, name it explicitly.
5. **Add a # References section**: Real URLs only. If you couldn't find documentation, note that — don't invent references.
6. **Set maturity to `draft`**: New skills are always draft until they have passed trigger tests and output review.
7. **Validate with skill-testing-harness**: Generate at least 3 positive triggers and 2 negative triggers to verify routing.

# Output defaults
A complete SKILL.md with:
- YAML frontmatter (name, description, source, license, compatibility, metadata)
- Sections: Purpose, When to use, Operating procedure, Output defaults, References, Failure handling
- Saved to `SKILLNAME-COPILOT-C/SKILL.md` (use `-C` suffix for created skills)

# References
- https://github.com/anthropics/skills (canonical skill format and examples)
- https://skills.sh/ (registry for locating existing skills before creating)
- https://raw.githubusercontent.com/anthropics/skills/main/skill-creator/SKILL.md (this skill's own upstream)

# Failure handling
- **Scope too broad**: Split into multiple focused skills. A skill doing 3 things routes poorly and executes worse.
- **No real docs found**: Write the skill anyway but mark references as "unverified" and set maturity to `experimental`.
- **Description field routes incorrectly in testing**: Iterate on the description field specifically — don't rewrite the whole skill.
