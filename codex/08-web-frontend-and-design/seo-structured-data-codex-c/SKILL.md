---
name: seo-structured-data-codex-c
description: Adds search-oriented metadata, sitemap, schema, and crawl-aware content structure where relevant. Use this when the user or repo work clearly points at seo and structured data or a task in the "Web, Frontend, and Design Skills" family needs repeatable procedure rather than ad hoc prompting. Do not use for backend-only or infrastructure-only work.
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: codex
  domain: seo-structured-data
  maturity: draft
  risk: low
  tags: [seo, structured, data]
---

# Purpose

Adds search-oriented metadata, sitemap, schema, and crawl-aware content structure where relevant.

# When to use this skill

Use this skill when:

- the user or repo work clearly points at seo and structured data
- a task in the "Web, Frontend, and Design Skills" family needs repeatable procedure rather than ad hoc prompting
- a plan, ticket, or repo state would benefit from explicit guardrails around seo and structured data

# Do not use this skill when

- the task is really about backend-only or infrastructure-only work
- If the task is more specifically about `brand-guidelines` or `webapp-testing`, prefer that skill instead.
- the relevant files, runtime, or deliverable type are already covered by a more specific active skill

# Operating procedure

1. Detect the actual UI stack, rendering constraints, and component boundaries involved in SEO and Structured Data.
2. Choose patterns that preserve component boundaries and state ownership without over-abstracting early.
3. Review interaction states, responsive behavior, and accessibility before polishing visuals.
4. Keep data flow and styling decisions explicit so later changes stay reviewable.
5. Verify the result on realistic states, not just the happy path screenshot.

# Decision rules

- Prefer clear component ownership over hidden coupling around component boundaries.
- Accessibility failures outrank cosmetic polish.
- Avoid abstractions that hide interactive state or state ownership.
- Review empty, loading, error, and dense-data states before calling the work done.

# Output requirements

1. `Detected Stack Signals`
2. `Chosen Pattern`
3. `Implementation Notes`
4. `Validation`

# References

Read these only when relevant:

- `references/implementation-patterns.md`
- `references/review-checklist.md`
- `references/common-pitfalls.md`

# Related skills

- `brand-guidelines`
- `webapp-testing`
- `analytics-instrumentation`
- `react-native-firebase`

# Failure handling

- If the scope is ambiguous, restate the decision boundary before proceeding.
- If the evidence is weak, say so explicitly and lower confidence instead of smoothing it over.
- If the task would be better served by a narrower skill, redirect to it rather than stretching this one.
