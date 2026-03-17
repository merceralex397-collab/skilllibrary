---
name: skeptic-pass
description: "Runs a deliberately doubtful review over a proposal to catch optimism bias and hand-wavy claims. A lightweight critique pass for early-stage thinking. Trigger when the task context clearly involves skeptic pass."
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
  tags: [skeptic, critique, review]
---

# Purpose
Reviews a proposal with deliberate skepticism to surface optimism bias, hand-waving, unsupported claims, and wishful thinking before the idea gains momentum. This is the lightweight critical pass for early-stage proposals—less structured than red-teaming, focused on puncturing unfounded confidence rather than systematic attack.

# When to use this skill
Use when:
- The user says "be skeptical about this", "poke holes", "what am I missing?", "sanity check this", or "devil's advocate"
- A proposal is in early stages and needs a reality check before resources are committed
- The author is too close to the idea and needs external challenge
- A plan is being approved without genuine scrutiny
- The team seems overly enthusiastic and confirmation bias may be operating

Do NOT use when:
- The user wants systematic adversarial attack on a finalized design (use `red-team-challenge`)
- The user wants only upside identified (use `steelman`)
- The idea is mature and needs structured risk analysis (use `premortem`)
- The user wants tradeoff comparison (use `tradeoff-analysis`)

# Operating procedure
1. **Adopt skeptic posture**: Read the proposal assuming every optimistic claim is likely wrong until proven otherwise. This is the operating stance—default to doubt unless evidence is provided.

2. **Flag unsupported claims**: Identify any assertion of fact that has:
   - No evidence cited
   - No source referenced
   - No logical derivation shown
   
   For each, note: "This claim is unsupported. What evidence backs it?"

3. **Identify the best version first**: Before critiquing, state in one sentence what the proposal is trying to achieve and what would need to be true for it to succeed. This ensures you're skeptical of the real proposal, not a strawman.

4. **Apply the six skeptic lenses**:

   **Timeline skepticism**: Is the schedule based on wishful thinking? What is the track record for similar work? Double the estimate—does it still make sense?

   **User behavior skepticism**: Does this assume users will change habits, learn new things, or act against their convenience? People rarely do.

   **Technical complexity skepticism**: Is the hardest part treated as solved or easy? Is there a "then a miracle occurs" step in the technical plan?

   **Dependency skepticism**: Does this rely on third parties acting predictably, quickly, or generously? What's their incentive?

   **Incentive alignment skepticism**: Are the incentives of all participants actually aligned with success? Who benefits from this failing?

   **Second-order effects skepticism**: What else changes if this works? Are those effects accounted for? What breaks?

5. **Rank the top five skeptic items**: The claims or assumptions that are:
   - Most likely to be wrong
   - Most consequential if they are wrong
   
   These are the proposal's soft underbelly.

6. **Deliver a verdict**: One of:
   - "Worth pursuing with these caveats: [list specific caveats]"
   - "High risk—needs these changes before proceeding: [list specific changes]"
   - "Not viable in current form because: [specific reason]"

# Output defaults
An **Unsupported Claims** list, each with what evidence would be needed.

A **Six Lenses Review** section with findings from each lens (skip lenses that don't apply).

A **Top 5 Skeptic Items** ranked list—the things most likely to be wrong and most damaging if so.

A one-sentence **Verdict** with clear recommendation.

# References
- Kahneman, D. (2011). Thinking, Fast and Slow — optimism bias, planning fallacy
- Tetlock, P. (2015). Superforecasting — calibrated skepticism
- Feynman, R. "Cargo Cult Science" — distinguishing real evidence from wishful thinking

# Failure handling
If the proposal is so vague that specific claims cannot be identified:
1. State that meaningful skeptic pass requires concrete claims to examine
2. Return the three questions that need answers before a skeptic pass can run
3. Identify which of the six lenses could be applied even with limited information
