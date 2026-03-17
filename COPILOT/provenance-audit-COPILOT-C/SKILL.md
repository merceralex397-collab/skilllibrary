---
name: provenance-audit
description: "Tracks where each generated skill came from: user request, repo evidence, imported example, or invented extension. You want evidence and traceability, so provenance deserves its own utility. Trigger when the task context clearly involves provenance audit."
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
  tags: [provenance, audit, traceability]
---

# Purpose
Audits a skill or artifact's origin chain: where it came from, who authored it, what license applies, what modifications were made, and what trust level to assign. Enables evidence-based decisions about whether to adopt, modify, or reject external skills.

# When to use this skill
Use when:
- Evaluating an external skill for adoption
- Auditing existing skills for license compliance
- Investigating a skill's modification history
- Establishing trust level for skill execution

Do NOT use when:
- Creating new skills from scratch (provenance is "authored here")
- Quick one-off skill usage without long-term adoption
- The skill is from a trusted internal source

# Operating procedure

## 1. Identify provenance sources
Every skill has one primary origin:

| Origin Type | Description | Trust Level |
|-------------|-------------|-------------|
| `authored` | Created in this repo | High |
| `forked` | Copied from external source, modified | Medium |
| `imported` | Copied from external source, unmodified | Medium |
| `generated` | AI-generated from requirements | Low |
| `unknown` | Origin unclear | Untrusted |

## 2. Extract provenance metadata
From SKILL.md frontmatter:
```yaml
---
name: skill-name
source: github.com/org/repo  # Origin URL
license: Apache-2.0          # License
# ...
---
```

From git history:
```bash
# First commit of skill file
git log --follow --format="%H %an %ad %s" -- SKILL.md | tail -1

# All modifications
git log --follow --oneline -- SKILL.md

# Check if imported from another repo
git log --follow -p -- SKILL.md | grep -E "^(Author|Date|commit)"
```

## 3. Verify source claims
If `source` field claims external origin:
```bash
# Fetch original
curl -s "https://raw.githubusercontent.com/org/repo/main/SKILL.md" > /tmp/original.md

# Compare
diff SKILL.md /tmp/original.md

# If different, skill has been modified (forked not imported)
```

## 4. Check license compatibility
```bash
# Extract license from source
license=$(grep -E "^license:" SKILL.md | cut -d: -f2 | tr -d ' ')

# Check compatibility matrix
case "$license" in
  "Apache-2.0"|"MIT"|"BSD-3-Clause")
    echo "Permissive: OK for any use"
    ;;
  "GPL-3.0"|"AGPL-3.0")
    echo "Copyleft: Check derivative work implications"
    ;;
  "")
    echo "WARNING: No license specified"
    ;;
esac
```

## 5. Build provenance report
```markdown
# Provenance Audit: [skill-name]
Date: [ISO date]
Auditor: [agent-id or human]

## Origin
- **Type:** [authored|forked|imported|generated|unknown]
- **Source:** [URL or "this repo"]
- **Original Author:** [name/org]
- **License:** [SPDX identifier]

## Modification History
| Date | Author | Change |
|------|--------|--------|
| 2024-01-01 | Original | Initial import |
| 2024-01-15 | agent-id | Updated for TypeScript stack |

## Trust Assessment
- **License:** [PASS|WARN|FAIL] - [reason]
- **Source verified:** [YES|NO|UNABLE]
- **Modifications reviewed:** [YES|NO|N/A]
- **Overall trust:** [HIGH|MEDIUM|LOW|UNTRUSTED]

## Recommendations
- [Action to take based on audit]
```

## 6. Trust level decision matrix

| Origin | License OK | Source Verified | Mods Reviewed | Trust Level |
|--------|------------|-----------------|---------------|-------------|
| authored | N/A | N/A | N/A | HIGH |
| forked | YES | YES | YES | HIGH |
| forked | YES | YES | NO | MEDIUM |
| imported | YES | YES | N/A | MEDIUM |
| imported | YES | NO | N/A | LOW |
| generated | N/A | N/A | YES | MEDIUM |
| generated | N/A | N/A | NO | LOW |
| unknown | * | * | * | UNTRUSTED |

## 7. Document provenance in skill
Add provenance section to SKILL.md:
```markdown
# Provenance
- Origin: Forked from github.com/anthropics/skills/example-skill
- Original license: Apache-2.0
- Forked: 2024-01-15
- Modifications:
  - Adapted for TypeScript project
  - Added project-specific examples
- Last audit: 2024-01-20
- Trust level: HIGH (source verified, mods reviewed)
```

## 8. Automated provenance tracking
For skill libraries, maintain provenance index:
```json
{
  "skills": {
    "skill-name": {
      "origin": "forked",
      "source": "github.com/org/repo",
      "license": "Apache-2.0",
      "imported_date": "2024-01-15",
      "last_audit": "2024-01-20",
      "trust": "high"
    }
  }
}
```

# Output defaults
```markdown
# Provenance Audit Report

## Summary
- Skills audited: N
- High trust: N
- Medium trust: N
- Low trust: N
- Untrusted: N

## Issues Found
- [skill-name]: [issue description]

## Recommendations
1. [Recommended action]
```

# References
- SPDX license identifiers: https://spdx.org/licenses/
- Software supply chain security principles

# Failure handling
- **Cannot reach source URL**: Mark as "source unverified", lower trust level
- **License unclear**: Flag for manual review, do not auto-adopt
- **Conflicting provenance claims**: Report conflict, require manual resolution
- **Skill has no frontmatter**: Treat as unknown origin, untrusted
