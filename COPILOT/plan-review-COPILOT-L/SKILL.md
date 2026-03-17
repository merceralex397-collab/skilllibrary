---
name: plan-review
description: "Reviews a proposed plan for completeness, sequencing, hidden assumptions, and execution readiness before implementation begins. You explicitly asked for this and GPTTalker already has a plan-review agent. Trigger when the task context clearly involves plan review."
source: github.com/gpttalker/agent-skills
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  category: planning-review-and-critique
  priority: P0
  maturity: draft
  risk: low
  tags: [plan-review, critique, readiness]
---

# Purpose
Critically reviews a plan before execution begins to identify gaps, hidden assumptions, incorrect sequencing, and unrealistic estimates. The goal is to catch problems while they're cheap to fix—before any code is written or resources committed.

# When to use this skill
Use when:
- User says "review this plan", "is this plan ready?", "check this before we start", or "sanity check this"
- A planning agent produced output that needs second-pass validation before handoff
- Implementation is about to begin and someone should verify the plan makes sense
- A ticket/brief will be handed to an implementer who shouldn't discover surprises mid-work

Do NOT use when:
- Plan is mid-execution and needs progress tracking (use `drift-detection`)
- User wants only risk brainstorming without reviewing a specific plan (use `premortem`)
- No plan exists yet—create one first (use `planning`)
- User wants estimation, not review (use `planning` with sizing)

# Operating procedure
1. **Check structural completeness**: Required sections: goal statement, acceptance criteria, task list, dependencies, constraints, and definition of done. Flag missing sections by name
2. **Validate sequencing**:
   - Draw implicit dependency graph. Flag any task listed after its dependent
   - Flag parallelizable work that's serialized unnecessarily
   - Flag concurrent tasks that share resources without acknowledgment
3. **Audit acceptance criteria**:
   - Each criterion must be observable and testable by someone unfamiliar with the work
   - Flag subjective language: "works correctly", "handles errors gracefully", "good performance"
   - Convert one vague criterion to concrete form as example
4. **Surface hidden assumptions**: List every unstated assumption. Categorize each:
   - **Verified**: You confirmed it (cite source)
   - **Plausible**: Reasonable default, state the risk if wrong
   - **Risky**: Could invalidate the plan; recommend verification step
5. **Identify critical path**: Name the sequence of blocking tasks that determines minimum duration. Flag any critical-path step that is vague, underspecified, or externally blocked
6. **Find missing work**: Common omissions to check:
   - Setup/teardown (environment, data, permissions)
   - Testing (unit, integration, manual verification)
   - Documentation updates
   - Migration/rollback procedures
   - Review/approval gates
7. **Estimate sanity check**: If estimates exist, flag any task >16 hours or any estimate lacking uncertainty acknowledgment
8. **Issue verdict**:
   - **Ready**: Complete, correctly sequenced, acceptable assumptions
   - **Ready with Caveats**: Executable but [list] needs attention during execution
   - **Not Ready**: [List] must be resolved before implementation

# Output defaults
```
## Plan Review: [Plan Name]

### Completeness
[Present/missing sections]

### Sequencing Issues
[Dependency problems or "None found"]

### Acceptance Criteria Quality
[Issues found or "All criteria testable"]

### Hidden Assumptions
| Assumption | Status | Risk if Wrong |
|------------|--------|---------------|

### Critical Path
[Task A → Task B → Task C] (N days)
Critical path concerns: [list or "None"]

### Missing Work
[List or "None identified"]

### Verdict: [Ready | Ready with Caveats | Not Ready]
**Blocking issues**: [list or "None"]
**Caveats**: [list or "None"]
```

# References
- https://cookbook.openai.com — planning patterns from OpenAI
- https://www.atlassian.com/agile/project-management/estimation — estimation calibration

# Failure handling
- **Plan is just bullet points**: State minimum structure needed, embed questions at each gap: "Goal: [WHAT IS THE OBSERVABLE OUTCOME?]"
- **Plan references external docs you can't access**: Note which sections cannot be reviewed and why; review what's available
- **Multiple interpretation possible**: State the two most likely interpretations, review against the more conservative one, flag ambiguity for author
