---
name: skill-eval-runner
description: "Runs trigger tests, behavior tests, baseline comparisons, and report aggregation for skills. The architecture docs and Claude creator loop both imply this as mandatory infrastructure. Trigger when the task context clearly involves skill eval runner."
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
  tags: [eval, testing, benchmark]
---

# Purpose
Executes a skill's eval suite (from evals/ directory) and produces structured report covering trigger accuracy, behavior correctness, and baseline comparison. Interprets results and generates quality verdict.

# When to use this skill
Use when:
- User says "run the eval", "test this skill", "run the tests"
- Skill modified and needs regression testing before promotion
- CI/pre-release validation requires documented eval
- Quality measurement before catalog/release inclusion

Do NOT use when:
- No evals/ exists—build first (use `skill-testing-harness`)
- Manual evaluation without tests (use `skill-evaluation`)
- Benchmarking variants (use `skill-benchmarking`)

# Operating procedure
1. **Locate test suite**:
   - Check for `evals/triggers.yaml`, `outputs.yaml`, `baselines.yaml`
   - Note which exist/missing
2. **Run trigger tests** (from triggers.yaml):
   - Each positive: Does skill fire? (target: Yes)
   - Each negative: Does skill NOT fire? (target: No)
   - Record: prompt, expected, actual, Pass/Fail
3. **Run output tests** (from outputs.yaml):
   - Run skill with input
   - Check against expected_sections, required_patterns, forbidden_patterns
   - Record: test, each check, Pass/Fail
4. **Run baseline comparison** (from baselines.yaml):
   - Run with skill active
   - Note expected elements present
   - Compare to baseline expected
   - Skill adds value? (Yes if 2+ elements baseline would lack)
5. **Aggregate results**:
   - Trigger precision: TP / (TP + FP)
   - Trigger recall: TP / (TP + FN)
   - Output pass rate: passed / total
   - Baseline win: Yes/No
6. **Issue verdict**:
   - **Pass**: All rates ≥80%, baseline win
   - **Pass with issues**: Any rate 60-79%
   - **Fail**: Any rate <60% or baseline lose

# Output defaults
```
## Eval Report: [skill-name]
**Date**: YYYY-MM-DD
**Version**: X.Y.Z

### Trigger Tests
| Prompt | Type | Expected | Actual | Result |
|--------|------|----------|--------|--------|
| "deploy app" | positive | trigger | trigger | ✓ |

**Precision**: X%  **Recall**: Y%

### Output Tests
| Test | Checks Passed | Result |
|------|---------------|--------|
| basic | 5/5 | ✓ |

**Pass rate**: X%

### Baseline Comparison
**Skill adds value**: Yes/No
**Evidence**: [elements present vs baseline]

### Verdict: [Pass | Pass with issues | Fail]
**Issues**: [list or "None"]
```

# References
- Skill's evals/ directory
- https://docs.anthropic.com/en/docs/test-and-evaluate/eval-overview

# Failure handling
- **Silent skill failure** (no output): Record Fail "No output", halt remaining tests for case
- **Missing test files**: Report which missing, run available ones
- **Flaky results**: Run 3x, majority rules, note flakiness
