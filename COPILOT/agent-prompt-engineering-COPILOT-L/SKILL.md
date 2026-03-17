---
name: agent-prompt-engineering
description: "Designs and hardens prompts for planners, implementers, reviewers, utilities, and workflow commands. Your reports repeatedly identified prompt hardening as necessary for weak-model reliability. Trigger when the task context clearly involves agent prompt engineering."
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
  tags: [prompts, agents, engineering]
---

# Purpose
Designs and hardens prompts for agent systems: preventing doom loops, avoiding status-over-evidence routing, handling impossible read-only delegation, and ensuring reliable execution across different model capabilities. Applies Anthropic's prompting best practices specifically for agentic contexts.

# When to use this skill
Use when:
- Creating new agent definitions (.opencode/agents/*.yaml)
- Debugging agents that behave inconsistently
- Hardening prompts after observing failure patterns
- Adapting prompts for different model capabilities

Do NOT use when:
- Writing one-off prompts for single tasks
- The agent is working reliably (don't fix what's not broken)
- Prompt issues are actually tool/infrastructure issues

# Operating procedure

## 1. Core prompting principles for agents

### Be explicit and direct
```yaml
# BAD: Vague
system_prompt: |
  You are a helpful coding assistant.

# GOOD: Explicit
system_prompt: |
  You are an implementer for project-name.
  Your job: Write code that passes tests and follows stack-standards.
  
  Before writing code:
  1. Read the ticket fully
  2. Check for existing patterns in src/
  3. Write tests first
  
  After writing code:
  1. Run tests: npm test
  2. Run linter: npm run lint
  3. Commit with ticket ID
```

### Tell what TO do, not what NOT to do
```yaml
# BAD: Negative instructions
system_prompt: |
  Do not write code without tests.
  Do not commit without running the linter.
  Do not skip the review step.

# GOOD: Positive instructions
system_prompt: |
  Always write tests alongside code.
  Always run the linter before committing.
  Always request review before marking done.
```

### Use structured output formats
```yaml
system_prompt: |
  When reporting findings, use this format:
  
  ## Finding: [title]
  **Severity:** [critical|major|minor]
  **Location:** [file:line]
  **Issue:** [description]
  **Evidence:** [proof]
  **Fix:** [suggestion]
```

## 2. Prevent common agent failure modes

### Failure: Doom loop
Agent repeats same action indefinitely.

**Cause:** No exit condition or success detection.

**Fix:**
```yaml
system_prompt: |
  Loop limit: Maximum 5 attempts per operation.
  
  After each attempt:
  - If success: proceed to next step
  - If failure: analyze error, try different approach
  - If 3 similar failures: STOP and report blocker
  
  Success indicators:
  - Tests pass
  - Linter passes
  - Expected output appears
```

### Failure: Status-over-evidence routing
Agent marks tasks done without verification.

**Cause:** Prompt allows status updates without proof.

**Fix:**
```yaml
system_prompt: |
  To mark a task done, you MUST provide evidence:
  
  ## Completion Evidence for [task]
  - [ ] Acceptance criterion 1: [evidence file:line or command output]
  - [ ] Acceptance criterion 2: [evidence]
  - [ ] Tests pass: [test output snippet]
  
  Do not update status without filling all evidence fields.
```

### Failure: Impossible read-only delegation
Delegating to read-only agent that needs to write.

**Cause:** Permission mismatch between task and agent.

**Fix:**
```yaml
# Define capabilities explicitly
agents:
  researcher:
    permissions: [read]
    capabilities: "Can read files, search code, fetch docs"
    cannot: "Cannot create files, run commands with side effects"
    
  implementer:
    permissions: [read, write, execute]
    capabilities: "Can create/edit files, run tests, make commits"
    
# In orchestrator prompt:
system_prompt: |
  Before delegating, verify agent capabilities:
  - Research tasks → researcher (read-only OK)
  - Implementation tasks → implementer (needs write)
  - Review tasks → reviewer (read-only OK)
```

### Failure: Context amnesia
Agent forgets earlier decisions/context.

**Cause:** Key information not reinforced.

**Fix:**
```yaml
system_prompt: |
  At the start of each task, recall:
  - Project: [load from BRIEF.md]
  - Stack: [load from STACK-PROFILE.md]
  - Current ticket: [from task assignment]
  
  Key constraints that always apply:
  - Use TypeScript strict mode
  - No any types
  - All functions must have tests
  
  Reference these constraints before making decisions.
```

## 3. Model-specific adjustments

### For capable models (Claude Sonnet 4+, GPT-4+)
- Can handle nuanced instructions
- Benefit from "think step by step"
- Can self-correct with guidance

```yaml
system_prompt: |
  Before implementing, think through:
  - What are the requirements?
  - What existing patterns should I follow?
  - What edge cases exist?
  
  Then implement and verify.
```

### For less capable models (Haiku, GPT-3.5)
- Need more explicit step-by-step
- Benefit from examples
- Need guardrails against hallucination

```yaml
system_prompt: |
  Follow this exact sequence:
  
  STEP 1: Read file
  Command: cat [filename]
  
  STEP 2: Identify change location
  Output: "Line [N]: [current content]"
  
  STEP 3: Make edit
  Change: [old] → [new]
  
  Do not skip steps. Do not combine steps.
```

## 4. Use XML structure for complex prompts
```yaml
system_prompt: |
  <role>
  You are an implementer for project-name.
  </role>
  
  <context>
  Stack: TypeScript, Node.js, Vitest
  Patterns: See src/patterns/
  </context>
  
  <instructions>
  1. Read the ticket
  2. Write tests first
  3. Implement to pass tests
  4. Run full test suite
  5. Commit with ticket ID
  </instructions>
  
  <constraints>
  - No any types
  - All exports must be typed
  - Test coverage > 80%
  </constraints>
  
  <output_format>
  After each commit, report:
  - Files changed: [list]
  - Tests: [pass/fail count]
  - Next step: [action]
  </output_format>
```

## 5. Include few-shot examples
```yaml
system_prompt: |
  <examples>
  
  <example>
  Task: Add validation to user input
  
  Step 1: Write test
  ```typescript
  test('rejects empty email', () => {
    expect(() => validateUser({ email: '' })).toThrow();
  });
  ```
  
  Step 2: Implement
  ```typescript
  function validateUser(input: UserInput): void {
    if (!input.email) throw new Error('Email required');
  }
  ```
  
  Step 3: Verify
  $ npm test -- validate
  ✓ rejects empty email
  </example>
  
  </examples>
```

## 6. Add self-verification
```yaml
system_prompt: |
  Before completing any task, self-verify:
  
  □ Did I follow the stack standards?
  □ Did I write tests for new code?
  □ Did I run the linter?
  □ Does the output match the expected format?
  
  If any check fails, fix before proceeding.
```

# Output defaults
Agent definitions with:
- Clear role statement
- Explicit capabilities and limitations
- Step-by-step operating procedure
- Output format specification
- Self-verification checklist

# References
- Anthropic prompting best practices: https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-prompting-best-practices
- XML tags for structure, examples for steering, explicit over implicit

# Failure handling
- **Agent still loops**: Add explicit loop counter and hard stop
- **Agent ignores instructions**: Move critical instructions to top of prompt
- **Model too weak for task**: Simplify task or upgrade model
- **Inconsistent behavior**: Add more examples of desired behavior
