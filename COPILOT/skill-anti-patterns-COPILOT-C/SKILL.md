---
name: skill-anti-patterns
description: "Collects common ways skills fail: vague scope, weak triggers, broad but shallow bodies, or missing evals. Handy as a QA lens when editing skills. Trigger when the task context clearly involves skill anti patterns."
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
  tags: [anti-patterns, quality, review]
---

# Purpose
Identifies specific anti-patterns in SKILL.md that reduce reliability, trigger accuracy, or output quality. Provides checklist with fixes. Use as QA lens when authoring, refining, or reviewing skills.

# When to use this skill
Use when:
- User says "check for anti-patterns", "what's wrong?", "review against standards"
- Skill being reviewed before promotion draft → stable
- Skill producing unexpected outputs
- Quick audit needed for structural issues

Do NOT use when:
- Full rewrite needed (use `skill-authoring`)
- Only triggers need fixing (use `skill-trigger-optimization`)
- Skill confirmed working—don't scan healthy skills

# Operating procedure

Check for each anti-pattern. For each PRESENT, provide specific fix.

**AP-1: Circular trigger language**
- Pattern: "Use when task clearly involves X"
- Fix: Replace with specific phrases user would say

**AP-2: Boilerplate procedure steps**
- Pattern: "Keep scope explicit", "Prefer references", "Decide whether belongs in core"
- Fix: Delete—meta-commentary from template, not instructions

**AP-3: Generic output defaults**
- Pattern: "Structured markdown artifact(s) with clear next steps"
- Fix: Name specific format—sections, tables, labels

**AP-4: Generic failure handling**
- Pattern: "If inputs missing, surface gap as decision item"
- Fix: Name specific most-common failure and response

**AP-5: Overloaded purpose**
- Pattern: Purpose has "and" 2+ times, covers different task families
- Fix: Split (use `skill-variant-splitting`) or narrow

**AP-6: Unmeasurable acceptance**
- Pattern: Output can't be evaluated without human judgment ("good", "appropriate")
- Fix: Observable criteria—section names, counts, labels

**AP-7: Missing Do NOT use when**
- Pattern: No section or empty
- Fix: Add 1-3 confusion cases with alternatives

**AP-8: Missing failure handling**
- Pattern: No section
- Fix: Add covering most common failure mode

**AP-9: Vague procedure steps**
- Pattern: "Consider", "Think about", "You might want to"
- Fix: Concrete action verbs—"Write", "Check", "List"

**AP-10: No references**
- Pattern: No URLs to authoritative sources
- Fix: Add real documentation links

# Output defaults
```
## Anti-Pattern Audit: [skill-name]

| ID | Pattern | Status | Fix |
|----|---------|--------|-----|
| AP-1 | Circular triggers | PRESENT | Replace "clearly involves" with "when user says X" |
| AP-2 | Boilerplate steps | ABSENT | — |
...

### Summary
- PRESENT: 3 (AP-1, AP-4, AP-7)
- ABSENT: 7

### Priority Fixes
1. AP-1: [specific fix]
2. AP-4: [specific fix]
3. AP-7: [specific fix]
```

If all ABSENT: "**Clean Bill of Health** — no anti-patterns detected"

# References
- This skill's checklist
- Skill authoring guidelines

# Failure handling
- **File unavailable**: Report and halt
- **Skill too broken to audit**: Recommend full rewrite via `skill-authoring`
- **Multiple interpretations**: Flag ambiguity, audit conservatively
