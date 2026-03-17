---
name: skill-authoring
description: "Creates or updates project-local skills inside a generated repo using the repo's own conventions and evidence. Necessary if repos are supposed to evolve their own local intelligence. Trigger when the task context clearly involves skill authoring."
source: created
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  category: generated-repo-core
  priority: P1
  maturity: draft
  risk: low
  tags: [skill, authoring, local]
---

# Purpose
Creates new skills following the Agent Skills format: YAML frontmatter defining routing metadata, plus a markdown body with Purpose, When to use, Operating procedure, Output defaults, and Failure handling. The description field is routing logic—it determines whether the skill fires, not documentation for humans.

# When to use this skill
Use when:
- User says "create a skill for X", "write a skill that...", "I need a skill to handle..."
- Repeated task pattern needs to be captured as reusable procedure
- Capability should be packaged for distribution or reuse across projects
- New agent specialization needed for a repo's agent team

Do NOT use when:
- Skill exists and needs minor improvement (use `skill-refinement`)
- Only description/trigger needs fixing (use `skill-description-optimizer` or `skill-trigger-optimization`)
- User wants a one-off prompt, not reusable skill (use `prompt-crafting`)
- Skill needs adaptation to different context (use `skill-adaptation`)

# Operating procedure
1. **Define the skill's job in one sentence**: "This skill [verb] when [trigger] and produces [output]." If you can't do this, the scope is wrong
2. **Choose the name**: Lowercase, hyphens, 2-4 words. Name describes what it does (verb-noun), not when it's used. Examples: `premortem`, `gap-analysis`, `acceptance-criteria-hardening`
3. **Write the description field** (THIS IS ROUTING LOGIC, NOT DOCUMENTATION):
   - First phrase = most discriminating signal that distinguishes this from similar skills
   - Include 2-3 trigger phrases users actually say, in quotes
   - Mention what it produces
   - End with "Trigger when: [specific observable condition]"
   - Keep under 60 words, no marketing language
4. **Write frontmatter**:
   ```yaml
   ---
   name: skill-name
   description: "[routing description]"
   source: [origin URL or "created"]
   license: Apache-2.0
   compatibility:
     clients: [openai-codex, gemini-cli, opencode, github-copilot]
   metadata:
     owner: [owner]
     category: [category]
     priority: P0|P1|P2
     maturity: draft|stable|deprecated
     risk: low|medium|high
     tags: [relevant, tags]
   ---
   ```
5. **Write Purpose**: 2-3 sentences of substance. What problem? What output? No filler
6. **Write "When to use"**:
   - "Use when:" — 4-6 specific trigger phrases or observable conditions
   - "Do NOT use when:" — 3-4 confusion cases with named alternatives
7. **Write Operating procedure**: Numbered steps, each starting with verb, each completable and verifiable. No meta-commentary like "Keep scope explicit"
8. **Write Output defaults**: Exact format with template or schema. Not "structured markdown" — name specific sections
9. **Write Failure handling**: Name the 2-3 most common failure modes and what to do for each
10. **Add References section**: Real URLs to authoritative sources
11. **Validate against anti-patterns** (check `skill-anti-patterns`):
    - No circular triggers ("when task involves X")
    - No boilerplate steps
    - No generic output defaults

# Output defaults
A complete skill folder:
```
skill-name/
├── SKILL.md          # Frontmatter + sections above
├── scripts/          # Optional automation
├── references/       # Optional large docs
└── evals/            # Optional tests
```

# References
- https://github.com/anthropics/skills — Anthropic skill implementation
- https://agentskills.io — Agent Skills specification
- https://github.com/vercel-labs/agent-skills — Vercel examples

# Failure handling
- **Scope too broad**: If skill handles multiple distinct tasks, split into separate skills
- **Can't write one-sentence definition**: Scope is wrong—narrow until it works
- **Overlaps existing skill**: Check catalog; either merge or explicitly differentiate in description
- **No clear trigger phrases**: Ask user what words they'd use when they want this
