---
name: skill-adaptation
description: "Adapts a general skill so it fits one repo, team, or project profile without losing the base pattern. You explicitly asked for this. Trigger when the task context clearly involves skill adaptation."
source: created
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  category: meta-skill-engineering
  priority: P1
  maturity: draft
  risk: low
  tags: [adaptation, profile, customization]
---

# Purpose
Takes a general-purpose skill and adapts it for a specific context: different tech stack, different organization, different domain, or different conventions. Preserves the core procedure while replacing generic references with context-specific ones.

# When to use this skill
Use when:
- User says "adapt this skill for our project", "customize for Python/React/etc.", "make this work for our team"
- Installing library skill that needs project-specific conventions
- Porting skill from one stack to another (e.g., React → Vue, npm → pnpm)
- Skill references tools/APIs/conventions that don't exist in target environment

Do NOT use when:
- Skill is already project-specific and just needs refinement (use `skill-refinement`)
- Creating skill from scratch (use `skill-authoring`)
- Only trigger description needs work (use `skill-description-optimizer`)
- Skill works fine in current context

# Operating procedure
1. **Read the source skill completely**: Understand the problem it solves, output it produces, and assumptions it encodes
2. **Identify the target context**:
   - Stack/language: What technologies?
   - Conventions: Naming, file structure, coding style?
   - Tools: What's available? What's forbidden?
   - Domain: What terminology?
3. **Catalog adaptation points** (things that MUST change):
   - Tool/command references (e.g., `npm` → `pnpm`, `pytest` → `unittest`)
   - File paths and patterns (e.g., `src/components` → `app/ui`)
   - Naming conventions (e.g., camelCase → snake_case)
   - Output formats (e.g., Markdown → org-mode)
   - Domain terminology (e.g., "user" → "customer")
4. **Identify invariants** (things that must NOT change):
   - Core procedure logic
   - Safety constraints
   - Quality checks
   - The fundamental problem being solved
5. **Create adapted version**:
   - Keep frontmatter structure
   - Update description if target context affects routing
   - Replace all adaptation points with target-specific versions
   - Add context-specific examples if skill uses few-shot
6. **Validate the adaptation**:
   - Every tool reference exists in target context
   - Every file path pattern makes sense
   - No dangling references to source-context things
7. **Document what was adapted**: Add Adaptation Note at bottom noting source skill and what changed

# Output defaults
```
## Adaptation Summary

**Source skill**: [name and location]
**Target context**: [stack/project/team]

### Changes Made
| Original | Adapted | Reason |
|----------|---------|--------|
| npm install | pnpm add | Project uses pnpm |
| src/components/ | app/ui/ | Different structure |

### Invariants Preserved
- [Core logic 1]
- [Core logic 2]

### Adapted SKILL.md
[Full content]

### Adaptation Note
Adapted from [source] for [target]. Changes: [list].
```

# References
- Original skill documentation
- Target project's conventions (AGENTS.md, README, etc.)

# Failure handling
- **Source skill poorly documented**: Infer intent from procedure, flag assumptions made
- **Target context unclear**: Ask: "What stack? What file structure? What tools available?"
- **Adaptation breaks core logic**: Skill may not be adaptable—recommend creating new skill
- **Missing equivalent in target**: Flag gap, suggest alternatives or note limitation
