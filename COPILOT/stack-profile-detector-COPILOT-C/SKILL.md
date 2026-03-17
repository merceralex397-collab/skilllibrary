---
name: stack-profile-detector
description: "Infers language, framework, cloud target, and runtime profile from repo evidence and spec material. This lets generation choose the right packs instead of a generic placeholder. Trigger when the task context clearly involves stack profile detector."
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
  tags: [stack, detection, profile]
---

# Purpose
Detects a project's stack profile from repository contents—package.json, Cargo.toml, requirements.txt, go.mod, Dockerfile, configuration files—and produces a structured stack profile document. Enables downstream skills to make stack-appropriate decisions without guessing.

# When to use this skill
Use when:
- Scaffolding a new project and need to determine stack from specs
- Onboarding to an existing repo with unknown stack
- Validating that detected stack matches actual conventions
- Generating stack-specific skills (stack-standards, testing-patterns)

Do NOT use when:
- Stack is explicitly documented in STACK-PROFILE.md
- Working on a known project with established conventions
- The project has no code yet (detect from specs instead)

# Operating procedure

## 1. Scan for manifest files
```bash
# Check for primary manifests
ls -la package.json Cargo.toml pyproject.toml setup.py go.mod pom.xml \
       build.gradle *.csproj Gemfile composer.json 2>/dev/null
```

Manifest priority (if multiple exist):
1. package.json (Node.js projects)
2. Cargo.toml (Rust projects)
3. pyproject.toml or setup.py (Python projects)
4. go.mod (Go projects)
5. pom.xml or build.gradle (Java projects)

## 2. Detect primary language

| Evidence | Language | Confidence |
|----------|----------|------------|
| package.json | JavaScript/TypeScript | High |
| tsconfig.json | TypeScript | High |
| Cargo.toml | Rust | High |
| pyproject.toml, setup.py, requirements.txt | Python | High |
| go.mod, go.sum | Go | High |
| pom.xml, build.gradle | Java/Kotlin | High |
| *.csproj, *.sln | C# | High |
| Gemfile | Ruby | High |
| *.swift, Package.swift | Swift | High |
| Only .js files | JavaScript | Medium |
| Only .py files | Python | Medium |

```bash
# TypeScript detection
if [ -f "tsconfig.json" ]; then
  language="TypeScript"
elif [ -f "package.json" ] && grep -q "typescript" package.json; then
  language="TypeScript"
elif [ -f "package.json" ]; then
  language="JavaScript"
fi
```

## 3. Detect framework

### Node.js/TypeScript frameworks
```bash
# Check package.json dependencies
deps=$(cat package.json | jq -r '.dependencies + .devDependencies | keys[]' 2>/dev/null)

case "$deps" in
  *next*) framework="Next.js" ;;
  *react*) framework="React" ;;
  *vue*) framework="Vue" ;;
  *express*) framework="Express" ;;
  *fastify*) framework="Fastify" ;;
  *nestjs*) framework="NestJS" ;;
  *) framework="Node.js (vanilla)" ;;
esac
```

### Python frameworks
```bash
# Check requirements.txt or pyproject.toml
if grep -q "fastapi" requirements.txt pyproject.toml 2>/dev/null; then
  framework="FastAPI"
elif grep -q "django" requirements.txt pyproject.toml 2>/dev/null; then
  framework="Django"
elif grep -q "flask" requirements.txt pyproject.toml 2>/dev/null; then
  framework="Flask"
fi
```

### Rust frameworks
```bash
# Check Cargo.toml dependencies
if grep -q "axum" Cargo.toml; then
  framework="Axum"
elif grep -q "actix" Cargo.toml; then
  framework="Actix"
elif grep -q "rocket" Cargo.toml; then
  framework="Rocket"
fi
```

## 4. Detect runtime/environment
```bash
# Node.js version
if [ -f ".nvmrc" ]; then
  node_version=$(cat .nvmrc)
elif [ -f "package.json" ]; then
  node_version=$(jq -r '.engines.node // "not specified"' package.json)
fi

# Python version
if [ -f ".python-version" ]; then
  python_version=$(cat .python-version)
elif [ -f "pyproject.toml" ]; then
  python_version=$(grep "python" pyproject.toml | head -1)
fi

# Docker
if [ -f "Dockerfile" ]; then
  base_image=$(grep "^FROM" Dockerfile | head -1 | cut -d' ' -f2)
fi
```

## 5. Detect testing setup
```bash
# Node.js
if grep -q "vitest" package.json; then
  test_framework="Vitest"
elif grep -q "jest" package.json; then
  test_framework="Jest"
elif grep -q "mocha" package.json; then
  test_framework="Mocha"
fi

# Python
if [ -f "pytest.ini" ] || grep -q "pytest" pyproject.toml; then
  test_framework="pytest"
fi

# Rust
test_framework="cargo test"  # Built-in

# Go
test_framework="go test"  # Built-in
```

## 6. Detect cloud/deployment target
```bash
# AWS
if [ -d ".aws" ] || [ -f "serverless.yml" ] || [ -f "samconfig.toml" ]; then
  cloud="AWS"
fi

# Google Cloud
if [ -f "app.yaml" ] || [ -d ".gcloud" ]; then
  cloud="GCP"
fi

# Vercel
if [ -f "vercel.json" ]; then
  cloud="Vercel"
fi

# Docker/Kubernetes
if [ -f "docker-compose.yml" ] || [ -d "k8s" ]; then
  deployment="Containerized"
fi
```

## 7. Generate STACK-PROFILE.md
```markdown
# Stack Profile
Generated: [ISO date]
Confidence: [HIGH|MEDIUM|LOW]

## Primary Stack
- **Language:** TypeScript
- **Runtime:** Node.js 20.x
- **Framework:** Next.js 14
- **Package Manager:** pnpm

## Testing
- **Framework:** Vitest
- **Command:** `pnpm test`
- **Location:** `**/*.test.ts`

## Build
- **Tool:** Next.js built-in
- **Command:** `pnpm build`
- **Output:** `.next/`

## Linting/Formatting
- **Linter:** ESLint
- **Formatter:** Prettier
- **Command:** `pnpm lint`

## Deployment
- **Target:** Vercel
- **Config:** `vercel.json`

## Database
- **Type:** PostgreSQL
- **ORM:** Prisma
- **Migrations:** `prisma migrate`

## Evidence
Detection based on:
- package.json: dependencies, scripts
- tsconfig.json: TypeScript configuration
- vercel.json: Deployment configuration
- prisma/schema.prisma: Database schema
```

## 8. Handle ambiguous cases
If detection is uncertain:
```markdown
## Stack Profile (DRAFT)
Confidence: LOW

## Detected
- Language: JavaScript or TypeScript (no tsconfig.json but .ts files exist)

## Undetected
- Framework: Multiple possibilities (has both express and fastify)
- Database: No ORM detected

## Action Required
Please confirm:
1. Is this TypeScript? [Add tsconfig.json if yes]
2. Primary framework? [Express / Fastify / Other]
3. Database? [PostgreSQL / MySQL / MongoDB / None]
```

# Output defaults
File: `docs/STACK-PROFILE.md`

Contains:
- Primary language and runtime
- Framework
- Package manager and commands
- Testing setup
- Build configuration
- Deployment target
- Database (if applicable)
- Confidence level and evidence

# References
- Package managers: npm, pnpm, yarn, pip, cargo, go
- Manifest files are authoritative sources

# Failure handling
- **No manifest files**: Check for code files (.ts, .py, .rs), infer language, mark confidence LOW
- **Multiple conflicting manifests**: Likely monorepo—detect for each package
- **Empty repository**: Cannot detect, require specs from user
- **Unusual setup**: Document what was found, flag for manual review
