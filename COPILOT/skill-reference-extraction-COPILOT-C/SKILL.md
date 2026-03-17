---
name: skill-reference-extraction
description: "Pulls large docs, examples, and schemas out of the main SKILL body into references for progressive disclosure. This follows the recommended architecture pattern. Trigger when the task context clearly involves skill reference extraction."
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
  tags: [references, extraction, architecture]
---

# Purpose
Extracts large reference material (docs, schemas, examples, tables) from SKILL.md into references/ directory. Keeps core skill concise while making detail available on demand—progressive disclosure.

# When to use this skill
Use when:
- SKILL.md >500 lines or >10KB
- Contains large code examples, schemas, lookup tables
- User says "this is too long", "extract references", "slim down"
- Detailed appendix not needed every invocation
- Multiple skills could share reference material

Do NOT use when:
- Skill already concise (<200 lines)
- "Reference" is actually core procedure
- Material small (<50 lines)
- Extraction would break flow

# Operating procedure
1. **Identify extraction candidates**:
   - Code examples >20 lines
   - Schema definitions
   - Lookup tables/mappings
   - API documentation excerpts
   - Configuration templates
   - Extended case studies
2. **Assess each**:
   - Needed every invocation? → Keep inline
   - Reference/lookup material? → Extract
   - Shared across skills? → Extract to shared location
3. **Create references/ structure**:
   ```
   skill-name/
   ├── SKILL.md
   └── references/
       ├── README.md
       ├── schema.json
       ├── examples/
       │   ├── basic.md
       │   └── advanced.md
       └── api-docs.md
   ```
4. **Extract each piece**:
   - Move to appropriate file
   - Name descriptively
   - Preserve formatting
5. **Add pointers in SKILL.md**:
   - "See references/schema.json for full schema"
   - Or: `<!-- @include references/schema.json -->`
   - Keep 1-2 line summary
6. **Create references/README.md**:
   ```markdown
   # References
   | File | Contents | When to use |
   |------|----------|-------------|
   | schema.json | API schema | Validating responses |
   ```
7. **Verify completeness**:
   - Skill understandable without references?
   - Procedure steps present?
   - References signposted?

# Output defaults
```
## Reference Extraction: [skill]

### Extracted
| Content | From | To | Size |
|---------|------|-----|------|
| Schema | L150-300 | references/schema.json | 5KB |

### Reduction
- Before: 850 lines, 25KB
- After: 280 lines, 8KB
- Reduction: 67%

### Verification
- [x] Procedure intact
- [x] References signposted
- [x] README created
```

# References
- Anthropic skill structure
- Progressive disclosure principles

# Failure handling
- **Can't determine what to extract**: If doubt, keep inline
- **Would break skill**: Material is procedural—leave inline
- **Circular references**: Flatten structure
- **Shared conflict**: Create skill-specific copy
