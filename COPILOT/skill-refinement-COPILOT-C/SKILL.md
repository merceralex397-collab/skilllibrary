---
name: skill-refinement
description: "Tightens, clarifies, and de-bloats an existing skill after real usage reveals rough edges. You explicitly asked for this and the creator docs support it. Trigger when the task context clearly involves skill refinement."
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
  tags: [refinement, editing, improvement]
---

# Purpose
Iteratively improves an existing skill based on observed failure modes, user feedback, or eval results. Focuses on surgical fixes—tightening vague language, fixing routing misses, removing bloat, clarifying edge cases—not rewrites.

# When to use this skill
Use when:
- User says "this skill isn't working right", "improve this skill", "the skill is too verbose/vague"
- Skill exists but produces inconsistent or low-quality output
- Eval results show specific failure patterns to address
- Skill triggers incorrectly (over/under firing)
- Real usage revealed edge cases not covered

Do NOT use when:
- Skill needs complete rewrite (use `skill-authoring`)
- Only description field needs work (use `skill-description-optimizer`)
- Skill needs different context (use `skill-adaptation`)
- Skill is working correctly—no change needed

# Operating procedure
1. **Collect evidence of the problem**:
   - What specific output was wrong?
   - What input triggered it?
   - How does expected differ from actual?
   - Is this routing problem or output quality problem?
2. **Categorize the failure mode**:
   - **Routing miss**: Skill didn't trigger when should → fix description
   - **Routing false positive**: Triggered when shouldn't → fix description, add "Do NOT use when"
   - **Wrong output format**: Structure off → fix Output defaults
   - **Missing edge case**: Scenario not handled → add to Operating procedure
   - **Vague instructions**: Procedure not specific → tighten language
   - **Scope creep**: Doing things outside job → add constraints
   - **Bloat**: Unnecessary steps → remove
3. **Make targeted fix**:
   - Change only what addresses the specific failure
   - Preserve everything working
   - Add test case for failure to evals/ if exists
4. **Verify fix doesn't break existing behavior**:
   - Run existing evals if available
   - Walk through known good cases mentally
   - Check routing changes don't cause new false positives/negatives
5. **Document the change**: What failure mode, what changed, date and evidence source

# Output defaults
```
## Refinement Summary

**Skill**: [name]
**Failure mode**: [category]
**Evidence**: [what went wrong]

### Change
**Section**: [which section]
**Before**: [original]
**After**: [new]
**Rationale**: [why this fixes it]

### Verification
- [ ] Existing evals still pass
- [ ] Known good cases still work
- [ ] Failure case now handled
```

# References
- The skill being refined
- Eval results or failure logs if available
- `skill-anti-patterns` checklist

# Failure handling
- **No clear failure mode**: Don't refine speculatively—ask for specific example of what went wrong
- **Multiple failure modes**: Address one at a time, verify, then next
- **Failure suggests fundamental design problem**: Recommend `skill-authoring` for rewrite
- **Can't reproduce failure**: Get exact input that caused it before attempting fix
