---
name: repo-process-doctor
description: "Audits the generated operating layer and repairs or replaces managed surfaces after process upgrades. You asked for a true repair path for sweeping repo changes; this is central. Trigger when the task context clearly involves repo process doctor."
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
  tags: [repair, process, audit]
---

# Purpose
Audits an existing repository's agent workflows for drift, inconsistencies, and workflow smells. Identifies where the agent operating layer has diverged from intended process—contradictory status semantics, missing state management tools, unsafe delegation patterns—and produces a repair plan or executes fixes.

# When to use this skill
Use when:
- Agents are behaving inconsistently or producing unexpected results
- Ticket statuses don't match actual work state
- Multiple process documents exist with conflicting instructions
- Upgrading a repo's agent infrastructure after a scaffolding version change
- Onboarding to a repo that "used to work" but now has issues

Do NOT use when:
- Setting up a new repo (use scaffold-kickoff instead)
- The repo has no agent infrastructure to audit
- Issues are with code logic, not process/workflow

# Operating procedure

## 1. Inventory process artifacts
Scan for all process-related files:
```bash
find . -name "AGENTS.md" -o -name "BOARD.md" -o -name "*.md" -path "*/tickets/*" \
  -o -name "workflow*.json" -o -name "*.yaml" -path "*/.opencode/*" 2>/dev/null
```

## 2. Check for workflow smells

### Smell: Contradictory status semantics
```bash
# Find all status definitions
grep -r "status:" tickets/ docs/ AGENTS.md 2>/dev/null | sort -u
```
**Red flag**: Different files define different status values (e.g., "done" vs "completed" vs "closed").

### Smell: Missing state tools
```bash
# Check for state management
ls .opencode/tools/*state* .opencode/tools/*status* 2>/dev/null
```
**Red flag**: Tickets exist but no tools to read/update them programmatically.

### Smell: Unsafe read-only delegation
```bash
# Check agent definitions for write permissions
grep -r "permissions:" .opencode/agents/ 2>/dev/null
grep -r "read-only" .opencode/agents/ AGENTS.md 2>/dev/null
```
**Red flag**: Research agents have write permissions; implementation agents are read-only.

### Smell: Orphaned tickets
```bash
# Tickets in in_progress with no recent commits
for ticket in tickets/*in_progress*; do
  id=$(basename "$ticket" | cut -d- -f1,2)
  if ! git log --oneline -1 --grep="$id" 2>/dev/null; then
    echo "Orphaned: $ticket"
  fi
done
```

### Smell: Circular dependencies
```bash
# Extract and check dependency graph
grep -h "depends_on:" tickets/*.md | # Parse and detect cycles
```

### Smell: Stale board
```bash
# Compare BOARD.md age to ticket files
board_age=$(stat -c %Y tickets/BOARD.md 2>/dev/null || echo 0)
latest_ticket=$(stat -c %Y tickets/*.md 2>/dev/null | sort -n | tail -1)
if [ "$board_age" -lt "$latest_ticket" ]; then
  echo "BOARD.md is stale"
fi
```

## 3. Generate diagnosis report
```markdown
# Process Doctor Report
Generated: [timestamp]

## Summary
- Files scanned: N
- Issues found: N (X critical, Y warnings)

## Critical Issues
| Issue | Location | Impact | Fix |
|-------|----------|--------|-----|
| Contradictory status | tickets/, AGENTS.md | Agent confusion | Standardize to [done, in_progress, blocked, todo] |

## Warnings
| Issue | Location | Recommendation |
|-------|----------|----------------|
| BOARD.md stale | tickets/BOARD.md | Regenerate from ticket files |

## Health Score
[GREEN|YELLOW|RED] - [explanation]
```

## 4. Repair actions

### Standardize status values
```bash
# Create canonical status enum
cat > .opencode/status-values.json << 'EOF'
{
  "valid_statuses": ["todo", "in_progress", "review", "done", "blocked"],
  "aliases": {
    "completed": "done",
    "closed": "done",
    "pending": "todo",
    "wip": "in_progress"
  }
}
EOF

# Update tickets using aliases
for file in tickets/*.md; do
  sed -i 's/status: completed/status: done/g' "$file"
  sed -i 's/status: closed/status: done/g' "$file"
done
```

### Regenerate BOARD.md
```bash
# Parse tickets and rebuild board
echo "# Project Board" > tickets/BOARD.md
echo "" >> tickets/BOARD.md
for status in todo in_progress review done blocked; do
  echo "## ${status^}" >> tickets/BOARD.md
  grep -l "status: $status" tickets/*.md 2>/dev/null | while read f; do
    title=$(grep "^title:" "$f" | cut -d: -f2-)
    echo "- $(basename $f .md):$title" >> tickets/BOARD.md
  done
  echo "" >> tickets/BOARD.md
done
```

### Fix orphaned tickets
```bash
# Set orphaned in_progress tickets to blocked
for ticket in [orphaned tickets from step 2]; do
  sed -i 's/status: in_progress/status: blocked/' "$ticket"
  echo "blocked_reason: No activity detected" >> "$ticket"
done
```

## 5. Commit repairs
```bash
git add -A
git commit -m "chore: process doctor repairs

- Standardized status values
- Regenerated BOARD.md
- Marked N orphaned tickets as blocked
- [other repairs]"
```

# Output defaults
```markdown
# Process Doctor Report
[Diagnosis as above]

## Repairs Applied
- [List of changes made]

## Manual Actions Required
- [Any issues that couldn't be auto-fixed]
```

# References
- Workflow observability: for ongoing monitoring after repair
- Ticket execution: for the canonical stage workflow being enforced

# Failure handling
- **No process artifacts found**: Report "No agent infrastructure detected. Use scaffold-kickoff to create."
- **Conflicts in repair**: If auto-fix would lose data, stop and list conflicting files for manual resolution
- **Unable to parse ticket format**: Flag specific files as malformed, continue with parseable files
