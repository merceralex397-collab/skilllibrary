---
name: skill-installation
description: "Installs or links a skill into a target client or local environment using the correct conventions. The skill-installer package in your sources makes this a clear candidate. Trigger when the task context clearly involves skill installation."
source: github.com/anthropics/skills
license: Apache-2.0
compatibility:
  clients: [openai-codex, gemini-cli, opencode, github-copilot]
metadata:
  owner: skill-library
  category: meta-skill-engineering
  priority: P0
  maturity: draft
  risk: low
  tags: [installation, linking, client]
---

# Purpose
Installs a skill package into local environment or client configuration. Handles extraction, integrity verification, client overlay application, and registration so skill is available for use.

# When to use this skill
Use when:
- User says "install this skill", "add skill from X", "set up skill"
- Installing from tarball, zip, or registry URL
- Installing from GitHub: `npx skills add owner/repo`
- Linking local folder for development

Do NOT use when:
- Creating/authoring skill (use `skill-authoring`)
- Packaging for distribution (use `skill-packaging`)
- Skill already installed and just needs configuration
- Uninstalling/removing skill

# Operating procedure
1. **Identify source**:
   - Local: `./skill-name-1.0.0.tar.gz` or `./skill-folder/`
   - Registry: `npx skills add skill-name`
   - GitHub: `npx skills add owner/repo`
2. **Download/extract**:
   - Tarball/zip: extract to temp
   - GitHub: clone or fetch
   - Local folder: verify SKILL.md exists
3. **Verify integrity**:
   - Check manifest.yaml exists
   - Verify SHA-256 checksum
   - If mismatch: abort
4. **Identify target client**:
   - Detect environment (Copilot, Claude, etc.)
   - Or use --client flag
   - Check compatibility list
5. **Apply client overlay** (if exists):
   - Find overlays/[client].yaml
   - Merge into base SKILL.md
6. **Install to correct location**:
   - Claude Desktop: `~/.claude/skills/[name]/`
   - VS Code Copilot: `.vscode/skills/[name]/`
   - Project-local: `./skills/[name]/`
7. **Register skill**:
   - Add to skills index/manifest
   - Update config files
8. **Verify installation**:
   - Folder exists with contents
   - Skill discoverable by client
9. **Report success**:
   - Show location
   - Show trigger instructions

# Output defaults
```
## Installation Complete

**Skill**: skill-name v1.0.0
**Location**: ~/.claude/skills/skill-name/
**Client**: Claude Desktop

### Verification
- [x] Checksum verified
- [x] Files extracted
- [x] Overlay applied
- [x] Registered

### Usage
Triggers on: [description]
Example: "help me with [purpose]"
```

# References
- https://skills.sh — Registry
- https://github.com/anthropics/skills
- Client skill documentation

# Failure handling
- **Checksum mismatch**: Don't install—"Integrity failed, may be corrupted"
- **Incompatible client**: Report supported clients, suggest alternative
- **Already installed**: Ask: overwrite, skip, or version?
- **Permission denied**: Suggest elevated permissions or alt location
- **Missing manifest**: Warn unverified source, proceed with caution
