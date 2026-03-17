---
name: skill-testing-harness
description: "Sets up repeatable prompts, fixtures, and comparison runs for skill development. This formalizes the test loop described in the creator docs. Trigger when the task context clearly involves skill testing harness."
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
  tags: [testing, harness, fixtures]
---

# Purpose
Creates test infrastructure for a skill: trigger tests (positive and negative), output tests (format and content), and baseline comparisons. The harness enables repeatable evaluation during development and refinement.

# When to use this skill
Use when:
- User says "create tests for this skill", "set up eval", "build test harness"
- New skill being created needs test coverage
- Skill lacks evals/ directory or test fixtures
- Skill refinement requires regression tests

Do NOT use when:
- Running existing tests (use `skill-eval-runner`)
- Evaluating quality (use `skill-evaluation`)
- Benchmarking performance (use `skill-benchmarking`)
- Tests exist and just need updating (edit directly)

# Operating procedure
1. **Analyze skill to test**:
   - What triggers it? (from description and "When to use")
   - What should NOT trigger? (from "Do NOT use when")
   - What output format?
   - What quality criteria?
2. **Create trigger tests** (`evals/triggers.yaml`):
   ```yaml
   positive_triggers:
     - input: "exact phrase that should trigger"
       expected: trigger
       notes: "core use case"
     - input: "variation that should also trigger"
       expected: trigger
   
   negative_triggers:
     - input: "phrase that should NOT trigger"
       expected: no_trigger
       better_skill: "skill that should handle this"
   ```
3. **Create output tests** (`evals/outputs.yaml`):
   ```yaml
   output_tests:
     - name: "basic case"
       input: "full prompt"
       expected_sections:
         - "## Required Section"
       required_patterns:
         - "must contain this"
       forbidden_patterns:
         - "should not contain"
   ```
4. **Create baseline comparisons** (`evals/baselines.yaml`):
   ```yaml
   baseline_comparisons:
     - name: "typical task"
       input: "prompt"
       baseline_context: "without skill"
       evaluation_criteria:
         - "correctness"
         - "completeness"
   ```
5. **Create test fixtures** (`evals/fixtures/`):
   - Sample input files
   - Mock data if needed
   - Expected output examples
6. **Document the harness** (`evals/README.md`):
   - How to run tests
   - What each file covers
   - How to add new tests

# Output defaults
```
skill-name/
├── SKILL.md
└── evals/
    ├── README.md
    ├── triggers.yaml
    ├── outputs.yaml
    ├── baselines.yaml
    └── fixtures/
```

# References
- https://docs.anthropic.com/en/docs/test-and-evaluate/eval-overview
- Existing evals/ in similar skills

# Failure handling
- **No clear triggers**: Can't write trigger tests—flag for `skill-description-optimizer`
- **Output format undefined**: Can't write output tests—flag for refinement
- **No comparable baseline**: Skip baseline tests, focus on absolute quality
- **Skill too complex**: Break into sub-capabilities, test each
