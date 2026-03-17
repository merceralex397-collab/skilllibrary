---
name: skill-packaging
description: "Packages a skill into installable outputs and validates bundle contents. Needed if you want portable artifacts rather than a loose folder. Trigger when the task context clearly involves skill packaging."
source: created
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  category: meta-skill-engineering
  priority: P1
  maturity: draft
  risk: low
  tags: [packaging, bundle, distribution]
---

# Purpose
Packages a skill folder into a distributable format (zip/tarball) with manifest, integrity checksums, and optional client-specific overlays. A packaged skill can be shared, versioned, and installed via registry tooling.

# When to use this skill
Use when:
- User says "package this skill", "create bundle", "prepare for distribution"
- Skill ready for sharing outside source repository
- Publishing to registry or marketplace
- Creating versioned release

Do NOT use when:
- Skill still in development (finish authoring first)
- Just need to copy folder locally (no packaging needed)
- Orchestrating multiple skills (use `skill-packager`)
- Installing package (use `skill-installation`)

# Operating procedure
1. **Validate skill is packageable**:
   - SKILL.md exists with valid frontmatter (name, description, license)
   - Required fields present
   - No broken references to missing files
   - All scripts/ and references/ files exist
2. **Create manifest file** (`manifest.yaml`):
   ```yaml
   name: skill-name
   version: 1.0.0
   description: "[from frontmatter]"
   license: Apache-2.0
   author: [author]
   repository: [source URL]
   files:
     - SKILL.md
     - scripts/*
     - references/*
     - evals/*
   compatibility:
     clients: [list]
   checksum: [sha256]
   ```
3. **Generate client overlays** (if needed):
   - Create overlays/ directory
   - Each: overlays/[client].yaml with modifications
4. **Calculate integrity checksum**:
   - SHA-256 of all files concatenated
   - Store in manifest
5. **Create bundle**:
   - `tar -czvf skill-name-1.0.0.tar.gz skill-folder/`
   - Or: `zip -r skill-name-1.0.0.zip skill-folder/`
6. **Validate bundle**:
   - Extract to temp location
   - Verify files present
   - Verify checksum matches
   - Verify SKILL.md parses
7. **Document installation**: Add instructions to manifest

# Output defaults
```
skill-name-1.0.0.tar.gz
├── manifest.yaml
├── SKILL.md
├── scripts/
├── references/
├── evals/
└── overlays/

## Verification Report
- [x] SKILL.md valid
- [x] Manifest complete
- [x] All files present
- [x] Checksum: abc123...
- [x] Extraction passed

Install: npx skills add ./skill-name-1.0.0.tar.gz
```

# References
- https://github.com/anthropics/skills
- https://agentskills.io

# Failure handling
- **Missing required files**: List what's missing; cannot package
- **Invalid frontmatter**: Show errors; fix before packaging
- **Checksum mismatch**: Re-generate, re-create bundle
- **Overlay conflicts with base**: Overlays modify only, not contradict
