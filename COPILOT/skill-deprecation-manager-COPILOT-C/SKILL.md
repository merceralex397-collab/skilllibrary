---
name: skill-deprecation-manager
description: "Retires or merges obsolete skills while preserving backward references and library clarity. Large libraries decay quickly without a retirement policy. Trigger when the task context clearly involves skill deprecation manager."
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
  tags: [deprecation, lifecycle, retirement]
---

# Purpose
Safely deprecates a specific skill: updates state, adds notices, redirects references, preserves content for history without leaving it active. Handles migration path, replacement pointer, and grace period.

# When to use this skill
Use when:
- User says "deprecate this skill", "retire this", "this is replaced by X"
- Catalog curation identified skill for retirement
- Skill superseded by new version or merged
- Skill causing harm (misfiring, wrong outputs)—pull immediately

Do NOT use when:
- Skill needs improvement, not retirement (use `skill-refinement`)
- Multiple skills need lifecycle management (use `skill-lifecycle-management`)
- Repo doesn't support deprecation—just delete

# Operating procedure
1. **Confirm deprecation decision**:
   - Reason: superseded by [X], merged into [Y], unused [N cycles], failing evaluation, causing harm
2. **Find all references**:
   - Search in: AGENTS.md, other SKILL.md ("Do NOT use when"), skills-lock.json, docs, commands
3. **Update each reference**:
   - Replace with replacement skill or note "no replacement"
4. **Update frontmatter**:
   ```yaml
   metadata:
     maturity: deprecated
     deprecated_by: replacement-skill  # or "none"
     deprecated_reason: "Superseded by newer version"
   ```
5. **Add deprecation notice** (top of SKILL.md):
   ```markdown
   > ⚠️ DEPRECATED as of [date]. Use [replacement] instead.
   > Reason: [one sentence]. Kept for reference only.
   ```
6. **Move to archive** (if exists):
   - Move to `ARCHIVE/skill-name/`
   - Do NOT delete—preserve for reference
7. **Update skills-lock.json**:
   - Mark `deprecated: true`
8. **Update library index**:
   - Remove from active catalog
   - Add to "Deprecated" section

# Output defaults
```
## Deprecation: [skill-name]

**Reason**: [superseded | merged | unused | failing | harmful]
**Replacement**: [skill] or "none"
**Date**: [YYYY-MM-DD]

### References Updated
| File | Line | Change |
|------|------|--------|
| AGENTS.md | 45 | skill-old → skill-new |
| other-skill.md | 28 | Updated "Do NOT use when" |

### Frontmatter Updated
- maturity: deprecated
- deprecated_by: [X]

### Archive Location
ARCHIVE/skill-name/

### Verification
- [x] References updated
- [x] Notice added
- [x] Archived
- [x] Index updated
```

# References
- Skill being deprecated
- Replacement skill if exists
- Library index

# Failure handling
- **Active dependencies, no replacement**: Don't deprecate—document gap, recommend `skill-authoring` for replacement first
- **Replacement incomplete**: Create migration path, note limitations
- **Urgent harm**: Deprecate immediately, create follow-up for replacement
