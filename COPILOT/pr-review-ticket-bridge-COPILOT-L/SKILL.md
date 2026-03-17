---
name: pr-review-ticket-bridge
description: "Transforms validated review findings into guarded follow-up tickets without letting comments become unchecked backlog noise. Useful whenever external review or host-side PR analysis feeds back into the local backlog. Trigger when the task context clearly involves pr review ticket bridge."
source: github.com/scafforge/session
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  category: package-scaffolding
  priority: P1
  maturity: draft
  risk: low
  tags: [pr, review, tickets]
---

# Purpose
Validates PR review comments against actual implementation, then generates follow-up tickets only for findings that survive evidence-based triage. Prevents review comments from becoming unchecked backlog noise by requiring each finding to have verified evidence before ticket creation.

# When to use this skill
Use when:
- A PR has been reviewed and has comments that need action
- Converting review feedback into tracked work items
- Closing a review cycle and capturing remaining work

Do NOT use when:
- The review is in progress (wait for completion)
- All comments were addressed in the PR (nothing to bridge)
- Comments are discussions, not action items

# Operating procedure

## 1. Collect review comments
```bash
# From GitHub
gh pr view [PR_NUMBER] --comments --json reviews,comments

# Or from local review file
cat reviews/PR-[NUMBER]-review.md
```

## 2. Categorize comments
For each comment, determine type:
- **Must Fix**: Blocking issue that must be addressed
- **Should Fix**: Important but not blocking
- **Consider**: Suggestion for improvement
- **Discussion**: Question or conversation (no action needed)
- **Resolved**: Already addressed in PR

## 3. Validate "Must Fix" and "Should Fix" against code
For each actionable comment:
```markdown
### Comment: [summary]
**Reviewer claim:** [what they said]
**Code location:** [file:line]

**Validation:**
- [ ] Code at location still exists
- [ ] Issue described is actually present
- [ ] No subsequent commit addressed this

**Verdict:** [VALID | INVALID | OUTDATED]
```

Reasons to invalidate:
- Code was already fixed in later commit
- Comment misunderstood the code
- Underlying assumption was wrong

## 4. Triage validated findings
For each VALID finding, assess:
```markdown
**Impact:** [high | medium | low]
**Effort:** [small | medium | large]
**Blocks release:** [yes | no]

**Decision:** [CREATE_TICKET | ADDRESS_NOW | DEFER | WONTFIX]
```

Decision matrix:
| Impact | Effort | Blocks? | Decision |
|--------|--------|---------|----------|
| high | small | yes | ADDRESS_NOW |
| high | large | yes | CREATE_TICKET (P0) |
| high | any | no | CREATE_TICKET (P1) |
| medium | small | no | ADDRESS_NOW or DEFER |
| low | any | no | DEFER or WONTFIX |

## 5. Generate tickets for CREATE_TICKET items
For each ticket-worthy finding:
```markdown
---
id: TKT-[NEXT]
title: [action] from PR #[NUMBER] review
status: todo
priority: [P0|P1|P2]
source: PR-[NUMBER]-review
created: [ISO date]
---

# TKT-[NEXT]: [Title]

## Origin
PR #[NUMBER] review comment by [reviewer]
Original comment: "[quote]"

## Description
[Expanded description of what needs to be done]

## Acceptance Criteria
- [ ] [Specific criterion based on review comment]
- [ ] [Verification that addresses reviewer concern]

## Context
- PR: #[NUMBER]
- File: [file:line]
- Reviewer: [name]
```

## 6. Update PR with triage results
Comment on PR summarizing triage:
```markdown
## Review Triage Summary

### Addressed in PR
- [Comment 1]: Fixed in commit [sha]

### Tickets Created
- TKT-XXX: [title] (P1)
- TKT-YYY: [title] (P2)

### Deferred
- [Comment]: Reason for deferral

### Won't Fix
- [Comment]: Rationale
```

## 7. Close review cycle
```bash
# Approve PR if all blocking items addressed
gh pr review [PR_NUMBER] --approve --body "All blocking items addressed. Non-blocking items tracked in tickets."

# Or request changes if blockers remain
gh pr review [PR_NUMBER] --request-changes --body "Blocking items remain: [list]"
```

# Output defaults
```markdown
# PR Review → Ticket Bridge Report
PR: #[NUMBER]
Date: [ISO date]

## Summary
- Comments received: N
- Validated as issues: N
- Tickets created: N
- Addressed in PR: N
- Deferred: N

## Tickets Created
| ID | Title | Priority | Source Comment |
|----|-------|----------|----------------|
| TKT-XXX | [title] | P1 | [reviewer]: "[snippet]" |

## Full Triage
[Detailed breakdown per comment]
```

# References
- GitHub PR review workflow: https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/about-pull-request-reviews
- Review statuses: Comment, Approve, Request Changes

# Failure handling
- **Cannot access PR comments**: Manually extract comments from GitHub web UI
- **Comment references deleted code**: Mark as OUTDATED, do not create ticket
- **Disagreement with reviewer**: Create ticket but flag for discussion, don't silently WONTFIX
- **Too many comments to process**: Batch by file or severity, process in multiple passes
