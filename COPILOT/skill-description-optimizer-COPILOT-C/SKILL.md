---
name: skill-description-optimizer
description: "Improves skill descriptions so routing triggers fire more reliably without overfiring. Your research highlighted undertriggering and the need for pushier descriptions. Trigger when the task context clearly involves skill description optimizer."
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
  tags: [description, routing, optimization]
---

# Purpose
Rewrites a skill's `description` field to fix routing problems. The description is routing logic that determines when hosts invoke the skill—it's not documentation or marketing copy. Bad descriptions cause undertriggering or overtriggering.

# When to use this skill
Use when:
- User says "optimize the description", "fix routing", "skill isn't being found"
- Evaluation shows low trigger recall (missing fires)
- Batch audit of descriptions for library release
- Description is passive, vague, or capability-statement language

Do NOT use when:
- Full "When to use" section needs rewriting (use `skill-trigger-optimization`)
- Skill scope is the problem, not text
- Description performing correctly

# Operating procedure
1. **Read current description**:
   - Length?
   - Style? (capability vs trigger statement)
   - Discriminating power?
2. **Identify failure mode**:
   - **Undertrigger**: Too narrow, technical, missing user words
   - **Overtrigger**: Too broad, overlaps siblings, no discrimination
   - **Passive**: Describes what skill *is* not when to invoke
3. **Extract discriminating signals**:
   - What words predict THIS skill (not siblings)?
   - Specific task verbs, domain terms, explicit names
4. **Write new description**:
   - Sentence 1: Action + most discriminating signal (lead with verb or name)
   - Sentence 2: Problem solved or context
   - Sentence 3: "Trigger when: [specific condition]"
   - Total: 40-70 words, no padding
5. **Check sibling conflicts**:
   - Read 2-3 similar skill descriptions
   - Does new description discriminate correctly?
   - Add distinguishing clause if needed
6. **Validate with test prompts**:
   - 5 positive cases: Should all match
   - 3 negative cases: Should none match

# Output defaults
```
## Description Optimization: [skill-name]

### Failure Mode
[Undertrigger | Overtrigger | Passive]

### Original
"[current description]"

### Optimized
"[new description]"

### Discrimination Check
| Similar Skill | How Differentiated |
|---------------|-------------------|
| skill-x | This uses "deploy", that uses "configure" |

### Test Results
| Case | Type | Matches? | Expected |
|------|------|----------|----------|
| "deploy to prod" | positive | ✓ | ✓ |
| "configure settings" | negative | ✗ | ✗ |
```

# References
- Sibling skill descriptions
- Trigger test results if available

# Failure handling
- **Scope ambiguous/overlaps at procedure level**: Description can't fix—recommend `skill-variant-splitting` or `skill-catalog-curation`
- **No discriminating signals exist**: Skill may be too generic—narrow scope
- **Can't test**: Create synthetic tests based on intended use
