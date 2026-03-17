---
name: reverse-brainstorming
description: "Asks how to make the plan fail, become slow, or become unreliable, then inverts the answers into protections. A practical hole-poking technique. Trigger when the task context clearly involves reverse brainstorming."
source: created
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  category: planning-review-and-critique
  priority: P1
  maturity: draft
  risk: low
  tags: [brainstorming, adversarial, critique]
---

# Purpose
Generates protective measures by first asking how to deliberately cause the problem, then inverting each sabotage into a corresponding safeguard. This is the inversion technique: instead of asking "how do we succeed?", ask "how would we guarantee failure?" The answers reveal vulnerabilities that normal planning misses because teams are anchored on success scenarios.

# When to use this skill
Use when:
- The user says "reverse brainstorm this", "how would we sabotage this?", "how could we make this fail on purpose?", or "what would kill this?"
- Conventional risk analysis isn't surfacing novel risks—the team is too familiar with the plan
- A creative approach is needed to break out of groupthink about what could go wrong
- The user wants to generate specific protective measures, not just a risk list
- The team is stuck in "how do we make this work?" mode and needs to flip perspective

Do NOT use when:
- The user wants structured risk analysis with likelihood/severity ratings (use `failure-mode-analysis`)
- The user wants to trace a past failure (use `root-cause-analysis`)
- The idea is too abstract to generate concrete sabotage actions
- The user wants adversarial security review (use `red-team-challenge`)

# Operating procedure
1. **State the goal clearly**: Write one sentence defining success for this plan:
   "The plan succeeds if [observable outcome]."
   
   This defines what we're trying to sabotage.

2. **Invert the goal**: Flip the question:
   "How would we guarantee this plan fails / delivers late / produces wrong results / gets abandoned / exceeds budget / harms users?"
   
   Choose the failure mode most relevant to the plan.

3. **Generate 10-15 sabotage actions**: Brainstorm concrete, specific ways to cause failure. No filtering yet—be creative, specific, and adversarial.
   
   Good sabotage examples:
   - "Delete the test suite on day 1"
   - "Add 5 external dependencies with no SLAs"
   - "Assign the most critical task to the newest team member with no documentation"
   - "Never write down decisions—keep everything in Slack"
   - "Change requirements every week"
   - "Skip code review for 'urgent' fixes"
   - "Don't set up monitoring until after launch"
   
   Bad examples (too vague):
   - "Communicate poorly"
   - "Don't plan well"

4. **Group sabotage actions by theme**: Cluster into 3-5 themes:
   - Technical debt accumulation
   - Coordination/communication breakdown
   - Incentive misalignment
   - Scope creep / requirement instability
   - Operational blindness
   - Resource starvation
   - Knowledge silos

5. **Invert each sabotage into a protection**: The inversion should be direct:
   
   | Sabotage | Protection |
   |----------|------------|
   | Delete the test suite | Enforce test coverage thresholds in CI that block merge |
   | No SLAs on dependencies | Require SLA documentation before approving new dependencies |
   | Critical tasks to newest person | Pair programming for critical path items |
   | Keep decisions in Slack | Decision log with mandatory ADR for architectural choices |

6. **Rate protection value**: For each protection:
   - **High value**: Prevents multiple failure modes, low effort to implement
   - **Medium value**: Prevents specific failure mode, moderate effort
   - **Low value**: Only prevents narrow sabotage, high effort or already covered

7. **Select top 5 protections**: The highest-value protections not already in the plan. These are the recommended additions.

# Output defaults
A **Sabotage List** grouped by theme, with each sabotage as a concrete action.

A **Sabotage → Protection Mapping** table with columns: Sabotage Action | Protection | Value (H/M/L)

A **Top 5 Protections to Add** section with:
- Each protection described
- How to implement it (specific, actionable)
- Which sabotages it prevents

# References
- Reverse brainstorming / negative brainstorming technique — creative problem-solving methodology
- Inversion mental model (Charlie Munger) — "invert, always invert"
- Pre-mortem variations that focus on causation

# Failure handling
If the plan is too vague to generate specific sabotage actions:
1. List the three aspects of the plan that need to be more concrete
2. Provide example sabotages conditional on those aspects being defined
3. Generate sabotages for the aspects that ARE concrete, clearly marking scope
