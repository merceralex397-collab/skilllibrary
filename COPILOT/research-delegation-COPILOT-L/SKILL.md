---
name: research-delegation
description: "Delegates read-only evidence gathering to narrow lanes and persists durable findings as artifacts. Useful for keeping research separate from mutation. Trigger when the task context clearly involves research delegation."
source: github.com/gpttalker/opencode-skills
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  category: generated-repo-core
  priority: P1
  maturity: draft
  risk: low
  tags: [research, delegation, read-only]
---

# Purpose
Delegates research tasks to sub-agents with precise scope definitions, explicit output contracts, and synthesis instructions. Keeps information gathering cleanly separated from code mutation—research agents read and report, implementation agents act on findings. Prevents the common failure mode of "research" that actually makes changes.

# When to use this skill
Use when:
- Need to understand a codebase area before making changes
- Gathering evidence from multiple sources (files, docs, web)
- Preparing a decision with supporting analysis
- The research could be parallelized across multiple sub-agents

Do NOT use when:
- The question can be answered by reading one file (just read it)
- You need to make changes during investigation (that's implementation)
- The "research" is actually prototyping or experimentation

# Operating procedure

## 1. Define research scope precisely
A good research delegation includes:
```markdown
## Research Task: [Descriptive name]

### Question
[Single, specific question to answer]

### Scope
- Files/directories to examine: [explicit list or pattern]
- External sources (if any): [URLs, APIs]
- Time/token budget: [limit]

### Output Contract
Return findings in this format:
- Answer: [direct answer to the question]
- Evidence: [file:line citations]
- Confidence: [high/medium/low with rationale]
- Gaps: [what couldn't be determined]

### Constraints
- READ ONLY: Do not modify any files
- Do not execute code that has side effects
- Do not make network requests unless explicitly listed
```

## 2. Parallelize independent research
If multiple questions are independent, delegate in parallel:
```
Research Task A: How does authentication work?
  Scope: src/auth/
  
Research Task B: What database schema exists?
  Scope: src/models/, migrations/
  
Research Task C: What are the API endpoints?
  Scope: src/routes/, docs/api.md
```

## 3. Enforce read-only constraint
Research agents must not:
- Create files (except findings artifact)
- Modify existing files
- Run commands that mutate state
- Make commits

Allowed actions:
- `cat`, `grep`, `find`, `tree`
- `git log`, `git show`, `git blame`
- Read-only API calls
- Web fetches for documentation

## 4. Require evidence-backed findings
Every claim must have a citation:
```markdown
## Finding: User authentication uses JWT

**Evidence:**
- `src/auth/jwt.ts:15` - JwtStrategy class definition
- `src/auth/guards.ts:8` - @UseGuards(JwtAuthGuard) decorator
- `package.json:12` - "@nestjs/jwt": "^10.0.0" dependency

**Confidence:** High - multiple corroborating sources
```

## 5. Synthesize delegated research
After research tasks complete, synthesize:
```markdown
## Research Synthesis: [Topic]

### Summary
[Combined findings in 2-3 sentences]

### Key Findings
1. [Finding from Task A]
2. [Finding from Task B]
3. [Finding from Task C]

### Conflicts/Uncertainties
- [Any contradictory findings]
- [Gaps that remain]

### Implications for Implementation
- [How findings affect next steps]
```

## 6. Persist findings as artifacts
Save research results for future reference:
```bash
mkdir -p docs/research
cat > docs/research/[topic]-[date].md << 'EOF'
# Research: [Topic]
Date: [ISO date]
Researcher: [agent-id]

[Synthesis content]
EOF
```

# Output defaults
```markdown
# Research Findings: [Topic]

## Question
[Original question]

## Answer
[Direct answer, 1-3 sentences]

## Evidence
| Claim | Source | Confidence |
|-------|--------|------------|
| [claim] | [file:line] | high/medium/low |

## Gaps
- [What couldn't be determined]

## Raw Findings
[Detailed findings from each research task]
```

# References
- Read-only delegation prevents accidental mutations during investigation
- Evidence-based findings enable verification and reduce hallucination

# Failure handling
- **Scope too broad**: If research would require reading >50 files, narrow scope or break into sub-tasks
- **No evidence found**: Report "No evidence found in specified scope" rather than guessing
- **Conflicting evidence**: Report both sources and the conflict, don't resolve silently
- **Research agent made changes**: Revert immediately, flag as constraint violation, retry with stricter enforcement
