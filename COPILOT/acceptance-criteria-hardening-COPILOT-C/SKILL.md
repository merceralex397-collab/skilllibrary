---
name: acceptance-criteria-hardening
description: "Turns vague success language into observable, testable, decision-complete acceptance criteria. Good acceptance criteria stabilize agents. Trigger when the task context clearly involves acceptance criteria hardening."
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
  tags: [acceptance-criteria, testing, clarity]
---

# Purpose
Converts vague, subjective, or incomplete acceptance criteria into observable, testable, decision-complete statements using BDD's Given/When/Then format. This skill applies Dan North's Behaviour-Driven Development insight: acceptance criteria are scenarios, and scenarios should be executable. An agent or tester should be able to verify each criterion without human interpretation.

# When to use this skill
Use when:
- The user says "harden these AC", "make these testable", "are these criteria good enough?", or "write Given/When/Then"
- A ticket has acceptance criteria containing words like "works correctly", "is fast", "looks good", "handles errors", or "is user-friendly"
- An agent is expected to self-evaluate completion and needs unambiguous criteria
- A QA or review cycle is about to start and criteria need to be clear before assessment
- Ticket refinement or backlog grooming sessions

Do NOT use when:
- No acceptance criteria exist yet (write them from scratch first)
- The criteria are already observable and testable—nothing to improve
- The scope of the ticket is unclear—fix scope first before hardening criteria
- The user wants to verify implementation against criteria (use `drift-detection`)

# Operating procedure
1. **Identify vague language patterns**: Scan each criterion for:
   - Subjective adjectives: "good", "clean", "fast", "user-friendly", "intuitive"
   - Undefined comparatives: "faster than before", "better than current"
   - Passive ambiguity: "errors are handled", "data is processed"
   - Scope gaps: what does "complete" mean? what counts as "success"?
   - Hidden actors: who does the action? what triggers the behavior?

2. **For each vague criterion, rewrite using Given/When/Then**:
   
   **Given** [a specific precondition or context]
   **When** [a specific action or event occurs]
   **Then** [a specific observable outcome]

   The Given establishes context. The When is the trigger. The Then is what a test would assert.

3. **Apply testability requirements**:
   - **Specific**: What exact behavior, output, or state must be observable?
   - **Measurable**: What is the threshold? (latency in ms, error rate in %, count of items, size in bytes)
   - **Actor-explicit**: Who or what performs the action? (user, system, scheduled job, API caller)
   - **Outcome-explicit**: What appears, what is stored, what is returned, what changes?
   - **Automatable**: Could a script verify this without human judgment?

4. **Add negative scenarios**: For each criterion, add what must NOT happen:
   - "Then the system does NOT display a loading spinner for more than 500ms"
   - "Then no error is logged"
   - "Then the previous data is NOT deleted"

5. **Add boundary conditions**: For each criterion, identify the most important edge case:
   - Empty input: What happens with zero items?
   - Maximum load: What happens at the upper limit?
   - Invalid data: What happens with malformed input?
   - Concurrent access: What happens with simultaneous requests?

6. **Check decision completeness**: Are there implicit decisions that must be made before this criterion can be evaluated?
   - "What counts as a 'valid' user?"
   - "What is the timeout threshold?"
   - "Which error cases return 4xx vs 5xx?"
   
   If the criterion requires a product decision not yet made, mark it **Needs Decision**.

# Output defaults
For each original criterion, output:

**Original**: [the vague criterion as written]

**Hardened**:
```gherkin
Given [precondition]
When [action]
Then [observable outcome]
And [additional assertions if needed]
```

**Edge cases**:
- [Boundary condition scenario]

**Negative case**:
- [What must NOT happen]

End with a **Decision Gaps** list for items that cannot be hardened without product input.

# References
- https://dannorth.net/blog/introducing-bdd/ — Dan North's original BDD article introducing Given/When/Then
- Cucumber/Gherkin syntax — standard format for executable specifications
- Specification by Example (Adzic, 2011) — comprehensive guide to testable requirements

# Failure handling
If the ticket scope is undefined:
1. State that criteria cannot be meaningfully hardened without clear scope
2. List the scope questions that must be answered first
3. Provide example hardened criteria conditional on scope answers

If criteria reference undefined terms, flag each term and request definitions.
