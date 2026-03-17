---
name: handoff-brief
description: "Builds a concise restart surface with actual state, next steps, and reading order. Long-running or interrupted work needs a compact re-entry surface. Trigger when the task context clearly involves handoff brief."
source: github.com/scafforge/session
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  category: package-scaffolding
  priority: P0
  maturity: draft
  risk: low
  tags: [handoff, context, restart]
---

# Purpose
Creates a concise, structured handoff document at session end or milestone completion. Captures what was done, what's next, what's blocked, and exactly how to resume—so the next session or agent can restart without re-reading everything. This is the complement to project-context: that skill loads context, this skill creates it.

# When to use this skill
Use when:
- Ending a work session with incomplete work
- Completing a milestone and preparing for next phase
- Handing off to a different agent or human
- Context window approaching limit and need to persist state

Do NOT use when:
- Work is fully complete with nothing pending
- Just starting work (use project-context instead)
- Quick task that doesn't warrant formal handoff

# Operating procedure

## 1. Capture current state
Gather facts, not opinions:
```bash
# Git state
git branch --show-current
git log --oneline -5
git status --short
git diff --stat

# Ticket state
cat tickets/BOARD.md | grep -A20 "In Progress"

# Test state
npm test 2>&1 | tail -20  # Or equivalent
```

## 2. Structure the handoff document

### Template: START-HERE.md
```markdown
# Handoff Brief
Generated: [ISO timestamp]
Session: [session-id if applicable]

## TL;DR
[One sentence: what state is the project in right now]

## What Was Done This Session
- [Completed item 1]
- [Completed item 2]
- [Partial progress on item 3]

## Current State

### Branch
`[branch-name]` — [N commits ahead of main]

### Uncommitted Changes
```
[git status --short output]
```

### Test Status
[PASSING | FAILING: N failures | NOT RUN]

### Tickets
| ID | Status | Progress |
|----|--------|----------|
| TKT-042 | in_progress | 70% - auth endpoint done, tests pending |
| TKT-043 | blocked | Waiting on TKT-042 |

## What's Next
Priority order for next session:

1. **[Most important next step]**
   - Why: [rationale]
   - How: [specific command or action]
   
2. **[Second priority]**
   - Why: [rationale]
   - How: [specific action]

## Blockers
| Blocker | Impact | Resolution |
|---------|--------|------------|
| [Description] | Blocks TKT-043 | [What's needed to unblock] |

## Context to Load
Read these files to resume:
1. This file (START-HERE.md)
2. `tickets/TKT-042-auth.md` — active ticket
3. `src/auth/` — files being modified

## Warnings
- [Any traps, gotchas, or things to be careful about]
- [Decisions that were deferred and why]

## Commands to Resume
```bash
# Verify state matches handoff
git status
npm test

# Continue from where we left off
[specific command to resume]
```
```

## 3. Place handoff correctly
```bash
# For session handoffs (temporary)
cat > START-HERE.md << 'EOF'
[handoff content]
EOF

# For milestone handoffs (persistent)
mkdir -p docs/handoffs
cat > docs/handoffs/[milestone]-[date].md << 'EOF'
[handoff content]
EOF
```

## 4. Validate handoff completeness

Checklist before finalizing:
- [ ] TL;DR accurately summarizes state
- [ ] All in-progress work mentioned
- [ ] Test status is current
- [ ] Blockers have resolution paths
- [ ] "What's Next" is actionable
- [ ] Commands to resume actually work

## 5. Commit handoff (if appropriate)
```bash
# For persistent handoffs
git add START-HERE.md docs/handoffs/
git commit -m "docs: handoff brief for [milestone/session]

State: [brief summary]
Next: [primary next action]"
```

## 6. Example handoffs

### Example: End of implementation session
```markdown
# Handoff Brief
Generated: 2024-01-15T17:30:00Z

## TL;DR
Auth endpoint implemented and tested, PR ready for review.

## What Was Done
- Implemented /api/auth/login endpoint
- Added JWT token generation
- Wrote 12 tests (all passing)
- Opened PR #47

## Current State
Branch: `tkt-042-auth` — 8 commits ahead of main
Tests: PASSING (47/47)
PR: #47 open, awaiting review

## What's Next
1. **Address PR review comments** (when received)
2. **Start TKT-043** (unblocked once 042 merges)

## Commands to Resume
```bash
git checkout tkt-042-auth
gh pr view 47
```
```

### Example: Blocked mid-ticket
```markdown
# Handoff Brief
Generated: 2024-01-15T14:00:00Z

## TL;DR
Blocked on external API access, partial implementation committed.

## What Was Done
- Set up API client structure
- Implemented auth flow skeleton
- Hit blocker: need API credentials

## Current State
Branch: `tkt-045-integration` — 3 commits
Tests: NOT RUN (missing credentials)
Uncommitted: None (all committed)

## Blockers
| Blocker | Impact | Resolution |
|---------|--------|------------|
| Missing API_KEY env var | Cannot test integration | Request from DevOps |

## What's Next
1. **Get API credentials** from DevOps (ticket DEVOPS-123)
2. **Add to .env.local** and verify connection
3. **Continue with data fetch implementation**

## Warnings
- Do NOT commit any credentials
- API has rate limits (100 req/min)
```

# Output defaults
```markdown
# Handoff Brief
Generated: [timestamp]

## TL;DR
[One sentence summary]

## What Was Done
[List]

## Current State
[Branch, tests, tickets]

## What's Next
[Ordered list with how/why]

## Blockers
[Table if any]

## Commands to Resume
[Exact commands]
```

# References
- Complement to project-context (load) and docs-and-handoff (maintain)
- Shape Up "Move On" chapter: clean handoffs between cycles

# Failure handling
- **Incomplete information**: Better to handoff with gaps noted than no handoff
- **Tests failing**: Document which tests fail and why, don't hide failures
- **Uncommitted changes**: Either commit or explicitly list what's uncommitted and why
- **Unclear next steps**: At minimum, state "Needs planning before continuing"
