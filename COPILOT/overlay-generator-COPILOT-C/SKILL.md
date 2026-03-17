---
name: overlay-generator
description: "Produces host-specific overlays for OpenAI, Gemini, OpenCode, Copilot, or other clients from a shared skill source. The architecture docs explicitly favor one source skill with multiple target packages. Trigger when the task context clearly involves overlay generator."
source: created
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  category: package-scaffolding
  priority: P1
  maturity: draft
  risk: low
  tags: [overlay, client, packaging]
---

# Purpose
Generates client-specific overlay files from a canonical SKILL.md source. Each AI client (Copilot, OpenCode, Codex, Gemini CLI) has different metadata requirements and packaging conventions—this skill produces the right overlay format for each target while keeping the skill content authoritative in one place.

# When to use this skill
Use when:
- Publishing a skill to multiple AI client ecosystems
- A skill needs client-specific metadata (permissions, triggers, model hints)
- Converting between overlay formats (e.g., Copilot → OpenCode)
- Validating that overlays match their source SKILL.md

Do NOT use when:
- Creating skills for a single client only
- The skill is purely documentation with no machine-readable parts
- Editing skill content (edit SKILL.md, then regenerate overlays)

# Operating procedure

## 1. Understand overlay architecture
```
SKILL.md (canonical source)
    │
    ├── metadata.json (Copilot overlay)
    ├── permissions.yaml (OpenCode overlay)
    ├── openai.yaml (Codex overlay)
    └── manifest.json (Universal overlay)
```

The SKILL.md frontmatter is the source of truth. Overlays are generated views.

## 2. Parse SKILL.md frontmatter
```yaml
# Example SKILL.md frontmatter
---
name: my-skill
description: "What this skill does"
source: github.com/org/repo
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  category: implementation
  priority: P0
  maturity: stable
  risk: low
  tags: [coding, typescript]
---
```

## 3. Generate Copilot overlay (metadata.json)
```json
{
  "name": "my-skill",
  "version": "1.0.0",
  "description": "What this skill does",
  "main": "SKILL.md",
  "license": "Apache-2.0",
  "repository": {
    "type": "git",
    "url": "github.com/org/repo"
  },
  "copilot": {
    "category": "implementation",
    "priority": "P0",
    "tags": ["coding", "typescript"],
    "triggers": [
      "when the task context clearly involves my-skill"
    ]
  }
}
```

Location: Same directory as SKILL.md

## 4. Generate OpenCode overlay (permissions.yaml)
```yaml
name: my-skill
description: "What this skill does"
version: "1.0.0"

permissions:
  read:
    - "src/**/*"
    - "tests/**/*"
    - "docs/**/*"
  write:
    - "src/**/*"
    - "tests/**/*"
  execute:
    - "npm"
    - "git"

triggers:
  patterns:
    - "implement"
    - "code"
    - "typescript"
  
model_hints:
  preferred: "claude-sonnet-4"
  fallback: "claude-haiku-4"
```

Location: `.opencode/skills/my-skill/permissions.yaml`

## 5. Generate Codex overlay (openai.yaml)
```yaml
name: my-skill
description: "What this skill does"
schema_version: "1.0"

capabilities:
  - file_read
  - file_write
  - shell_execute

activation:
  keywords:
    - "implement"
    - "typescript"
    - "code"

constraints:
  max_file_size: 1000000
  allowed_extensions: [".ts", ".js", ".json", ".md"]
```

Location: `.codex/skills/my-skill/openai.yaml`

## 6. Generate universal manifest (manifest.json)
For cross-client skill registries:
```json
{
  "schema_version": "1.0.0",
  "skill": {
    "name": "my-skill",
    "description": "What this skill does",
    "version": "1.0.0",
    "license": "Apache-2.0",
    "source": "github.com/org/repo"
  },
  "compatibility": {
    "clients": ["openai-codex", "gemini-cli", "opencode", "github-copilot"],
    "min_versions": {}
  },
  "metadata": {
    "category": "implementation",
    "priority": "P0",
    "maturity": "stable",
    "tags": ["coding", "typescript"]
  },
  "files": {
    "skill": "SKILL.md",
    "copilot": "metadata.json",
    "opencode": "permissions.yaml",
    "codex": "openai.yaml"
  }
}
```

## 7. Validation
After generating overlays, validate:
```bash
# Check all overlays reference same name
grep -h "name" metadata.json permissions.yaml openai.yaml | sort -u
# Should show single name

# Check description consistency
grep -h "description" metadata.json permissions.yaml openai.yaml

# Validate JSON/YAML syntax
cat metadata.json | jq .
cat permissions.yaml | yq .
```

## 8. Batch generation
For a skill library:
```bash
for skill_dir in COPILOT/*/; do
  skill_md="$skill_dir/SKILL.md"
  if [ -f "$skill_md" ]; then
    generate_overlays "$skill_md"
  fi
done
```

# Output defaults
For each SKILL.md:
```
skill-name/
├── SKILL.md           # Canonical source (unchanged)
├── metadata.json      # Copilot overlay
├── permissions.yaml   # OpenCode overlay (optional)
└── openai.yaml        # Codex overlay (optional)
```

Plus at skill library root:
```
skills-manifest.json   # Index of all skills and their overlays
```

# References
- Anthropic Skills format: https://github.com/anthropics/skills
- Skills are SKILL.md + metadata overlays per client

# Failure handling
- **Missing frontmatter field**: Use sensible defaults, log warning
- **Invalid YAML in frontmatter**: Stop, report parse error
- **Client not in compatibility list**: Skip that overlay, log skip
- **Overlay already exists**: Backup existing, regenerate, diff for review
