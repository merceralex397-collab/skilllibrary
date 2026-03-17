---
name: skill-registry-manager
description: "Manages a skill registry: adding new skills, updating metadata, marking deprecations, querying by tag/category, and generating registry reports. Trigger when asked to 'update the skill registry', 'add X to the registry', 'list all skills in category Y', or 'mark skill X as deprecated'. Do NOT use for authoring new skill content — use skill-creator or skill-authoring instead."
source: created
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  category: package-scaffolding
  priority: P1
  maturity: draft
  risk: low
  tags: [registry, catalog, metadata, provenance, lifecycle]
---

# Purpose
Maintains the canonical registry of skills in a library: their slugs, categories, maturity states, source provenance, and tags. Acts as the single source of truth for "what skills exist, in what state, and where they came from." Supports skills-lock.json-style manifests and MANIFEST.md-style human-readable catalogs.

# When to use this skill
Use when:
- Adding a new skill entry to the registry after creation
- Updating a skill's maturity (draft → stable → deprecated)
- Querying the registry: "which skills are in category X?", "which skills are marked deprecated?"
- Generating a registry report or diff (what changed since last release)
- Reconciling the MANIFEST.md against the actual folder contents

Do NOT use when:
- Writing or rewriting SKILL.md content — use `skill-creator` or `skill-refinement`
- Evaluating skill quality — use `skill-evaluation`
- Installing skills into a client — use `skill-installation`

# Operating procedure
1. **Identify the registry file(s)**: Look for `MANIFEST.md`, `skills-lock.json`, or a `registry/` directory. Establish which is authoritative for the operation.
2. **For ADD**: Collect slug, category, priority (P0/P1/P2), source URL, license, source_tags (located/created), and initial maturity (`draft`). Append to manifest in alphabetical order within its category.
3. **For UPDATE**: Find the entry by slug. Update only the specified fields. Preserve all other fields. Record the change reason in a `changelog` field if the format supports it.
4. **For DEPRECATE**: Set `maturity: deprecated`, add `deprecated_by: REPLACEMENT_SLUG` and `deprecated_at: YYYY-MM-DD`. Do not delete the entry — deprecation is a state, not a removal.
5. **For QUERY**: Filter the registry by the requested dimension (category, maturity, tag, source). Return a structured list with slug, description, and status.
6. **For RECONCILE**: Diff the MANIFEST.md slugs against `ls COPILOT/` folder names. Report: entries in manifest but no folder, folders with no manifest entry, maturity mismatches.
7. **Write back**: Update the registry file(s). If both MANIFEST.md and skills-lock.json exist, update both consistently.

# Output defaults
- **ADD/UPDATE/DEPRECATE**: Confirmation of the change with the updated registry entry shown as a diff
- **QUERY**: Markdown table with columns: slug, category, maturity, source
- **RECONCILE**: Two-section report: `Missing folders` and `Unregistered folders`, plus a summary count

# References
- https://github.com/anthropics/skills (skills ecosystem structure)
- https://skills.sh/ (public registry format)
- https://docs.npmjs.com/cli/v10/configuring-npm/package-lock-json (lock file format inspiration)

# Failure handling
- **Registry file not found**: Check for MANIFEST.md, skills-lock.json, registry.yaml, or catalog.json. If none exist, create MANIFEST.md with the standard header and an empty skills table.
- **Slug collision**: Refuse to add a duplicate slug. Surface both entries for human decision: merge, rename, or deprecate one.
- **Inconsistency between MANIFEST.md and skills-lock.json**: Report the inconsistency explicitly. Do not auto-resolve — ask which is authoritative.
