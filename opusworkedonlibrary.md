# Opus Skill Library Improvement Log

## Overview

This document records the full process of building the unified skill library: merging 315 skills from the COPILOT library (Opus/Sonnet rewrites) with ~350 skills from the Codex library, running skill-improver passes across all categories, performing Scafforge workflow comparison for Category 01, and rewriting boilerplate stubs with domain-specific technical content. The result is a 350-skill library with 0 boilerplate stubs, specific routing descriptions, and concrete operating procedures.

## Process

### Phase 1 — Merge
Two source libraries were combined:
- **COPILOT library** (315 skills): Opus/Sonnet-authored skills with strong procedures but sometimes generic descriptions
- **Codex library** (~350 skills): Template-generated skills covering all 17 categories, many with boilerplate procedures

Commit `b7df75c` added the 315-skill COPILOT library. Commit `8ff214e` brought in Codex skill changes. Overlapping skills were deduplicated; the version with more substantive content was kept, with routing descriptions fixed regardless of source.

### Phase 2 — Boilerplate Rewrite
~120 skills had prompt-blob syndrome: identical 5-step generic procedures copied across all skills in a category with zero domain content. These were rewritten in batches:
- Commit `7857c58`: Rewrite 40 SKILL.md files with substantive procedures
- Commit `0b1796f`: Replace generic descriptions with specific triggers and named alternatives
- Commit `8fa69f7`: Rewrite 5 AI/ML skill files with domain-specific technical content
- Commit `39cefef`: Rewrite flask, express-node, go-api-service, python with real technology content
- Commit `e733d41`: Rewrite 40 boilerplate skills with domain-specific content (Batch C)
- Commit `6d61778`: Rewrite 41 boilerplate SKILL.md files with domain-specific content (Batch B)

### Phase 3 — Scafforge Comparison (Category 01)
The 10 Scafforge skills in Category 01 were compared against their library counterparts. Key workflow terms, step sequences, gates, decision trees, and mode systems were preserved while broadening beyond OpenCode-specific paths. See Category 01 below for details.

### Phase 4 — Skill-Improver Pass
Each skill was evaluated against the skill-improver checklist:
1. Description must be routing logic (trigger phrases + DO NOT USE boundaries), not marketing
2. Procedure must contain the actual technique, not a description of the technique
3. Decision rules must be domain-specific, not generic
4. Output contract must be explicit
5. Failure modes must be named and specific
6. References must point to real external docs

### Methodology: Official Docs References
Where applicable, official documentation was integrated as authoritative references rather than paraphrased guidance:
- WCAG 2.2 criterion numbers for accessibility
- Core Web Vitals thresholds for frontend performance
- RFC 7282 for consensus-building patterns
- OWASP ASVS and MITRE ATT&CK for security skills
- Google SRE Postmortem Culture for incident analysis
- W3C DTCG format for design tokens
- LoRA/QLoRA/DPO paper citations for ML training skills

---

## Changes by Category

### Category 01 — Package Scaffolding (20 skills)

10 Scafforge skills merged with library versions preserving workflow logic; 10 library-only skills had boilerplate descriptions replaced.

- **scaffold-kickoff** — Preserved complete Scafforge 10-step workflow with decision tree (greenfield/retrofit/refinement), gates at every step, done checklist
- **agent-prompt-engineering** — Merged Scafforge 7-step procedure with library examples; added role-based prompt contract table (orchestrator/planner/implementer/reviewer/utility)
- **spec-pack-normalizer** — Adopted Scafforge 6-step procedure with 12 required sections schema, decision packet concept, validation step
- **repo-scaffold-factory** — Adopted Scafforge two-phase model (script-assisted + agent-driven) with "files NOT to customize" guardrails
- **opencode-team-bootstrap** — Adopted Scafforge team composition tables with baseline agents, project-specific agents by type, design principles
- **ticket-pack-builder** — Adopted Scafforge wave system (0-3), parallel lane rules with overlap_risk, manifest.json as source of truth
- **project-skill-bootstrap** — Adopted Scafforge dual modes (foundation/synthesis) with quality rules (≤12-15 skills, repo-specific not generic)
- **handoff-brief** — Merged Scafforge systematic procedure (gather → write → validate) with library templates; added validation checklist
- **pr-review-ticket-bridge** — Added "perform professional review first" step before comment triage; canonical ticket proposal concept
- **repo-process-doctor** — Added three explicit modes (audit/propose-repair/apply-repair); safe vs intent-changing repair boundary table
- **community-skill-harvester** — Replaced boilerplate description with specific triggers (find external skills, evaluate quality, migrate)
- **migration-pack-builder** — Replaced boilerplate description with specific triggers (major dependency upgrades, framework migrations)
- **overlay-generator** — Replaced boilerplate description; tightened procedure; noted overlay format evolution
- **provenance-audit** — Replaced boilerplate description; preserved trust level decision matrix
- **skill-deprecation-manager** — Replaced boilerplate description; procedure was already well-structured
- **skill-description-optimizer** — Replaced boilerplate description using skill's own methodology; added anti-pattern list
- **skill-eval-runner** — Replaced boilerplate description; added verdict criteria table
- **skill-packager** — Replaced boilerplate description; removed dangling reference to non-existent sub-skill
- **skill-registry-manager** — Complete rewrite — was the worst skill in category (5 hollow generic steps with zero registry content); now has 6-step registry management procedure with consistency checks and index generation
- **stack-profile-detector** — Replaced boilerplate description; procedure content was already the strongest in the category

### Category 02 — Generated Repo Core (32 skills)

These skills form the core project-context layer. Improvements focused on making descriptions routing-precise and ensuring procedures contain actionable steps rather than generic advice.

- **api-schema** — Added OpenAPI/JSON Schema validation commands and contract-first workflow steps
- **auth-patterns** — Added JWT/session/OAuth2 decision tree with specific library references
- **context-intelligence** — Sharpened description to target codebase understanding tasks specifically
- **database-persistence** — Added migration workflow, ORM pattern guidance, connection pooling specifics
- **dependency-upgrades** — Added semver range strategy, breaking change detection, automated upgrade commands
- **deployment-pipeline** — Added CI/CD stage definitions with concrete GitHub Actions/GitLab CI examples
- **docs-and-handoff** — Added START-HERE.md template, handoff checklist, session resume context
- **error-handling** — Added error taxonomy (operational vs programmer), retry patterns, structured error types
- **external-api-client** — Added retry/backoff patterns, circuit breaker, timeout configuration
- **incident-postmortem** — Added blameless postmortem template, timeline reconstruction, action item tracking
- **isolation-guidance** — Added dependency injection patterns, test double strategies, module boundary enforcement
- **local-git-specialist** — Added rebase workflow, conflict resolution procedures, commit message conventions
- **mcp-protocol** — Added MCP-specific transport selection, tool design patterns, resource/prompt primitives
- **migration-refactor** — Added strangler fig pattern, feature flag migration, incremental rollout steps
- **node-agent-patterns** — Added agent loop patterns, tool registration, conversation state management
- **performance-baseline** — Added benchmark methodology, flamegraph interpretation, regression detection thresholds
- **planning** — Added plan template with dependency edges, acceptance criteria, file touch lists
- **process-doctor** — Added smell detection patterns, safe vs intent-changing repair boundary
- **project-context** — Added repo inventory procedure, stack detection, convention extraction
- **prompt-crafting** — Added system prompt structure, few-shot patterns, chain-of-thought templates
- **release-engineering** — Added semantic versioning workflow, changelog generation, tag/release automation
- **repo-navigation** — Added codebase exploration patterns, file tree reading, symbol search strategies
- **research-delegation** — Added evidence quality tiers, source triangulation, synthesis templates
- **review-audit-bridge** — Added findings-first review structure, severity grading, follow-up ticket generation
- **security-best-practices** — Added OWASP Top 10 mapping, input validation patterns, secrets management
- **security-hardening** — Added attack surface reduction checklist, header configuration, dependency scanning
- **security-ownership-map** — Added CODEOWNERS generation, security-sensitive path identification
- **security-threat-model** — Added STRIDE methodology, threat matrix template, risk scoring
- **stack-standards** — Added language/framework convention extraction, linter configuration, style enforcement
- **testing** — Added test pyramid strategy, coverage targets, test naming conventions
- **ticket-execution** — Added ticket-to-implementation workflow, acceptance criteria verification, PR template
- **workflow-observability** — Added structured logging patterns, metric collection, trace correlation

### Category 03 — Meta-Skill Engineering (18 skills)

Skills about building, testing, and maintaining other skills. Already had good procedural content from Codex authoring; improvements focused on routing descriptions and cross-references.

- **skill-adaptation** — Added specific triggers for adapting skills across contexts; explicit boundary with skill-refinement
- **skill-anti-patterns** — Added anti-pattern checklist (prompt-blob, kitchen-sink trigger, phantom refs, circular content); pre-promotion review use case
- **skill-authoring** — Added YAML frontmatter template, description-as-routing-logic guidance, section structure requirements
- **skill-benchmarking** — Added eval methodology: prompt-response pairs, scoring rubric, baseline comparison
- **skill-catalog-curation** — Added catalog consistency checks, naming convention enforcement, orphan detection
- **skill-creator** — Already comprehensive (427 lines); minor routing description tightening
- **skill-evaluation** — Added structured evaluation criteria: routing accuracy, procedure completeness, output quality
- **skill-installation** — Consolidated with skill-installer (commit `92bb75a`); deduplicated
- **skill-installer** — Merged content from skill-installation; canonical install procedure
- **skill-lifecycle-management** — Added lifecycle stages (draft → stable → deprecated → archived) with promotion criteria
- **skill-packaging** — Added distribution bundle structure, manifest generation, validation steps
- **skill-provenance** — Added trust level assessment, license compliance check, source verification
- **skill-reference-extraction** — Added pattern for extracting reusable references from official docs into skill bodies
- **skill-refinement** — Added incremental improvement methodology; explicit boundary with skill-authoring (new) and skill-trigger-optimization (routing only)
- **skill-safety-review** — Added safety checklist for skills that modify files, run commands, or access external services
- **skill-testing-harness** — Added test case structure, eval prompt design, pass/fail criteria
- **skill-trigger-optimization** — Added diagnostic methodology: undertriggering/overtriggering detection, discriminating signal extraction, test prompt validation
- **skill-variant-splitting** — Added criteria for when one skill should become two; merge/split decision rules

### Category 04 — Planning, Review, and Critique (18 skills)

3 major rewrites (generic stubs → full method skills), 10 moderate improvements (named failure modes added), 5 minor polishes.

- **red-team-challenge** — REWRITE: Full adversarial attack method with 8 attack vector categories, exploitation walkthrough, OWASP ASVS + MITRE ATT&CK references
- **skeptic-pass** — REWRITE: Full evidence-quality review with 5-tier grading (Proven/Plausible/Asserted/Contradicted/Unfalsifiable), 7 optimism bias patterns
- **steelman** — REWRITE: Full charitable reconstruction with 7-step procedure, verdict scale (Strong/Conditional/Weak), Rapoport's Rules reference
- **architecture-review** — Added 5 named failure modes (diagram worship, level confusion, coupling blindness, greenfield bias, armchair architecture); arc42 reference
- **blocker-extraction** — Added 5 named failure modes (false blocker inflation, chronic acceptance, missing the real blocker, workaround blindness, owner-free blockers)
- **contradiction-finder** — Added 5 named failure modes (false contradiction from ambiguity, missing temporal context, shallow extraction, over-extraction, single-source blindness)
- **decision-packet-builder** — Added 5 named failure modes (decision inflation, option theater, missing meta-decision, recommendation hedging, stale packets); RFC 7282
- **drift-detection** — Added 5 named failure modes (wrong canonical reference, completeness illusion, false drift from ambiguity, missing undocumented additions, snapshot bias)
- **failure-mode-analysis** — Added 5 named failure modes (component granularity mismatch, happy-path FMEA, detection optimism, missing cascade analysis, static analysis); SRE reference
- **gap-analysis** — Added 5 named failure modes (vague target state, inventory-only analysis, effort sandbagging, missing dependency gaps, surplus blindness); NIST SSDF
- **root-cause-analysis** — Added 5 named failure modes (stopping at proximate cause, blame-as-root-cause, multiple root causes dodge, solution-first RCA, missing process failure); SRE reference
- **scope-pressure-test** — Added 5 named failure modes (estimation without constraints, hidden work blindness, uniform sizing, sacred cow protection, phase 2 graveyard)
- **reverse-brainstorming** — Added 5 named failure modes (vague sabotage, skipping inversion, groupthink in reverse, protection overload, missing the boring sabotage)
- **acceptance-criteria-hardening** — Added 5 named failure modes; BDD reference (Dan North)
- **assumptions-audit** — Added 4 named failure modes (assumption inflation, taxonomy without ranking, conversion failure, assumption-decision confusion)
- **plan-review** — Added 5 named failure modes (approval without teeth, critique without repair, sequencing blindness, scope confusion, risk theater)
- **premortem** — Added 5 named failure modes (cinematic failure bias, risk listing without controls, unfocused horizon, existing control blindness, equal-weight risks); SRE reference
- **tradeoff-analysis** — Added 5 named failure modes (criteria rigging, score washing, omitting regret, false precision, missing reversal triggers); RFC 7282

### Category 05 — Agentic Orchestration and Autonomy (20 skills)

Focused on multi-agent coordination, delegation, and autonomous workflow patterns. Improvements added specific trigger/skip conditions and concrete coordination protocols.

- **agent-orchestration** — Added bounded delegation contracts, artifact handoff points, critical path retention
- **approval-gates** — Added gate types (human, automated, hybrid), escalation procedures, timeout handling
- **artifact-contracts** — Added contract schema definitions, validation rules, versioning
- **autonomous-backlog-maintenance** — Added backlog grooming triggers, priority recalculation, stale ticket detection
- **autonomous-run-control** — Added run budget limits, checkpoint criteria, abort conditions
- **collaboration-checkpoints** — Added checkpoint types, sync protocols, conflict detection
- **delegation-boundaries** — Added delegation decision tree, capability matching, scope containment
- **goal-decomposition** — Added decomposition heuristics, dependency graph generation, parallel lane identification
- **human-interrupt-handling** — Added interrupt classification, context preservation, resume protocols
- **long-run-watchdog** — Added health check patterns, progress monitoring, deadlock detection
- **manager-hierarchy-design** — Added hierarchy depth limits, span of control guidance, escalation paths
- **multi-agent-debugging** — Added trace correlation, message replay, state inspection patterns
- **panel-of-experts** — Added expert selection criteria, opinion synthesis, conflict resolution
- **parallel-lane-safety** — Added overlap risk detection, file-level conflict prevention, merge strategies
- **process-versioning** — Added version migration paths, backward compatibility rules, rollback procedures
- **session-resume-rehydration** — Added context reconstruction from artifacts, state recovery, continuation protocols
- **subagent-research-patterns** — Added research delegation templates, evidence quality requirements, synthesis rules
- **swarm-patterns** — Added map/reduce, scatter-gather, and blackboard architectures with convergence protocols
- **verification-before-advance** — Added gate check templates, evidence requirements, pass/fail criteria
- **workflow-state-memory** — Added state persistence patterns, checkpoint/restore, session state schemas

### Category 06 — Agent Role Candidates (16 skills)

Role-specific agent templates. Improvements added clear trigger/skip conditions, specific output formats, and cross-references to related roles.

- **backlog-verifier** — Added verification checklist, staleness detection, priority validation
- **code-review** — Added findings-first review structure, severity ordering, residual risk reporting
- **context-summarization** — Added summary templates, information density targets, context window awareness
- **docs-handoff** — Added handoff document template, recipient-specific formatting
- **github-prior-art-research** — Added search strategy patterns, relevance scoring, synthesis templates
- **implementer-context** — Added context gathering checklist, file touch list generation
- **implementer-hub** — Added work coordination patterns, status tracking, completion criteria
- **implementer-node-agent** — Added Node.js-specific implementation patterns, test-first workflow
- **planner** — Added plan template with dependency edges, file touch lists, acceptance criteria, ordered implementation steps
- **qa-validation** — Added test matrix generation, edge case identification, regression detection
- **repo-evidence-gathering** — Added evidence search patterns, citation formatting, confidence levels
- **security-review** — Added vulnerability checklist, threat model mapping, remediation priorities
- **shell-inspection** — Added command output analysis, environment diagnosis, log interpretation
- **team-leader** — Added work breakdown structure, specialist assignment, progress tracking, blocker escalation
- **ticket-audit** — Added audit checklist, completeness scoring, missing-field detection
- **ticket-creator** — Added ticket template with acceptance criteria, labels, priority, assignee

### Category 07 — MCP (20 skills)

Model Context Protocol skills covering server development, tool design, security, and integration. Mix of Anthropic-sourced skills and new Codex skills.

- **get-started** — MCP quickstart with Python FastMCP and TypeScript SDK setup
- **mcp-auth-transports** — Added OAuth 2.1 flow, SSE/stdio/streamable-http transport selection, token management
- **mcp-builder** — Anthropic-sourced comprehensive MCP server development guide (4 phases)
- **mcp-chatgpt-app-bridge** — Added ChatGPT Actions integration with MCP servers
- **mcp-development** — Added server lifecycle, handler registration, error handling patterns
- **mcp-go-server** — Added Go-specific MCP server implementation patterns
- **mcp-host-integration** — Added host-side tool invocation, permission models, multi-server routing
- **mcp-inspector-debugging** — Added MCP Inspector usage, protocol-level debugging, message tracing
- **mcp-marketplace-publishing** — Added packaging requirements, metadata schemas, submission workflow
- **mcp-migration-retrofit** — Added patterns for adding MCP to existing APIs and services
- **mcp-multi-tenant-design** — Added tenant isolation, per-tenant tool scoping, credential management
- **mcp-python-fastmcp** — Added FastMCP decorators, resource patterns, lifespan management
- **mcp-resources-prompts** — Added resource URI schemes, prompt templates, sampling patterns
- **mcp-schema-contracts** — Added JSON Schema for tool inputs, validation patterns, versioning
- **mcp-security-permissions** — Added least-privilege tool gating, input validation, injection defense, fail-closed behavior
- **mcp-testing-evals** — Added MCP server testing patterns, mock transports, eval frameworks
- **mcp-tool-design** — Added naming conventions, description writing for LLMs, annotation patterns (readOnlyHint, destructiveHint)
- **mcp-typescript-sdk** — Added TypeScript SDK patterns, type-safe tool definitions, server/client setup
- **opencode-bridge** — Added OpenCode-specific MCP integration patterns
- **opencode-primitives** — Added OpenCode primitive tool patterns and composition

### Category 08 — Web, Frontend, and Design (26 skills)

16 template stubs rewritten with real framework code; 10 already-developed skills received frontmatter standardization and targeted additions.

- **react-typescript** — Added discriminated union props, generic components, forwardRef typing, useReducer patterns (72→140 lines)
- **nextjs-app-router** — Added route conventions, Server/Client component decision guide, Server Actions, caching patterns (73→157 lines)
- **tailwind-shadcn** — Added shadcn CLI commands, cn() utility, cva variant pattern, CSS custom property theming (74→184 lines)
- **vue** — Added script setup, defineProps/defineEmits, composable pattern, Pinia store, typed routes (74→162 lines)
- **svelte** — Added Svelte 5 runes ($state, $derived, $effect, $props), SvelteKit routes, form actions (74→184 lines)
- **testing-web** — Added Testing Library query priority, component test pattern with Vitest, MSW mocking (74→173 lines)
- **forms-validation** — Added React Hook Form + Zod integration, accessible form patterns, cross-field validation (74→157 lines)
- **state-management** — Added state categorization table, useReducer with discriminated actions, Zustand, TanStack Query (74→157 lines)
- **frontend-performance** — Added Core Web Vitals targets, bundle optimization, rendering optimization, image optimization (74→139 lines)
- **accessibility-audit** — Added WCAG 2.2 criteria by level (A/AA/AAA), axe-core CLI, WAI-ARIA APG patterns (74→146 lines)
- **analytics-instrumentation** — Added event naming conventions, GA4 implementation, type-safe trackEvent wrapper (73→196 lines)
- **design-tokens** — Added 3-layer architecture (primitive→semantic→component), W3C DTCG format, Style Dictionary (74→198 lines)
- **seo-structured-data** — Added meta tags template, Next.js metadata API, JSON-LD examples, sitemap generation (74→217 lines)
- **ux-design** — Added visual hierarchy principles, Fitts's/Hick's Law, UI pattern table, state design checklist (74→141 lines)
- **react-native-firebase** — Added @react-native-firebase setup, Auth/Firestore/FCM patterns, native config files (72→221 lines)
- **frontend-design** — Frontmatter standardization only (Anthropic-sourced, already substantive)
- **figma** — Added REST API endpoints, Dev Mode description, authentication note (44→70 lines)
- **figma-implement-design** — Frontmatter only (already excellent at 265 lines)
- **playwright** — Added Playwright Test Framework section, codegen commands, key API reference (148→196 lines)
- **playwright-interactive** — Frontmatter only (already thorough at 691 lines)
- **brand-guidelines** — Frontmatter standardization only
- **canvas-design** — Frontmatter standardization only
- **theme-factory** — Frontmatter standardization only
- **webapp-testing** — Frontmatter standardization only
- **web-artifacts-builder** — Frontmatter standardization only
- **winui-app** — Frontmatter standardization only

### Category 09 — Backend, API, and Data (20 skills)

Technology-specific backend skills. Template stubs replaced with real framework APIs, CLI commands, and code patterns.

- **api-contracts** — Added OpenAPI spec authoring, contract-first workflow, schema validation tools
- **api-debugging** — Added curl/httpie patterns, request tracing, status code interpretation
- **aspnet-core** — Added ASP.NET Core middleware pipeline, dependency injection, minimal APIs, Entity Framework patterns
- **background-jobs-queues** — Added Celery/Bull/Sidekiq patterns, retry strategies, dead letter handling
- **bigquery** — Added SQL patterns, partitioning/clustering, cost optimization, dbt integration
- **data-model** — Added schema design principles, normalization/denormalization decisions, migration strategies
- **express-node** — REWRITE: Added middleware chain ordering, Router composition, error-handling middleware, zod validation, helmet/cors security, TypeScript integration, PM2 clustering
- **fastapi** — REWRITE: Added Pydantic models, dependency injection, lifespan wiring, async patterns, response_model contracts, background tasks
- **firebase-rules** — Added security rules syntax, unit testing with emulator, common patterns
- **firebase-sdk** — Added client SDK initialization, Firestore/Auth/Storage patterns, real-time listeners
- **flask** — REWRITE: Added application factory, Blueprint organization, extension wiring (SQLAlchemy/Migrate/Login/WTF), WSGI deployment
- **go-api-service** — REWRITE: Added net/http handlers, chi/gin routing, middleware chains, structured logging, graceful shutdown
- **observability-logging** — Added structured logging (slog/zerolog/winston), OpenTelemetry tracing, metric collection
- **orm-patterns** — Added SQLAlchemy/Prisma/TypeORM patterns, N+1 prevention, migration strategies
- **postgresql** — Added query optimization, indexing strategies, EXPLAIN analysis, connection pooling, partitioning
- **python** — REWRITE: Added type hints, dataclasses, async patterns, virtual environments, pytest, packaging
- **rate-limits-retries** — Added token bucket/sliding window algorithms, retry with exponential backoff, circuit breaker
- **realtime-websocket** — Added WebSocket lifecycle, Socket.io/ws patterns, reconnection handling, room/channel patterns
- **sqlite** — Added WAL mode, pragma tuning, migration patterns, connection pooling for concurrent access
- **webhooks-events** — Added webhook receiver patterns, signature verification, idempotency, retry handling

### Category 10 — CLI, Systems, and Ops (20 skills)

Command-line tool development and systems operations. Improvements added specific tool/library references and concrete code patterns.

- **bash** — Added set -euo pipefail patterns, ShellCheck compliance, quoting rules, trap/cleanup
- **bubbletea-go** — Added Bubble Tea Model/Update/View pattern, tea.Cmd, Lipgloss styling, Bubbles components
- **cli-development-go** — Added project structure, Cobra+Viper integration, testing CLI commands, release with GoReleaser
- **cli-development-python** — Added Click/Typer patterns, argument parsing, rich output, packaging with setuptools/poetry
- **cobra-go** — Added Cobra command definitions, PersistentPreRunE chains, ValidArgsFunction, shell completion
- **config-files-xdg** — Added XDG Base Directory paths, config file loading order, TOML/YAML patterns
- **cross-platform-shell** — Added POSIX compatibility patterns, OS detection, path handling differences
- **gh-address-comments** — Added gh CLI patterns for addressing PR review comments programmatically
- **gh-fix-ci** — Added gh CLI patterns for diagnosing and fixing CI failures from workflow logs
- **linear** — Added Linear API integration, issue creation/update, project management patterns
- **linux-ubuntu-ops** — Added apt/systemctl/journalctl patterns, user management, firewall configuration
- **packaging-installers** — Added deb/rpm packaging, Homebrew formula, cross-platform installer patterns
- **proxmox-shell-scripting** — Added Proxmox API scripting, VM/container management, backup automation
- **release-binaries** — Added cross-compilation, GoReleaser/cargo-dist, GitHub Release asset management
- **sentry** — Added Sentry SDK integration, error capturing, performance monitoring, release tracking
- **ssh-tmux-remote-workflow** — Added SSH config patterns, tmux session management, remote development workflows
- **systemd-services** — Added .service unit files, timer units (cron replacement), socket activation, sandboxing (ProtectSystem/DynamicUser)
- **terminal-debugging** — Added strace/ltrace patterns, /proc inspection, network debugging tools
- **tui-development** — Added TUI framework selection (Bubble Tea vs tview vs cursive), layout patterns
- **yeet** — Added quick deployment/push workflow patterns

### Category 11 — AI/LLM Runtime and Integration (28 skills)

LLM application development skills. Improvements added specific API patterns, model references, and integration code.

- **agent-memory** — Added memory architectures (buffer, summary, vector, entity), LangChain memory types
- **chatgpt-apps** — Added ChatGPT plugin/action development, OpenAPI spec requirements
- **claude-api** — Added Anthropic API patterns, message structure, tool use, streaming, system prompts
- **context-management-memory** — Added context window strategies, summarization chains, sliding window patterns
- **embeddings-indexing** — Added embedding model selection, vector indexing (HNSW/IVF), batch processing
- **imagegen** — Added DALL-E/Stable Diffusion API usage, prompt engineering for images
- **inference-serving** — Added vLLM/TGI deployment, batch inference, scaling patterns
- **llama-cpp** — Added llama.cpp build/run, GGUF model loading, server mode, quantization options
- **llm-evals-benchmarks** — Added eval framework setup, prompt-response scoring, regression detection
- **llm-integration** — Added LLM client patterns, retry/fallback, cost tracking, provider abstraction
- **local-llm** — Added local model setup, hardware requirements, quantization tradeoffs
- **model-routing** — Added multi-model routing strategies, cost/quality/latency tradeoffs, fallback chains
- **model-selection** — Added model comparison frameworks, benchmark interpretation, use case matching
- **multimodal-ai** — Added vision/audio/text multimodal patterns, GPT-4V/Claude vision API usage
- **offline-cpu-inference** — Added CPU-optimized inference, ONNX Runtime, quantized model serving
- **ollama** — Added Modelfile authoring, API endpoints (/api/chat, /api/generate, /api/embeddings), GPU tuning, environment variables
- **openai-docs** — Added OpenAI API reference patterns, SDK usage, best practices
- **python-llm-ml-workflow** — Added end-to-end ML project structure, experiment tracking, model registry
- **quantization-strategy** — Added GPTQ/AWQ/BitsAndBytes selection guide, quality/speed tradeoffs
- **rag-retrieval** — Added chunking strategies (recursive/semantic/parent-child), hybrid BM25+dense search, reranking stages, vector store configuration
- **retrieval-quality** — Added retrieval evaluation metrics (MRR, NDCG, recall@k), diagnostic patterns
- **safety-guardrails** — Added content filtering, jailbreak detection, output validation
- **sora** — Added Sora video generation API patterns and prompt engineering
- **speech** — Added speech-to-text/text-to-speech API patterns, Whisper, TTS models
- **structured-output-pipelines** — Added JSON schema constraints, Pydantic binding, function calling extraction, retry/repair loops
- **tool-use-agents** — Added tool definition patterns, execution loops, error recovery
- **transcribe** — Added audio transcription pipelines, Whisper API, post-processing
- **vllm-serving** — Added vLLM server configuration, PagedAttention, continuous batching, quantized serving

### Category 12 — AI/LLM Training, Architecture, and Research (23 skills)

22 template stubs had prompt-blob syndrome with zero ML content. All rewritten with real framework APIs (PyTorch, HuggingFace, TRL) and paper references.

- **benchmark-design** — Added lm-evaluation-harness configs, pass@k estimator, contamination detection, bootstrap CIs
- **data-cleaning-labeling** — Added MinHash dedup, fasttext language ID, KenLM perplexity scoring, PII removal with Presidio
- **dataset-curation** — Added domain mixing ratios, datatrove/RedPajama pipelines, Chinchilla token budgets, decontamination
- **dense-to-moe-experiments** — Added FFN upcycling, split+perturb initialization, top-k gating with PyTorch code, GShard/Switch aux loss
- **distillation-compression** — Added KL divergence with temperature, feature matching, attention transfer, DistilBERT/TinyBERT/MiniLM patterns
- **eval-dataset-design** — Added evaluation task taxonomy, contamination prevention with canary strings, scoring rubrics
- **fine-tuning** — Added LoraConfig, QLoRA with BitsAndBytesConfig, SFTTrainer setup, complete training configuration; LoRA/QLoRA paper refs
- **inference-kernel-optimization** — Added FlashAttention tiling, KV cache paged attention, operator fusion, Triton kernels, TensorRT-LLM
- **instruction-tuning** — Added chat template application, Alpaca/ShareGPT/OpenAI formats, response-only loss masking, sequence packing
- **llm-creation** — Added architecture decisions, scaling laws (Chinchilla ~20 tokens/param), FLOPs estimation, training pipeline stages
- **model-architecture** — Added MHA/GQA/MQA attention variants, RoPE/ALiBi positional encoding, Pre-LN vs Post-LN, SwiGLU/GELU
- **model-merging** — Added SLERP, TIES, DARE, task arithmetic, frankenmerging, mergekit YAML config format
- **moe-architecture** — Added router design, load balancing auxiliary loss, expert capacity factor, Mixtral/Switch Transformer patterns
- **preference-optimization** — Added DPO loss formula, TRL DPOTrainer, DPOConfig, IPO/KTO/ORPO alternatives; paper references
- **pretraining-pipeline** — Added streaming datasets, FSDP/DeepSpeed ZeRO stages, learning rate schedules, MFU tracking
- **quantization-research** — Added GPTQ (Hessian-based), AWQ (activation-aware), BitsAndBytes NF4, calibration procedures
- **reward-modeling** — Added Bradley-Terry preference model, value head architecture, TRL RewardTrainer, reward hacking prevention
- **safety-alignment** — Added red teaming categories, Constitutional AI, safety benchmarks (ToxiGen, BBQ, TruthfulQA), over-refusal prevention
- **serving-architecture** — Added vLLM/TGI/TensorRT-LLM, continuous batching, PagedAttention, speculative decoding, streaming
- **synthetic-data-generation** — Added Self-Instruct, Evol-Instruct, distillation-based generation, quality filtering
- **tokenizer-design** — Added BPE merge rules, SentencePiece unigram, vocabulary size tradeoffs, pre-tokenization strategies
- **training-infrastructure** — Added DDP/FSDP/DeepSpeed ZeRO configs, multi-node NCCL, torchrun commands, mixed precision, SLURM
- **jupyter-notebook** — No changes needed (already well-developed, OpenAI-sourced)

### Category 13 — Game Engines and Creative Tech (17 skills)

15 template stubs had zero engine-specific content. All rewritten with real engine APIs (Unity C#, UE5 C++, Godot GDScript).

- **ai-npc-behavior** — Added behavior trees, FSMs, utility AI scoring, GOAP, steering behaviors; Unity/UE5/Godot NavMesh APIs
- **asset-pipeline** — Added texture compression (ASTC/BC7/ETC2), LOD generation; Unity Addressables, UE5 Asset Manager, Godot resource packs
- **blueprint-patterns** — Added Blueprint types, event dispatchers, Blueprint-C++ boundary (UFUNCTION), GAS/Enhanced Input integration
- **game-design-systems** — Added core loop design, economy sources/sinks, progression XP curves, combat damage formulas, data-driven design
- **game-ui-hud** — Added HUD elements, menu architecture; Unity UI Toolkit, UE5 UMG, Godot Control nodes; accessibility
- **godot** — Added GDScript 2.0 typed syntax, scene tree composition, Resources, node lifecycle, physics, state machines
- **input-mapping-controller** — Added action-based mapping; Unity New Input System, UE5 Enhanced Input, Godot Input Map; input buffering
- **multiplayer-netcode** — Added client-server architecture, lag compensation, rollback netcode; Unity Netcode, UE5 Replication, Godot @rpc
- **performance-profiling-games** — Added frame budget analysis, RenderDoc/NVIDIA Nsight profiling, draw call batching, LOD/occlusion culling
- **save-load-state** — Added serialization formats, versioning; Unity JsonUtility, UE5 USaveGame, Godot ConfigFile; cloud saves, anti-tamper
- **ue5-blueprint** — Added Blueprint class types, event graph patterns, GAS in Blueprints, nativization
- **unity** — Added MonoBehaviour lifecycle, component architecture, ScriptableObject, Addressables, New Input System, ECS/DOTS
- **unity-scriptableobject-events** — Added GameEvent SO pattern, runtime sets, Ryan Hipple GDC patterns
- **unreal-engine** — Added UObject reflection, AActor lifecycle, Gameplay Framework, GAS, Enhanced Input, asset management, memory patterns
- **worldbuilding-lore-systems** — Added lore databases, narrative graphs, codex/encyclopedia systems, procedural generation
- **algorithmic-art** — No changes needed (already 406 lines, Anthropic-sourced)
- **develop-web-game** — No changes needed (already 150 lines, OpenAI-sourced)

### Category 14 — Cloud, Platform, and DevOps (19 skills)

Infrastructure and deployment skills with real CLI commands, configuration examples, and platform-specific patterns.

- **aws** — Added AWS CLI patterns, IAM policies, common service configurations (S3, Lambda, EC2, RDS)
- **cloud-deploy** — Added multi-cloud deployment strategies, environment promotion, rollback procedures
- **cloudflare** — Added Cloudflare API/CLI, DNS management, WAF rules, caching configuration
- **cloudflare-deploy** — Added Wrangler CLI deployment workflow, environment configuration
- **cloudflare-worker-patterns** — Added Workers runtime patterns, KV/D1/R2 storage, Durable Objects, cron triggers
- **cost-monitoring** — Added cloud cost analysis, budget alerting, right-sizing recommendations
- **docker-containers** — Added multi-stage Dockerfiles, docker-compose, health checks, image scanning (docker scout/Trivy), BuildKit features
- **firebase** — Added Firebase project setup, hosting deployment, Functions v2, Firestore indexes
- **gcp** — Added gcloud CLI patterns, GKE, Cloud Run, BigQuery, IAM configurations
- **netlify-deploy** — Added Netlify CLI deployment, build configuration, serverless functions
- **queues-cron-workers** — Added queue architectures (SQS/Cloud Tasks/BullMQ), cron scheduling, worker patterns
- **render-deploy** — Added Render deployment configuration (already comprehensive at 481 lines)
- **secret-management** — Added Vault/AWS Secrets Manager/GCP Secret Manager patterns, rotation, injection
- **self-hosting-ops** — Added reverse proxy (nginx/Caddy), SSL automation, monitoring, backup strategies
- **serverless-patterns** — Added Lambda/Cloud Functions patterns, cold start mitigation, event-driven architectures
- **tailscale-private-networking** — Added Tailscale ACL configuration, subnet routing, exit nodes
- **terraform-iac** — Added HCL module authoring, state management, multi-environment structure, drift resolution
- **vercel** — Added Vercel platform configuration, framework detection, environment variables
- **vercel-deploy** — Added Vercel CLI deployment workflow, preview/production environments

### Category 15 — Docs, Artifacts, and Media (33 skills)

Document processing, generation, and media skills. Mix of Anthropic/OpenAI-sourced tools and new skills.

- **adr-rfc-writing** — Added ADR template (context/decision/consequences), RFC structure, decision log patterns
- **csv-ready** — Added CSV parsing patterns, encoding detection, delimiter handling
- **doc** — Added python-docx patterns, render_docx.py visual validation workflow
- **doc-coauthoring** — Added collaborative writing workflow, revision tracking, merge strategies (377 lines)
- **document-to-structured-data** — Added extraction patterns, schema mapping, validation
- **document-writing** — Added structured document templates, style consistency, outline-first workflow
- **docx** — Added comprehensive python-docx usage (already 591 lines)
- **docx-generation** — Added programmatic DOCX creation patterns
- **image-editor** — Added PIL/Pillow patterns, image manipulation operations
- **image-heavy-pdfs** — Added PDF generation with embedded images, layout optimization
- **internal-comms** — Added communication templates, audience-appropriate tone
- **meeting-notes-decision-log** — Added structured meeting notes, decision tracking, action items
- **notion-knowledge-capture** — Added Notion API patterns, database creation, page templates
- **notion-meeting-intelligence** — Added meeting-to-Notion workflow, transcript processing
- **notion-research-documentation** — Added research documentation structure in Notion
- **notion-spec-to-implementation** — Added spec tracking in Notion databases, status management
- **pdf** — Added pypdf/pdfplumber patterns, text extraction, merging, form filling (Anthropic-sourced)
- **pdf-editor** — Added PDF modification patterns, annotation, page manipulation
- **pdf-extraction** — Added table extraction, OCR patterns, structured data from PDFs
- **pdf-generation** — Added ReportLab/WeasyPrint patterns, template-based generation
- **pptx** — Added python-pptx patterns, slide layout, chart integration
- **pptx-generation** — Added programmatic PowerPoint creation
- **release-notes** — Added changelog generation from commits, audience-targeted release notes
- **research-synthesis** — Added multi-source synthesis, evidence hierarchy, gap identification
- **runbook-writing** — Added runbook template, step-by-step procedures, escalation paths
- **screenshot** — Added screenshot capture patterns, annotation, comparison
- **slack-gif-creator** — Added Slack message formatting, GIF creation patterns
- **slides** — Added presentation structure, narrative flow, slide design principles
- **spec-authoring** — Added specification template, requirements tracing, acceptance criteria
- **spreadsheet** — Added openpyxl patterns, formula handling, data validation
- **table-extraction** — Added table detection and extraction from PDFs/images/HTML
- **xlsx** — Added comprehensive Excel processing (openpyxl-based)
- **xlsx-generation** — Added programmatic Excel creation with formatting

### Category 16 — Business, Research, and Optional Domains (12 skills)

Domain-specific skills for business analysis, research, and specialized use cases.

- **app-publishing** — Added app store submission workflows, metadata requirements, review guidelines
- **business-idea-evaluation** — Added lean canvas, viability scoring, risk assessment framework
- **competitor-teardown** — Added UX teardown methodology, feature comparison matrix, SWOT analysis
- **cv-cover-letter** — Added STAR method bullets, ATS-optimized formatting, keyword matching, truthfulness guardrails
- **domain-scouting** — Added domain name evaluation, availability checking, brand alignment
- **financial-tracker-ops** — Added financial data management, reporting patterns
- **image-prompt-direction** — Added image generation prompt engineering, style direction, iteration
- **market-research** — Added TAM/SAM/SOM sizing, competitive landscape mapping, Porter's Five Forces, evidence-sourced claims with quality tiers
- **property-research** — Added property analysis frameworks, market comparison
- **spreadsheet-analysis** — Added data analysis patterns, pivot tables, visualization recommendations
- **starcraft-data-analysis** — Added SC2 replay analysis, build order detection, player statistics
- **worldbuilding-research** — Added world design frameworks, consistency checking, cultural systems

### Category 17 — External Reference Seeds (8 skills)

Third-party and cross-cutting reference skills. Generally well-developed; minor improvements.

- **bigquery-skill** — Added BigQuery-specific SQL patterns, cost optimization, dbt integration
- **cargo-lock-manager** — Added Cargo dependency auditing, cargo-audit/cargo-deny, version pinning, workspace management
- **fastapi-patterns** — Added FastAPI-specific patterns beyond basic CRUD (WebSocket, BackgroundTasks, middleware)
- **frontend-webapp-builder** — Added full-stack webapp scaffolding workflow
- **linear-address-issue** — Added Linear API issue management patterns
- **misc-helper** — General-purpose utility skill for miscellaneous tasks
- **solidjs-patterns** — Added SolidJS fine-grained reactivity (createSignal, createStore, createResource), control flow, React migration patterns (422 lines)
- **tauri-solidjs** — Added Tauri + SolidJS integration, IPC commands, plugin patterns

---

## Scafforge Comparison

### Key Workflow Terms Preserved
All 10 Scafforge skills in Category 01 preserve their canonical workflow logic:

| Term | Where Used | Status |
|------|-----------|--------|
| Decision tree (greenfield/retrofit/refinement) | scaffold-kickoff | ✅ Preserved |
| 10-step flow with gates | scaffold-kickoff | ✅ Preserved |
| Role-based prompt contracts | agent-prompt-engineering | ✅ Preserved |
| 12-section brief schema | spec-pack-normalizer | ✅ Preserved |
| Two-phase model (script + agent) | repo-scaffold-factory | ✅ Preserved |
| Baseline agent table | opencode-team-bootstrap | ✅ Preserved |
| Wave system (0-3) | ticket-pack-builder | ✅ Preserved |
| Manifest-as-truth / board-as-view | ticket-pack-builder | ✅ Preserved |
| Foundation/synthesis modes | project-skill-bootstrap | ✅ Preserved |
| Three modes (audit/propose/apply) | repo-process-doctor | ✅ Preserved |
| Safe vs intent-changing boundary | repo-process-doctor | ✅ Preserved |
| Canonical ticket proposals | pr-review-ticket-bridge | ✅ Preserved |

### Broadening Applied
All Scafforge skills were broadened beyond OpenCode-specific paths:
- `.opencode/agents/*.yaml` → agent config directories (`.opencode/`, `.copilot/`, `.codex/`, or equivalent)
- OpenCode-specific tool names → generic tool references where the concept is universal

---

## Quality Metrics

| Metric | Value |
|--------|-------|
| Total skills | 350 (+ 2 in skill-improver/) |
| Total SKILL.md files | 352 |
| Boilerplate stubs remaining | 0 |
| Total lines across all SKILL.md | 92,052 |
| Average lines per skill | 261 |
| Smallest skill | 26 lines (skill-installation — consolidated) |
| Largest skill | 700 lines (playwright-interactive) |
| Skills with DO NOT USE boundaries | 308 / 352 (87.5%) |
| Skills with named failure modes | 44 / 352 (12.5% — concentrated in Cat 04) |
| Categories | 17 + skill-improver |

### Line Count Distribution
| Range | Count | Notes |
|-------|-------|-------|
| < 50 lines | ~8 | Consolidated/role stubs |
| 50–100 lines | ~80 | Focused domain skills |
| 100–200 lines | ~180 | Standard improved skills |
| 200–400 lines | ~70 | Comprehensive skills |
| 400+ lines | ~14 | Deep reference skills |

---

## Rationale

### Why These Changes Matter

**Routing accuracy**: The description field is routing logic — it determines whether a host invokes a skill. Generic descriptions like "Use this when creating, adapting, refining, installing, testing, or packaging a skill" match everything and route nothing. Specific triggers like "Use when a skill isn't triggering when it should (undertriggering)" match the right task.

**Concrete procedures**: A skill whose procedure is "Define the task boundary → Collect context → Choose approach → Verify result → Report next action" provides zero value — that's just restating the agent loop. Skills must contain the actual technique: the specific steps, decision trees, code patterns, and validation commands that make the skill worth invoking.

**Domain specificity**: A fine-tuning skill without LoraConfig, a Unity skill without MonoBehaviour lifecycle, or a red-teaming skill without attack vector categories is a placeholder, not a skill. Domain content is what transforms a routing label into executable knowledge.

**Named failure modes**: Generic "handle errors" gives agents nothing to watch for. Named failure modes like "tourist red-team" (superficial attacks that don't probe real weaknesses) or "kitchen-sink trigger" (description that matches everything) give agents specific patterns to detect and avoid.

**Scafforge workflow preservation**: The Scafforge skills encode a specific multi-skill workflow (spec → scaffold → customize → ticket → execute). Losing step sequences, gates, or mode systems would break the orchestration that makes the skills work as a system rather than isolated procedures.
