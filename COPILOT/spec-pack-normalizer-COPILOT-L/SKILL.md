---
name: spec-pack-normalizer
description: "Normalizes scattered notes, chats, specs, and requirements into one canonical brief and decision packet. You repeatedly work from messy source material, so this skill is foundational. Trigger when the task context clearly involves spec pack normalizer."
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
  tags: [spec, normalization, brief]
---

# Purpose
Normalize scattered notes, chat logs, specs, and requirements into a single canonical brief and decision packet — the clean source of truth that all downstream agents work from.

# When to use this skill
Use when:
- Project requirements exist in multiple places (chat, Google Doc, Notion, README, email)
- Agents are making inconsistent decisions because there is no single source of truth
- The user pastes raw notes and says "figure out what we're building"
- A brief exists but contains contradictions or gaps that need resolution

Do NOT use when:
- A canonical brief already exists and is current
- The task is a single well-defined implementation request with no ambiguity

# Operating procedure
1. **Gather all input sources**: collect every spec fragment, chat excerpt, doc, or note
2. **Extract decisions** from each source: what is decided, what is deferred, what is contradicted
3. **Build the contradiction list**: for each contradiction, state both positions and the preferred resolution
4. **Write the canonical brief** at `docs/specs/BRIEF.md`:
   ```markdown
   ## What we are building
   <one paragraph — product/system name, primary goal, primary user>

   ## Constraints
   - <hard constraint 1>
   - <hard constraint 2>

   ## Out of scope
   - <explicit exclusion 1>

   ## Open decisions
   | Decision | Options | Owner | Due |
   |----------|---------|-------|-----|

   ## Resolved decisions
   | Decision | Resolution | Rationale |
   |----------|------------|-----------|

   ## Reading order
   <ordered list of source documents, most authoritative first>
   ```
5. **Mark sources as processed**: add a `[NORMALIZED → BRIEF.md YYYY-MM-DD]` tag to each source document
6. **Validate completeness**: the brief must answer: what, why, who, constraints, and out-of-scope — flag missing answers as open decisions

# Output defaults
`docs/specs/BRIEF.md` + list of open decisions requiring owner resolution.

# Failure handling
If two sources directly contradict each other on a hard constraint, do not resolve it — surface it as a `CONFLICT:` item in open decisions and block downstream work until it is resolved.
