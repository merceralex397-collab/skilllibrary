---
name: skill-packager
description: "Builds distributable bundles, manifests, and checksums for installing or sharing skills. Useful if you want a portable library instead of raw markdown folders. Trigger when the task context clearly involves skill packager."
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
  tags: [packaging, bundle, distribution]
---

# Purpose
Orchestrates full packaging workflow for one or more skills: validates, generates overlays, creates manifests, and produces release-ready bundle. This is the orchestration layer above `skill-packaging` for batch operations.

# When to use this skill
Use when:
- User says "package all skills", "build release bundle", "create distribution"
- Packaging multiple skills into single distribution
- Need overlays for multiple target clients
- CI/CD pipeline producing skill artifacts
- Releasing new library version

Do NOT use when:
- Packaging single skill (use `skill-packaging`)
- Installing packages (use `skill-installation`)
- Only need manifest without full bundle
- Skills not ready for packaging

# Operating procedure
1. **Scan for skills**:
   - Find all directories with SKILL.md
   - Filter by maturity if specified (e.g., only "stable")
   - Build process list
2. **Validate each skill**:
   - Parse frontmatter
   - Check required fields
   - Verify referenced files exist
   - Flag invalid skills (continue with valid)
3. **Determine target clients**:
   - From config or parameter
   - Default: all compatible per frontmatter
4. **Generate overlays** (per skill × client):
   - If compatible, create overlay
   - Store in skill/overlays/[client].yaml
5. **Create manifest per skill**:
   - Generate manifest.yaml with version, checksum
   - Use semver from tags or config
6. **Build individual bundles**:
   - Call `skill-packaging` for each
   - Produce: skill-name-version.tar.gz
7. **Create combined bundle** (optional):
   - Combine into skills-bundle-version.tar.gz
   - Include index.yaml with all metadata
8. **Generate release notes**:
   - List all skills
   - Note version changes
   - List added/removed/deprecated

# Output defaults
```
dist/
├── index.yaml
├── skills-bundle-1.0.0.tar.gz
├── skill-a-1.0.0.tar.gz
├── skill-b-1.2.0.tar.gz
└── RELEASE-NOTES.md
```

index.yaml:
```yaml
version: 1.0.0
generated: 2024-01-15T10:00:00Z
skills:
  - name: skill-a
    version: 1.0.0
    checksum: abc...
    file: skill-a-1.0.0.tar.gz
```

# References
- https://github.com/anthropics/skills
- Semver: https://semver.org/

# Failure handling
- **Some skills invalid**: Package valid ones, report invalid with errors
- **Version conflict**: Use highest or flag for resolution
- **Missing git tags**: Fall back to 0.0.0-dev or require explicit
- **Overlay generation fails**: Package base without overlay, note limitation
