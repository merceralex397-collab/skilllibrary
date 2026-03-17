---
name: project-skill-bootstrap
description: "Synthesizes project-local skills from the brief, stack, and domain signals instead of shipping only generic placeholders. This is the main answer to the 'bland skills' problem. Trigger when the task context clearly involves project skill bootstrap."
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
  tags: [skills, bootstrap, project]
---

# Purpose
Generates project-local skills populated with actual project data, stack-specific conventions, and domain-specific procedures—not generic placeholders. This skill reads the project's brief, stack profile, and codebase patterns, then produces skills that reference real files, real commands, and real constraints specific to this project.

# When to use this skill
Use when:
- After initial scaffolding, to customize generic skill templates
- When project conventions have been established but skills don't reflect them
- Adding a new domain-specific skill (e.g., "deployment", "database-migrations")
- Skills feel generic and don't help agents work effectively

Do NOT use when:
- The project has no established patterns yet (scaffold first)
- Updating a single skill (edit directly)
- The skill is universal, not project-specific (belongs in skill library)

# Operating procedure

## 1. Gather project signals
Extract concrete facts from:
```bash
# Stack
cat docs/STACK-PROFILE.md
cat package.json | jq '{name, scripts, dependencies}'
cat Cargo.toml | head -30

# Structure
tree -L 2 -I 'node_modules|dist|target'

# Patterns (sample real code)
head -50 src/index.* src/main.* 2>/dev/null
ls src/**/*.test.* tests/**/* 2>/dev/null | head -10

# Commands that exist
grep -E "^\s+\"[a-z]+\":" package.json | head -20  # npm scripts
```

## 2. Identify skill gaps
Compare existing `.opencode/skills/` to project needs:
```bash
ls .opencode/skills/ 2>/dev/null
```

Common project-specific skills needed:
- `stack-standards` — filled with actual stack choices
- `testing-patterns` — project's actual test conventions
- `deployment` — project's actual deploy process
- `database-ops` — project's actual DB patterns
- `api-conventions` — project's actual API patterns

## 3. Generate skill with real data
For each skill, replace placeholders with actuals:

### Example: Filling stack-standards
```yaml
# BEFORE (generic)
primary_language: [TypeScript | Python | Rust | Go | ...]
test_framework: [vitest | pytest | cargo test | go test | ...]

# AFTER (project-specific)
primary_language: TypeScript
runtime: Node.js 20.x
test_framework: vitest
test_command: npm test
formatter: prettier (config: .prettierrc)
linter: eslint (config: eslint.config.js)
```

### Example: Filling testing-patterns
```markdown
# Testing Patterns (Project-Specific)

## Test Location
Tests live in `src/**/*.test.ts` alongside source files.

## Test Framework
- Framework: Vitest
- Run all: `npm test`
- Run single: `npm test -- auth.test.ts`
- Watch mode: `npm test -- --watch`

## Patterns Used in This Project
- Use `describe`/`it` blocks (see `src/auth/auth.test.ts`)
- Mock external services with `vi.mock()` (see `src/api/api.test.ts`)
- Use factories in `tests/factories/` for test data
```

## 4. Reference real files
Every skill should cite actual project files:
```markdown
## Examples
- Authentication: see `src/auth/auth.service.ts`
- API route: see `src/routes/users.ts`
- Database model: see `src/models/User.ts`
```

## 5. Include real commands
```markdown
## Commands
```bash
# Validate before commit
npm run lint && npm run typecheck && npm test

# Start development
npm run dev

# Deploy
npm run build && npm run deploy
```
```

## 6. Add domain-specific knowledge
If the project has domain-specific concepts:
```markdown
## Domain Glossary
- **Replay**: A StarCraft 2 game recording file (.SC2Replay)
- **Fingerprint**: Unique identifier for a player's playstyle
- **Build Order**: Sequence of units/structures a player creates

## Domain Rules
- Replay files are immutable once parsed
- Fingerprints are computed, never manually assigned
- Build orders are ordered lists, not sets
```

## 7. Validate generated skills
For each skill:
- [ ] All referenced files exist
- [ ] All commands are valid
- [ ] No placeholder text remains (`[...]`, `TODO`, etc.)
- [ ] Skill is specific enough to be useful

## 8. Place skills correctly
```bash
mkdir -p .opencode/skills
# Write each skill
cat > .opencode/skills/testing-patterns.md << 'EOF'
[skill content]
EOF
```

# Output defaults
Skills written to `.opencode/skills/` with:
- Project name in metadata
- Real file references
- Real commands
- No placeholder text

Example output:
```
.opencode/skills/
├── stack-standards.md      # Filled with actual stack
├── testing-patterns.md     # Project's actual test patterns
├── api-conventions.md      # Project's actual API style
└── deployment.md           # Project's actual deploy process
```

# References
- Skills are instructions for agents, not documentation for humans
- Project-specific skills >> generic skills for reducing drift

# Failure handling
- **No stack profile exists**: Run stack-profile-detector first
- **Referenced file doesn't exist**: Remove reference or note "planned: [file]"
- **Command doesn't exist**: Check package.json/Makefile; if truly missing, note "TODO: add [command]"
- **Domain too complex to summarize**: Create multiple domain skills (domain-glossary, domain-rules, domain-workflows)
