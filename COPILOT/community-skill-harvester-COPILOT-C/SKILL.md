---
name: community-skill-harvester
description: "Finds external skills, extracts reusable patterns, and converts them into local proposal candidates. Your reports rely heavily on external examples, so harvesting should be formalized. Trigger when the task context clearly involves community skill harvester."
source: created
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  category: package-scaffolding
  priority: P2
  maturity: draft
  risk: low
  tags: [community, harvesting, import]
---

# Purpose
Harvests skills from public registries (skills.sh, GitHub repos, official skill collections) and evaluates them for quality, licensing, and fitness for adoption. Converts promising external skills into local proposal candidates with proper provenance tracking.

# When to use this skill
Use when:
- Looking for existing skills before building from scratch
- Evaluating external skill quality for potential adoption
- Migrating skills from community sources to local library
- Discovering patterns from how others structure skills

Do NOT use when:
- Building a novel skill with no external precedent
- Skills are for internal use only (no community comparison needed)
- Quick evaluation (just read the skill directly)

# Operating procedure

## 1. Identify skill sources

### Primary registries
- **skills.sh**: Aggregated skill registry with usage stats
- **Anthropic Skills**: Official reference implementations
- **GitHub Topics**: `agent-skills`, `ai-skills`, `mcp-server`

### High-quality repos to watch
```
github.com/anthropics/skills          # Official Anthropic
github.com/modelcontextprotocol/servers  # MCP reference servers
github.com/microsoft/github-copilot-*    # Microsoft skills
```

## 2. Search for relevant skills
```bash
# GitHub search
gh search repos --topic agent-skills --limit 20 --json fullName,description,stargazersCount

# skills.sh (if API available)
curl -s "https://skills.sh/api/search?q=[topic]" | jq .

# Direct repo exploration
gh api repos/anthropics/skills/contents/skills --jq '.[].name'
```

## 3. Evaluate skill quality

### Quality checklist
| Criterion | Check | Weight |
|-----------|-------|--------|
| Has SKILL.md | `test -f SKILL.md` | Required |
| Clear purpose | Non-vague description | High |
| Concrete procedures | Specific steps, not platitudes | High |
| License specified | SPDX identifier present | Required |
| Active maintenance | Commits in last 6 months | Medium |
| Usage evidence | Stars, forks, downloads | Medium |
| No security issues | No credentials, safe commands | Required |

### Quality scoring
```bash
score=0

# Required checks (fail = reject)
[ -f SKILL.md ] || exit 1
grep -q "license:" SKILL.md || exit 1

# Quality checks
grep -qE "^# Purpose" SKILL.md && ((score+=2))
grep -qE "^## [0-9]\\." SKILL.md && ((score+=2))  # Numbered steps
grep -qE "```(bash|typescript|python)" SKILL.md && ((score+=1))  # Code examples
[ $(wc -l < SKILL.md) -gt 50 ] && ((score+=1))  # Substantive content

echo "Quality score: $score/6"
```

## 4. License compatibility check
```bash
license=$(grep "license:" SKILL.md | cut -d: -f2 | tr -d ' ')

# Acceptable licenses for adoption
acceptable="Apache-2.0 MIT BSD-2-Clause BSD-3-Clause ISC CC0-1.0 Unlicense"

if echo "$acceptable" | grep -qw "$license"; then
  echo "License OK: $license"
else
  echo "License review needed: $license"
fi
```

## 5. Extract skill patterns
Before importing, understand the skill's patterns:
```markdown
## Skill Analysis: [name]

### Structure
- Frontmatter: [standard/custom fields]
- Sections: [Purpose, When to use, Procedure, etc.]
- Code blocks: [bash/typescript/python]

### Patterns worth adopting
- [Pattern 1]: [Why it's good]
- [Pattern 2]: [Why it's good]

### Patterns to avoid
- [Anti-pattern]: [Why it's problematic]

### Adaptation needed
- [Change 1]: [For local conventions]
- [Change 2]: [For stack compatibility]
```

## 6. Create import proposal
For skills that pass evaluation:
```markdown
# Skill Import Proposal

## Source
- URL: [github.com/org/repo/skills/name]
- License: [Apache-2.0]
- Stars: [N]
- Last updated: [date]

## Quality Assessment
- Score: [N/6]
- Checklist: [passed items]

## Rationale
Why adopt this skill:
- [Reason 1]
- [Reason 2]

## Required Adaptations
- [ ] Update for local stack (TypeScript, not Python)
- [ ] Add project-specific examples
- [ ] Align with local SKILL.md format

## Provenance
After import, document:
- Origin: forked from [URL]
- Import date: [ISO date]
- Modifications: [list]

## Recommendation
[ADOPT | ADAPT | REFERENCE_ONLY | REJECT]
```

## 7. Execute import (if approved)
```bash
# Fetch original
curl -o /tmp/original-skill.md "[raw skill URL]"

# Create local version
mkdir -p COPILOT/[skill-name]-COPILOT-[L|C]
cp /tmp/original-skill.md COPILOT/[skill-name]-COPILOT-[L|C]/SKILL.md

# Add provenance header
cat > /tmp/provenance << 'EOF'
# Provenance
- Origin: forked from [URL]
- Original license: [license]
- Import date: [ISO date]
- Modifications: See git history
---

EOF

# Prepend provenance (after frontmatter)
# ... edit SKILL.md to include provenance section

# Commit with clear provenance
git add COPILOT/[skill-name]-*
git commit -m "feat: import [skill-name] from [source]

Origin: [URL]
License: [license]
Adaptations: [brief summary]"
```

## 8. Track harvesting results
Maintain a harvest log:
```markdown
# Skill Harvest Log

## [Date] Harvest: [topic]

### Evaluated
| Skill | Source | Score | Decision |
|-------|--------|-------|----------|
| skill-a | anthropics/skills | 6/6 | ADOPT |
| skill-b | random/repo | 2/6 | REJECT |
| skill-c | microsoft/copilot | 5/6 | ADAPT |

### Imported
- skill-a → COPILOT/skill-a-COPILOT-L/

### Rejected (with reasons)
- skill-b: No concrete procedures, just vague advice
```

# Output defaults
```markdown
# Harvest Report: [topic]
Date: [ISO date]

## Summary
- Sources searched: N
- Skills evaluated: N
- Quality passed: N
- Imported: N

## Imports
| Skill | Source | License | Adaptations |
|-------|--------|---------|-------------|
| [name] | [URL] | [license] | [changes] |

## Patterns Discovered
- [Useful pattern from evaluation]

## Recommendations
- [Next skills to look for]
```

# References
- skills.sh registry: https://skills.sh/
- Anthropic Skills: https://github.com/anthropics/skills
- License compatibility: SPDX license list

# Failure handling
- **Registry unavailable**: Fall back to direct GitHub search
- **License unclear**: Do not import; flag for manual review
- **Skill format incompatible**: Extract concepts, rewrite in local format
- **Quality too low**: Document as "reference only", do not import
