# Node Registry

Every node in this registry maps 1:1 to a plugin from the source architecture. Nodes are not merged, collapsed, or abstracted. If a node's purpose is unclear, it stays as a stub — that's the scaffold working as intended.

Status key:
- **core** — essential to repo maintenance, activate early
- **supporting** — useful when the situation calls for it
- **integration** — bridges to an external service
- **language** — code intelligence for a specific language
- **build** — constructs things (components, pages, infra)
- **governance** — watches, guards, enforces
- **style** — shapes output format or voice
- **transform** — converts between formats
- **meta** — operates on the system itself
- **diagnostic** — tests that the system works
- **ops** — monitoring, observability, runtime health
- **stub** — purpose unknown or unclear, preserved for later

---

## Cluster 01: Git Operations & Release

### git-release
- **Category:** core
- **Function:** Version tagging, changelog generation, release notes, GitHub releases
- **Repo role:** The heartbeat. Every version that goes out passes through here.

### git-ship
- **Category:** core
- **Function:** Push workflows, branch-to-deploy pipelines, ship-it automation
- **Repo role:** The last mile — code is ready, this gets it out the door.

### review-submission
- **Category:** core
- **Function:** PR review, code review checklists, submission quality gates
- **Repo role:** Gatekeeper. Nothing merges without passing through this node.

### gitlab-mr-review
- **Category:** integration
- **Function:** Merge request review for GitLab repos
- **Repo role:** Parallel path to review-submission for GitLab-hosted projects. Same function, different platform.

---

## Cluster 02: Development Workflow & Lifecycle

### dev-workflow
- **Category:** core
- **Function:** Day-to-day dev patterns — branching strategy, commit conventions, workflow orchestration
- **Repo role:** The operating rhythm. How work moves from "I'm starting this" to "this is done."

### dev-cycle
- **Category:** core
- **Function:** Sprint/iteration management, feature lifecycle from idea to merge
- **Repo role:** The larger arc — where dev-workflow is the day, dev-cycle is the week/sprint.

### dev-sandbox
- **Category:** core
- **Function:** Isolated test environments, throwaway branches, safe experimentation
- **Repo role:** The safe space. Try things without breaking things.

### cursor-team-kit
- **Category:** supporting
- **Function:** IDE-level team coordination, shared configs, editor conventions
- **Repo role:** Makes sure everyone's editor is speaking the same language. .editorconfig, shared snippets, workspace settings.

---

## Cluster 03: Project Management

### jira
- **Category:** integration
- **Function:** Issue tracking, ticket lifecycle, sprint boards
- **Repo role:** External issue tracker bridge. Links commits to tickets, syncs status.

### monday
- **Category:** integration
- **Function:** Kanban boards, timelines, team dashboards
- **Repo role:** Alternative PM surface. Some projects live here instead of Jira.

### airtable
- **Category:** integration
- **Function:** Flexible tabular data layer — project registry, asset tracking, anything structured
- **Repo role:** The spreadsheet that isn't a spreadsheet. Good for tracking things that don't fit neatly into issues.

### ai-pm-copilot
- **Category:** core
- **Function:** Automated project management — standup summaries, blocker detection, velocity tracking
- **Repo role:** The PM brain. Reads the state of the repo and tells you what's happening.

### plan-guardian
- **Category:** governance
- **Function:** Scope creep detection, plan adherence, milestone tracking
- **Repo role:** Watches for drift. "You said you'd ship X by Friday. It's Thursday. Here's what's left."

### beast-plan
- **Category:** core
- **Function:** Heavy planning — architecture decisions, roadmap, big-picture strategy
- **Repo role:** The planning node for when the question isn't "what's the next task" but "what are we actually building and why."

### ocpm
- **Category:** stub
- **Function:** Unknown — possibly operational/change project management
- **Repo role:** Placeholder. Name suggests ops-level change management.

### ccpm
- **Category:** supporting
- **Function:** Critical chain project management — dependency-aware scheduling
- **Repo role:** When tasks have hard dependencies and the critical path matters. Not every project needs this; the ones that do really need it.

---

## Cluster 04: Testing & QA

### codeceptjs-e2e-tests
- **Category:** core
- **Function:** End-to-end browser testing with CodeceptJS framework
- **Repo role:** Full-stack integration tests. Does the thing actually work when a user clicks the button.

### test-automation-generator
- **Category:** core
- **Function:** Automatically generates test cases from code or specifications
- **Repo role:** Test scaffolding. Reads your code, writes the tests you should have written.

### lorikeet-qa
- **Category:** supporting
- **Function:** QA workflows — possibly visual regression, acceptance testing, or QA process management
- **Repo role:** The QA process itself, not just the test runner. Checklists, sign-offs, regression tracking.

---

## Cluster 05: Deployment & Infrastructure

### dokploy
- **Category:** build
- **Function:** Self-hosted deployment platform, Docker-based deploy orchestration
- **Repo role:** Deploy engine for self-hosted projects. Containers up, services running.

### terraform-ls
- **Category:** build
- **Function:** Infrastructure as code via Terraform — language server for IaC authoring and review
- **Repo role:** When infrastructure is code in the repo, this node reads and validates it.

### vercel-best-practices
- **Category:** governance
- **Function:** Vercel-specific deployment patterns, edge functions, build configuration
- **Repo role:** Best practices layer for Vercel-deployed projects. Not a deploy engine itself — more like the building code inspector for Vercel.

### aws-diagram
- **Category:** supporting
- **Function:** AWS architecture diagramming — visual infrastructure documentation
- **Repo role:** Makes the infrastructure legible. Generates architecture diagrams from AWS configs.

---

## Cluster 06: Language Servers / Code Intelligence

These nodes are all the same structural pattern: code intelligence for a specific language. In the repo maintenance context, they're the eyes — they let the maintainer read, navigate, lint, and reason about code in that language. Each is discrete because each language has its own semantics.

### csharp-roslyn-lsp
- **Category:** language
- **Function:** C# code intelligence via Microsoft Roslyn compiler platform
- **Repo role:** Eyes for C# codebases.

### omnisharp-lsp
- **Category:** language
- **Function:** Alternative C# language server (OmniSharp)
- **Repo role:** Second pair of C# eyes. Some projects prefer OmniSharp over Roslyn.

### pyrefly-lsp
- **Category:** language
- **Function:** Python code intelligence
- **Repo role:** Eyes for Python codebases.

### apex-lsp
- **Category:** language
- **Function:** Salesforce Apex language server
- **Repo role:** Eyes for Apex/Salesforce codebases.

### perlnavigator-lsp
- **Category:** language
- **Function:** Perl language server
- **Repo role:** Eyes for Perl codebases.

### gdscript-lsp
- **Category:** language
- **Function:** Godot Engine GDScript language server
- **Repo role:** Eyes for Godot game projects.

### cds-lsp
- **Category:** language
- **Function:** SAP Core Data Services language server
- **Repo role:** Eyes for SAP/CDS projects.

### lean-lsp
- **Category:** language
- **Function:** Lean theorem prover language server
- **Repo role:** Eyes for formal verification / Lean projects.

### typescript-native-lsp
- **Category:** language
- **Function:** Native TypeScript language server
- **Repo role:** Eyes for TypeScript codebases. Likely the most frequently activated language node.

---

## Cluster 07: Documentation & Specs

### doc-bootstrap
- **Category:** core
- **Function:** Initial project documentation scaffolding — README, CONTRIBUTING, LICENSE, directory structure docs
- **Repo role:** Day zero documentation. Every new repo passes through here.

### spec-writer
- **Category:** core
- **Function:** Technical specification authoring
- **Repo role:** Writes the specs that define what gets built. Architecture docs, technical designs, interface contracts.

### prd-generator
- **Category:** core
- **Function:** Product requirement document generation
- **Repo role:** The "what and why" document. Translates intent into structured requirements.

### openspec
- **Category:** supporting
- **Function:** Open specification format — API specs, schema documentation, standards-based specs
- **Repo role:** When the spec needs to be machine-readable or follow a standard format (OpenAPI, JSON Schema, etc.).

### docs-search-tool
- **Category:** supporting
- **Function:** Search across project documentation
- **Repo role:** Find things in the docs. Useful when the documentation corpus gets large enough that you can't just read it all.

### microsoft-learn
- **Category:** supporting
- **Function:** Microsoft ecosystem documentation reference
- **Repo role:** Reference node for .NET, Azure, Windows, and Microsoft stack documentation.

---

## Cluster 08: Frontend & Design

### frontend-lab
- **Category:** build
- **Function:** Frontend prototyping and component experimentation
- **Repo role:** The workbench for trying out UI ideas before they're committed.

### grid-design
- **Category:** build
- **Function:** CSS grid and layout system design
- **Repo role:** Layout architecture. How things are arranged on screen.

### design-principles
- **Category:** governance
- **Function:** Design system governance — consistency, accessibility, pattern adherence
- **Repo role:** The style guide enforcer. Makes sure the UI isn't a patchwork of 12 different design languages.

### prototype
- **Category:** build
- **Function:** Rapid prototyping
- **Repo role:** Fast, throwaway builds to test an idea.

### prototyper
- **Category:** build
- **Function:** Appears to be a variant or companion to prototype
- **Repo role:** Same neighbourhood. May be a different approach to the same function (e.g., code-based vs visual prototyping).

### silince-gutnebrg-builder
- **Category:** build
- **Function:** WordPress Gutenberg block builder
- **Repo role:** Custom block development for WordPress sites. Name has a typo (Gutenberg), noted and preserved as-is from source.

### vertical-builder
- **Category:** build
- **Function:** Vertical-specific application scaffolding
- **Repo role:** When you're building something for a specific industry/vertical, this scaffolds the domain-specific parts.

---

## Cluster 09: Memory & Context

This cluster is arguably the most critical. Without memory and context continuity, the repo maintainer starts fresh every session — which means it's not really a maintainer, it's a contractor who shows up and asks "so what are we doing today?" every morning.

### claude-memory
- **Category:** core
- **Function:** Persistent memory across sessions
- **Repo role:** The continuity layer. Remembers what happened last time.

### memory-agent
- **Category:** core
- **Function:** Active memory management — retrieval, pruning, relevance scoring
- **Repo role:** Doesn't just store memories, manages them. Decides what's worth remembering and what's noise.

### vectorhub-memory
- **Category:** core
- **Function:** Vector-based semantic memory — search by meaning, not keyword
- **Repo role:** "What was that thing we did with the authentication flow?" works even if you don't remember the exact words.

### context
- **Category:** core
- **Function:** Context window management
- **Repo role:** Manages what's in the active working set. What the maintainer is "thinking about" right now.

### context-handoff
- **Category:** core
- **Function:** Cross-session context transfer
- **Repo role:** When a session ends and a new one begins, this is what passes the baton.

### continual-learning
- **Category:** core
- **Function:** Learning from past interactions, pattern recognition, improving over time
- **Repo role:** The maintainer gets better at maintaining this specific repo over time. Learns preferences, patterns, anti-patterns.

---

## Cluster 10: Autonomous Operation

### autonomous-loop
- **Category:** core
- **Function:** Self-directed work loops — continue working without being prompted
- **Repo role:** The engine that lets the maintainer do a multi-step task without asking "now what?" after each step.

### agent-teams
- **Category:** core
- **Function:** Multi-agent coordination — spawn specialist agents for subtasks
- **Repo role:** Delegation. When a task has multiple independent parts, spin up agents to handle them in parallel.

### hardworking
- **Category:** meta
- **Function:** Persistence, thoroughness, don't-give-up-early behavioural patterns
- **Repo role:** A meta-node. Not a function but a disposition. "Keep going until it's actually done."

---

## Cluster 11: Backend & Language-Specific Build

### backend-specialist
- **Category:** build
- **Function:** Server-side architecture, API design, database schema patterns
- **Repo role:** The backend brain. Knows how to structure a server, design an API, normalize a database.

### bun-typescript
- **Category:** build
- **Function:** Bun runtime + TypeScript development tooling
- **Repo role:** Build tooling for Bun-based TypeScript projects. Fast runtime, specific patterns.

### dune
- **Category:** build
- **Function:** OCaml/Reason build system
- **Repo role:** Build tooling for OCaml projects. Dune is the standard build system in that ecosystem.

---

## Cluster 12: Security

### forge-security
- **Category:** governance
- **Function:** Security scanning, vulnerability assessment, dependency auditing
- **Repo role:** The security reviewer. Scans deps for CVEs, checks for secrets in code, flags insecure patterns.

### ida-reverse-engineer
- **Category:** build
- **Function:** Binary analysis, reverse engineering, disassembly
- **Repo role:** Specialist node. When you need to understand what a binary does, or verify compiled output matches source.

---

## Cluster 13: Analytics & Monitoring

### user-journey-analysis
- **Category:** ops
- **Function:** User flow analysis, funnel tracking, journey mapping
- **Repo role:** Understands how users move through the product. Connects code changes to user behaviour.

### datadog
- **Category:** ops
- **Function:** Application monitoring, alerting, observability, metrics
- **Repo role:** Runtime health. Is the deployed thing actually working? Are response times degrading? Are errors spiking?

### feature-ears
- **Category:** ops
- **Function:** Feature flag monitoring, rollout tracking, feature adoption metrics
- **Repo role:** Tracks what features are live, how they're performing, whether they should be rolled back.

---

## Cluster 14: Content & Creative

### creative-music-output-style
- **Category:** style
- **Function:** Creative output formatting tuned for music domain
- **Repo role:** Style node. When output needs to speak the language of music production/composition.

### dj-content-creator
- **Category:** style
- **Function:** DJ and music content generation
- **Repo role:** Content creation node for music/DJ-specific projects.

### pdf2latex
- **Category:** transform
- **Function:** PDF to LaTeX conversion
- **Repo role:** Format bridge. Takes PDF inputs and produces LaTeX source.

### latex2cn
- **Category:** transform
- **Function:** LaTeX conversion — possibly to Chinese, or a LaTeX transformation variant
- **Repo role:** Format bridge. LaTeX in, transformed LaTeX (or localized content) out.

### ppt-loop
- **Category:** transform
- **Function:** Presentation iteration and automation
- **Repo role:** Automated presentation generation/updates. When slides need to be built or refreshed from data.

### why-how-what-output-style
- **Category:** style
- **Function:** Simon Sinek's Golden Circle output structuring — Start With Why
- **Repo role:** Style node. Structures output as Why → How → What. Useful for READMEs, pitches, project intros.

---

## Cluster 15: Integrations & Services

### freshservice
- **Category:** integration
- **Function:** IT service management platform
- **Repo role:** Bridge to ITSM. Ticket creation, incident tracking, service desk workflows.

### it-triage-system
- **Category:** integration
- **Function:** IT ticket triage and priority routing
- **Repo role:** When issues come in, this decides where they go and how urgent they are.

### n8n
- **Category:** integration
- **Function:** Workflow automation platform (open-source Zapier alternative)
- **Repo role:** The glue between services. Triggers, webhooks, automated pipelines.

### n8n-skills
- **Category:** integration
- **Function:** n8n-specific workflow capabilities and patterns
- **Repo role:** Companion to n8n node — specific recipes and patterns for n8n workflows.

### miro
- **Category:** integration
- **Function:** Visual collaboration, whiteboarding, diagramming
- **Repo role:** When planning needs to be visual. Architecture sessions, brainstorming, flow mapping.

### amber-electric
- **Category:** integration
- **Function:** Australian energy provider API integration
- **Repo role:** Domain-specific integration. Connects to Amber Electric for energy data/pricing.

### home-assistant-skills
- **Category:** integration
- **Function:** Home automation platform integration
- **Repo role:** IoT/home automation bridge. For projects that touch Home Assistant.

---

## Cluster 16: Meta & Utilities

### hello-world
- **Category:** diagnostic
- **Function:** Baseline test — the simplest possible operation
- **Repo role:** Smoke test. If this node doesn't work, nothing works. The canary.

### codex-skills
- **Category:** meta
- **Function:** Codex-specific capabilities and patterns
- **Repo role:** Meta-capability node for OpenAI Codex integration patterns.

### rs-commands
- **Category:** stub
- **Function:** Unknown — possibly Rust commands, or a command registry
- **Repo role:** Placeholder. Name suggests either Rust tooling or a general command set.

### any-chat-completions
- **Category:** meta
- **Function:** Generic LLM chat completion bridge — model-agnostic
- **Repo role:** The universal adapter. Route a prompt to any LLM endpoint.

### gemini-consult
- **Category:** meta
- **Function:** Cross-model consultation — ask Google Gemini for a second opinion
- **Repo role:** Second opinion node. When you want another model's take on something.

### my-time-plugin
- **Category:** supporting
- **Function:** Time tracking, scheduling, time management
- **Repo role:** Tracks time spent on repo tasks. Useful for billing, velocity, or just knowing where time goes.

### bullet-onboarding
- **Category:** supporting
- **Function:** Onboarding flow automation — quick structured onboarding
- **Repo role:** When a new contributor joins, this is what gets them oriented.

### awesome-claude-skills
- **Category:** meta
- **Function:** Skill discovery and registry — find existing skills
- **Repo role:** The index. What skills exist, what they do, where they are.

### claude-rules-generator
- **Category:** meta
- **Function:** Generate .claude rules files for projects
- **Repo role:** Creates the CLAUDE.md and rules that govern how Claude behaves in a specific repo.

### hashmind-synapse
- **Category:** stub
- **Function:** Unknown — name suggests knowledge graph or neural/associative linking
- **Repo role:** Placeholder. Could be a knowledge connection layer. Preserved for exploration.

### ralph-v2
- **Category:** stub
- **Function:** Unknown — Ralph is an open-source CMDB/asset management system
- **Repo role:** Placeholder. If it's Ralph the CMDB, it tracks hardware/software assets.

### hosts-db
- **Category:** supporting
- **Function:** Host and server database management
- **Repo role:** Tracks what's running where. Server inventory, host configurations.

### project-collaboration-system
- **Category:** supporting
- **Function:** Collaboration tooling — shared workspaces, team coordination
- **Repo role:** The collaboration layer. How multiple contributors coordinate on the same repo.

### ewo-discovery-skill
- **Category:** stub
- **Function:** Unknown — discovery or exploration pattern
- **Repo role:** Placeholder. Name suggests an exploration/discovery workflow.

### frappe-print-format
- **Category:** build
- **Function:** Frappe framework print format templates
- **Repo role:** Specialist node for Frappe/ERPNext projects. Generates print-ready document formats.

### universal-dev
- **Category:** meta
- **Function:** General-purpose development patterns — language/framework agnostic
- **Repo role:** The generalist. When no specialist node applies, this one handles it.
