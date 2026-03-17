---
name: opencode-team-bootstrap
description: "Builds the initial project-specific operating layer of agents, tools, plugins, commands, and skills. Your repos use an agent operating layer, so this deserves a dedicated builder. Trigger when the task context clearly involves opencode team bootstrap."
source: github.com/scafforge/session
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  category: package-scaffolding
  priority: P0
  maturity: draft
  risk: low
  tags: [opencode, bootstrap, agents]
---

# Purpose
Generates a project-specific OpenCode agent team with specialized agents, tools, plugins, commands, and skills tailored to the project's stack and domain. Transforms the generic agent scaffold into a team of specialists that understand this specific codebase—implementers who know the stack, reviewers who know the patterns, researchers who know where to look.

# When to use this skill
Use when:
- After repo-scaffold-factory has created the base structure
- The project needs specialized agents beyond generic roles
- Converting a Copilot/Cursor repo to OpenCode format
- Stack profile is known and agents should specialize

Do NOT use when:
- No scaffold exists yet (run scaffold-kickoff first)
- The project is too simple for agent specialization
- Working in a non-OpenCode environment

# Operating procedure

## 1. Read project context
```bash
cat docs/BRIEF.md
cat docs/STACK-PROFILE.md
tree -L 2 -I 'node_modules|dist|target'
```

## 2. Determine team composition
Based on stack profile, select agent roles:

| Stack | Recommended Agents |
|-------|-------------------|
| TypeScript/Node | ts-implementer, ts-reviewer, api-specialist |
| Python/FastAPI | py-implementer, py-reviewer, api-specialist |
| Rust | rust-implementer, rust-reviewer, systems-specialist |
| React | react-frontend, component-specialist |
| Full-stack | backend-specialist, frontend-specialist, infra-specialist |
| Any | planner, researcher, test-specialist |

## 3. Create directory structure
```bash
mkdir -p .opencode/agents .opencode/skills .opencode/tools .opencode/commands
```

## 4. Generate agent definitions
For each agent, create `.opencode/agents/[name].yaml`:

### Example: implementer agent
```yaml
name: implementer
description: "Implements features according to tickets and stack standards"
model: claude-sonnet-4  # or project preference
system_prompt: |
  You are an implementer for [PROJECT_NAME], a [DESCRIPTION].
  
  Stack: [from STACK-PROFILE.md]
  
  Before coding:
  1. Read the ticket fully
  2. Check stack-standards skill
  3. Review similar existing code
  
  Coding rules:
  - Follow patterns in src/
  - Write tests alongside code
  - Commit incrementally with ticket ID
  
  You have access to: edit files, run tests, git operations

tools:
  - bash
  - edit
  - view
  - grep

permissions:
  write: [src/, tests/]
  read: [docs/, tickets/]
  execute: [npm, git]
```

### Example: reviewer agent
```yaml
name: reviewer
description: "Reviews code for bugs, security issues, and pattern violations"
model: claude-sonnet-4
system_prompt: |
  You are a code reviewer for [PROJECT_NAME].
  
  Review for:
  1. Bugs and logic errors
  2. Security vulnerabilities
  3. Pattern violations (see stack-standards)
  4. Missing tests
  
  Do NOT comment on:
  - Formatting (linter handles this)
  - Style preferences
  - Trivial naming
  
  Output: Structured findings with evidence

tools:
  - view
  - grep
  - bash (read-only commands)

permissions:
  write: []  # READ ONLY
  read: [src/, tests/, docs/]
```

### Example: researcher agent
```yaml
name: researcher
description: "Investigates codebase and external resources for information"
model: claude-haiku-4  # Fast model for exploration
system_prompt: |
  You are a researcher for [PROJECT_NAME].
  
  Your job: Find information, report findings with evidence.
  
  You can:
  - Read any file
  - Search with grep
  - Check git history
  - Fetch documentation URLs
  
  You cannot:
  - Modify files
  - Run commands with side effects
  
  Always cite sources: file:line for code, URL for docs.

tools:
  - view
  - grep
  - glob
  - bash (grep, find, git log only)
  - web_fetch

permissions:
  write: []  # READ ONLY
  read: ["**/*"]
```

## 5. Generate project-specific skills
Create `.opencode/skills/` with filled-in skills:
```bash
# Stack standards (populated from STACK-PROFILE.md)
# Testing patterns (from test files analysis)
# API conventions (from routes analysis)
```

See project-skill-bootstrap for detailed skill generation.

## 6. Generate custom commands
Create `.opencode/commands/` for common workflows:

### Example: tkt command
```yaml
# .opencode/commands/tkt.yaml
name: tkt
description: "Work on a ticket"
usage: "/tkt TKT-XXX"
handler: |
  1. Load ticket from tickets/TKT-XXX*.md
  2. Switch to implementer agent
  3. Invoke ticket-execution skill
  4. Execute until done or blocked
```

### Example: review command
```yaml
name: review
description: "Review current changes"
usage: "/review"
handler: |
  1. Get diff of current branch vs main
  2. Switch to reviewer agent
  3. Invoke review-audit-bridge skill
  4. Output findings
```

## 7. Generate configuration
Create `.opencode/config.yaml`:
```yaml
project:
  name: [from BRIEF.md]
  description: [from BRIEF.md]

defaults:
  model: claude-sonnet-4
  agent: implementer

agents:
  - implementer
  - reviewer
  - researcher

skills:
  - stack-standards
  - testing-patterns
  - project-context

commands:
  - tkt
  - review
```

## 8. Initialize AGENTS.md reference
Update AGENTS.md to reference OpenCode setup:
```markdown
## OpenCode Configuration

This project uses OpenCode agents defined in `.opencode/`:

### Agents
- `implementer`: Writes and tests code
- `reviewer`: Reviews changes (read-only)
- `researcher`: Investigates codebase (read-only)

### Commands
- `/tkt TKT-XXX`: Work on a ticket
- `/review`: Review current changes

### Skills
See `.opencode/skills/` for project-specific guidance.
```

# Output defaults
```
.opencode/
├── config.yaml           # Main configuration
├── agents/
│   ├── implementer.yaml
│   ├── reviewer.yaml
│   └── researcher.yaml
├── skills/
│   ├── stack-standards.md
│   └── testing-patterns.md
├── commands/
│   ├── tkt.yaml
│   └── review.yaml
└── tools/                # Custom tools if needed
```

# References
- OpenCode documentation: https://opencode.ai/docs
- OpenCode uses AGENTS.md + .opencode/ directory for configuration
- Agent permissions model: explicit read/write scopes

# Failure handling
- **No stack profile**: Generate generic agents, flag for customization later
- **Stack not supported**: Create generic implementer/reviewer, note stack-specific gaps
- **OpenCode not installed**: Generate files anyway; they're valid for future use
- **Conflicting agent names**: Use project prefix (e.g., `myproj-implementer`)
