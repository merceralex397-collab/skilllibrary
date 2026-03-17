---
name: skill-variant-splitting
description: "Decides when one broad skill should be split into stack variants, platform variants, or micro-skills. Critical once a skill starts absorbing too many domains. Trigger when the task context clearly involves skill variant splitting."
source: created
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  category: meta-skill-engineering
  priority: P2
  maturity: draft
  risk: low
  tags: [variants, splitting, decomposition]
---

# Purpose
Determines when a skill has grown too broad and should be split into focused variants. A skill trying to do too many things has vague triggers, bloated procedures, and mediocre output. Splitting creates specialized variants that route precisely and perform excellently.

# When to use this skill
Use when:
- User says "this skill does too much", "split this skill", "create variants"
- Skill has multiple disjoint sections ("For Python:" / "For JavaScript:")
- Skill triggers on unrelated inputs (routing confusion)
- Procedure has many "if X then Y, else Z" branches
- Description >2 sentences trying to cover all cases
- Refinement keeps adding exceptions

Do NOT use when:
- Skill is focused and working well
- Variations minor (use overlays instead)
- Problem is just description (use `skill-description-optimizer`)
- Skill genuinely handles one coherent task

# Operating procedure
1. **Identify splitting signals**:
   - Count distinct "modes" (if/else, "For X:" sections)
   - Count distinct non-overlapping trigger clusters
   - Does output format vary by case?
   - Do different contexts need different behaviors?
2. **Determine split axis**:
   - **Stack**: skill-testing-python, skill-testing-javascript
   - **Platform**: skill-deploy-aws, skill-deploy-gcp
   - **Scope**: skill-review-quick, skill-review-thorough
   - **Domain**: skill-api-rest, skill-api-graphql
3. **Define variants**:
   - Each passes "one sentence" test
   - Each has distinct, non-overlapping triggers
   - Each has focused, linear procedure
4. **Extract shared core**:
   - What's common across all?
   - Consider: base with extensions, or fully independent
5. **Write each variant**:
   - Name: original-[variant]
   - Focused description
   - Simplified procedure
   - Variant-specific examples
6. **Update routing**:
   - Each "Do NOT use when" references siblings
   - "Do NOT use for GraphQL (use `api-testing-graphql`)"
7. **Verify coverage**:
   - All original cases covered
   - No gaps
   - No overlaps
8. **Deprecate original** (if fully replaced)

# Output defaults
```
## Variant Split: [original]

### Split Axis
[Stack | Platform | Scope | Domain]: [reasoning]

### Variants
| Variant | Scope | Triggers |
|---------|-------|----------|
| skill-a | [scope] | "phrase 1" |
| skill-b | [scope] | "phrase 2" |

### Shared Core
[What's common or "None - independent"]

### Coverage
- [x] Case 1 → variant A
- [x] Case 2 → variant B
- [x] No gaps/overlaps

### Migration
Original: [deprecate | keep as router | keep for general]
```

# References
- Original skill
- Similar splits in catalog

# Failure handling
- **Can't find clean axis**: May be unified; don't force split
- **Variants overlap**: Rethink axis or accept ambiguity
- **Too many (>5)**: Consider hierarchy or umbrella router
- **Core larger than variants**: Don't split—redundant
