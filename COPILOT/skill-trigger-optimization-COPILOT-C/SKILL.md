---
name: skill-trigger-optimization
description: "Improves trigger phrases, descriptions, and routing language so a host actually invokes the skill when it should. Important because undertriggering was called out repeatedly. Trigger when the task context clearly involves skill trigger optimization."
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
  tags: [triggers, routing, optimization]
---

# Purpose
Fixes skill routing problems by rewriting the description field and "When to use" triggers. The description is routing logic—it determines when a host invokes the skill. Bad descriptions cause undertriggering (doesn't fire when should) or overtriggering (fires when shouldn't).

# When to use this skill
Use when:
- Skill isn't triggering when it should (undertriggering)
- Skill is triggering when it shouldn't (overtriggering, false positives)
- User says "why isn't this skill being used?", "wrong skill fired", "fix the triggers"
- Eval shows poor routing accuracy (low precision or recall)
- Description is vague, generic, or reads like marketing copy

Do NOT use when:
- Skill triggers correctly but output wrong (use `skill-refinement`)
- Creating new skill (use `skill-authoring`)
- Entire skill needs rewrite (use `skill-authoring`)
- Problem is procedure, not routing

# Operating procedure
1. **Diagnose the routing problem**:
   - **Undertriggering**: What phrases should trigger but don't? List 3-5
   - **Overtriggering**: What triggered but shouldn't? What should have?
   - **Confusion**: Which skill being confused? What distinguishes them?
2. **Analyze current description**:
   - Is first phrase most discriminating signal?
   - Does it mention trigger words users actually say?
   - Does it describe what skill produces?
   - Does it have "Do NOT use when" anti-triggers?
3. **Identify discriminating signals**:
   - What words/phrases ONLY appear when this skill should trigger?
   - What context signals indicate this skill?
   - What's minimal set that reliably indicates this skill?
4. **Rewrite the description**:
   - **First phrase**: Most discriminating signal (verb + specific object)
   - **Include**: Specific trigger phrases users say
   - **Include**: What skill produces/does
   - **Avoid**: Generic ("helps with", "assists in")
   - **Avoid**: Marketing ("powerful", "comprehensive")
5. **Add explicit anti-triggers**:
   - "Do NOT use when: [confusion case] (use `alternative` instead)"
   - Cover most common false positive scenarios
6. **Test the new description**:
   - Would undertriggering phrases now match?
   - Would overtriggering phrases now NOT match?
   - Still risk of confusion?

# Output defaults
```
## Trigger Optimization: [skill-name]

### Problem
[Undertriggering | Overtriggering | Confusion]
Examples: [problematic inputs]

### Analysis
- Current first phrase: "[current]"
- Missing trigger words: [list]
- Overly generic terms: [list]

### Rewritten Description
**Before**: "[current]"
**After**: "[new with specific triggers]"

### Anti-triggers Added
- Do NOT use when: [case] (use `skill-x`)

### Verification
- [ ] Undertriggering cases now match
- [ ] Overtriggering cases now don't match
- [ ] No new confusion
```

# References
- Similar skills in catalog for differentiation
- Trigger test results if available

# Failure handling
- **Can't identify discriminating signals**: Skill may be too broad—recommend `skill-variant-splitting`
- **Every fix causes new false positives**: Overlapping scope—redesign boundaries
- **No usage data**: Create synthetic tests, optimize against those
- **Genuine overlap with another skill**: Consider merging or explicit routing rules
