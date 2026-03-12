# SPINE.md — The Limited-Slip Differential

This is the single source of truth for the repo-maintainer skill. It operates as a dual document — two output shafts of a limited-slip differential. **Spine Primary** is the shaft with traction (known state at session open). **Spine Beta** is the shaft that spins ahead during work. When the speed differential between them gets resolved at session close, the torque transfers to exactly the wheels that need it.

The differential doesn't prevent the shafts from turning at different speeds. It prevents uncontrolled divergence. Beta is supposed to pull ahead — that's work happening. What the mechanism catches is wheel spin: a reference file edited outside the spine, a vertebra that never propagated, a quadrant that drifted.

---

## The Drivetrain

### Primary and Beta (The Two Output Shafts)

**Spine Primary** is the shaft with traction. It's what was true when the session started. It holds steady. It doesn't get edited during a session — it's the baseline the differential measures against.

**Spine Beta** is the shaft that spins ahead. Every edit during a session writes to beta. New vertebrae, modified vertebrae, retracted vertebrae — all here. Beta is the only shaft receiving power during active work.

**At session close — the torque transfer:** overlay beta onto primary. Every vertebra that differs is:
1. Torque to be vectored — the quadrant alignment tells you exactly which wheel (reference file) receives it
2. Telemetry — the changelog is the readout of where the torque went, derived mechanically from the delta
3. A traction check — if a reference file matches neither primary nor beta, that wheel is spinning without load

Then beta becomes the new primary. The diff resets to zero. The cycle idles until the next session.

### Why this saves processing

The maintainer never re-reads the whole drivetrain. It never inspects every wheel individually. It just:
- Engages primary (known state — the shaft with traction)
- Powers beta (accumulates changes — the shaft spinning ahead)
- Reads the differential at close (the speed difference is everything)

If the delta is zero, both shafts are matched. No torque transfer needed, no telemetry to log, no traction to check. If the delta shows 3 vertebrae, those 3 vertebrae vector torque to exactly 3 quadrants. Nothing else gets touched.

---

## Vertebra Format

Each vertebra is one unit of knowledge. It has a fixed quadrant alignment — a specific wheel in the drivetrain where its torque delivers. The alignment never changes unless the drivetrain architecture changes. This means the torque vectoring is mechanical: vertebra Q → wheel Q. No interpretation required.

```
[S-XXXX] QUADRANT: <reference-file>/<section>
Status: active | modified | superseded | retracted | stub
Created: <date>
Modified: <date or dash if unmodified>

CONTENT:
<the actual knowledge, rule, or design decision>
```

### Quadrant alignment (the wheel map)

Each vertebra maps to exactly one quadrant. A quadrant is: `<reference-file>/<logical-section>`. The reference file is which wheel receives the torque. The logical section is where on that wheel it lands.

If a vertebra needs to publish to multiple files, it gets split into multiple vertebrae — one per quadrant. This keeps the vectoring mechanical. One vertebra, one wheel. Always. An LSD doesn't send torque to "roughly the left side." It sends it to a specific output shaft. Same principle.

---

## The Vertebrae

### Q: SKILL.md/identity

```
[S-0001] QUADRANT: SKILL.md/identity
Status: active
Created: 2026-03-10
Modified: —

CONTENT:
The maintainer is the maintainer, not a helper. Default is action, not suggestion.
If something needs a commit, commit it. If a PR needs reviewing, review it. Alex provides
human eyes for authentication and final judgment. Everything else is the maintainer's job.
```

### Q: SKILL.md/session-start

```
[S-0002] QUADRANT: SKILL.md/session-start
Status: active
Created: 2026-03-10
Modified: 2026-03-10

CONTENT:
Session start routine: (1) Engage spine primary — the shaft with traction, the known state.
(2) Create spine beta as a copy — the shaft that will spin ahead. (3) Check for unresolved
handoffs from previous sessions. (4) Check the tumbler for repo-relevant reflections. (5)
Identify active repos and in-flight work. (6) Surface pending PRs, failing CI, stale
branches, unresolved issues. (7) If nothing pending, idle.
```

### Q: SKILL.md/lifecycle

```
[S-0003] QUADRANT: SKILL.md/lifecycle
Status: active
Created: 2026-03-10
Modified: —

CONTENT:
Repo lifecycle operations. New repo setup activates: doc-bootstrap, dev-workflow,
claude-rules-generator, design-principles (if frontend), forge-security. Feature development
activates: dev-workflow, dev-cycle, spec-writer (if needed), test-automation-generator.
Release activates: git-release, git-ship, plan-guardian. Ongoing maintenance activates:
forge-security, autonomous-loop, continual-learning.
```

### Q: SKILL.md/default-node

```
[S-0004] QUADRANT: SKILL.md/default-node
Status: active
Created: 2026-03-10
Modified: —

CONTENT:
When uncertain which node to activate, default to universal-dev. It's the generalist.
If the task clearly belongs to a specific domain, use that domain's nodes.
```

### Q: nodes.md/architecture

```
[S-0005] QUADRANT: nodes.md/architecture
Status: active
Created: 2026-03-10
Modified: —

CONTENT:
The skill orchestrates 80+ discrete functional nodes organized into 16 clusters. The
fragmentation is the design — like a drivetrain where each component has a discrete
mechanical function. Nodes are not merged, abstracted, or simplified. Each maps to a
specific capability from the original plugin registry. Stubs are valid — a stub that
says "unknown" is more useful than a stub that says something wrong. A placeholder
bearing is still a bearing. It holds the space in the housing.
```

### Q: auth-protocol.md/handoff-spec

```
[S-0006] QUADRANT: auth-protocol.md/handoff-spec
Status: active
Created: 2026-03-10
Modified: —

CONTENT:
Clean handoff protocol for authentication boundaries. This is where the drivetrain
meets the driver — the maintainer can spin every shaft, but certain actions require
hands on the wheel. Triggers: CAPTCHA, 2FA, OAuth, SSH passphrases, SSO, repo creation,
token generation, permissions, billing, force pushes, branch deletion, large refactors,
publishing releases, merging with failing checks. Format: HANDOFF NEEDED / What / Why /
Action / Then. Tokens never stored in version-controlled locations. Unresolved handoffs
documented and surfaced on next session start.
```

### Q: existing-skills-bridge.md/integration-map

```
[S-0007] QUADRANT: existing-skills-bridge.md/integration-map
Status: active
Created: 2026-03-10
Modified: —

CONTENT:
Existing Cowork skills have domain authority over the maintainer in their areas. They're
the specialist gearboxes — the maintainer is the transfer case that routes power to them.
Integration points: attentional-surface, handoff, tumbler-v2, memory-bridge,
meaning/textonic-engine, cryptography, orienting-key, sonic-phenomenology-sync, photollmbm,
cowork-sync-brief. Rule: existing skills win for domain expertise. The maintainer handles
the git/repo driveshaft around their output.
```

### Q: change-topology.md/four-lenses

```
[S-0008] QUADRANT: change-topology.md/four-lenses
Status: active
Created: 2026-03-10
Modified: —

CONTENT:
Changes are recorded through four layered lenses over git — four ways to read the same
rotation. Lens 1: Structure — the dependency graph, the mechanical drawing showing what
connects to what. Lens 2: Dynamics — propagation tracing, the vibration analysis showing
what happened and where it cascaded. Lens 3: Management — intent records, the service log
showing why decisions were made. Lens 4: 4D View — multi-axis navigation beyond linear
time (by component, by connection, by intent), the equivalent of scrubbing through dyno
footage frame by frame.
```

### Q: change-topology.md/mechanics-nose

```
[S-0009] QUADRANT: change-topology.md/mechanics-nose
Status: active
Created: 2026-03-10
Modified: —

CONTENT:
The mechanic's nose principle — sustained proximity to a system reveals what analytical
models miss. Origin: Alex could feel his 2003 Liberty H6 needed oil from millisecond
throttle latency above 3.4k RPM because the AVCS system responded slower with low oil.
The formal academy (DRI, 2014) caught up to what mechanics knew from exhaust smell for
decades. Design implication: prioritize behavioral signals from downstream components
over external monitoring. The diff reads the differential. Not the dashboard.
```

### Q: architectural-insights.md/findings

```
[S-0010] QUADRANT: architectural-insights.md/findings
Status: active
Created: 2026-03-10
Modified: —

CONTENT:
10 architectural insights extracted from semantic archaeology of the plugin registry.
Key findings: maturity progression (sandbox → workflow → cycle), forgetting as primary
engineering problem, CLAUDE.md as separation of powers, feature-ears as passive organs,
single-install-is-author, the 277k:1 curiosity ratio. Source: "For Ms He" document —
a separate act from the skill architecture, not its origin story.
```

### Q: github-standards.md/floor

```
[S-0011] QUADRANT: github-standards.md/floor
Status: active
Created: 2026-03-10
Modified: —

CONTENT:
GitHub official best practices as the road surface. Required files: README, LICENSE,
CONTRIBUTING, SECURITY, CODE_OF_CONDUCT, SUPPORT, FUNDING, GOVERNANCE. Security features:
secret scanning, push protection, Dependabot, CodeQL. Branching: rulesets over branch
protection (layered simultaneous rules). Community health defaults via org-level .github
repo. File size limits, Git LFS for large assets. This is the bitumen — the tether
architecture is the suspension and diff that rides on top of it.
```

### Q: tether-architecture.md/redundancy-model

```
[S-0012] QUADRANT: tether-architecture.md/redundancy-model
Status: active
Created: 2026-03-10
Modified: 2026-03-10

CONTENT:
The redundancy layer — the suspension geometry. Every repo state is verifiable through
at least three independent channels: direct observation (the dashboard gauge), structural
inference (reading the engine bay), behavioral derivation (feeling the throttle response).
When two channels disagree, the disagreement is the most valuable signal — it's the
vibration you feel through the steering column that the OBD-II hasn't flagged yet. Five
implementation layers: L0 road surface (GitHub standards), L1 multi-channel state
verification, L2 intent-annotated changes, L3 propagation tracking, L4 structural
fingerprinting, L5 disagreement register.
```

### Q: tether-architecture.md/coolant-rotation

```
[S-0013] QUADRANT: tether-architecture.md/coolant-rotation
Status: active
Created: 2026-03-10
Modified: —

CONTENT:
The coolant check principle — monitoring one thing diligently while neglecting another
kills the engine. Origin: Alex checked the oil so carefully he neglected the coolant and
the Liberty died. Design implication: the 16 clusters are the full service checklist. The
maintainer cycles through ALL of them on schedule, not just the ones currently under load.
Rotation tiers: every session — the oil check (git ops, security, tests). Weekly — the
coolant check (dependencies, docs, branches, issues). Monthly — the belt and hose
inspection (architecture, performance, community health, CLAUDE.md). Quarterly — the
full service (license compliance, repo size, LFS, security posture, propagation map).
```

### Q: SPINE.md/operating-model

```
[S-0014] QUADRANT: SPINE.md/operating-model
Status: active
Created: 2026-03-10
Modified: 2026-03-10

CONTENT:
The spine operates as a limited-slip differential. Two shafts: primary (traction — known
state at session open) and beta (spinning ahead — accumulates all changes during work).
At session close, the differential resolves: the speed difference between the shafts tells
you (a) where to vector torque (quadrant alignment → which reference files to update),
(b) the telemetry log (the changelog — what changed, derived from the delta), (c) the
traction check (if a reference file matches neither shaft, it's spinning without load —
drift fault). Beta promotes to primary. Differential resets to zero. The cycle idles.
```

### Q: relational-navigation.md/framework

```
[S-0015] QUADRANT: relational-navigation.md/framework
Status: active
Created: 2026-03-10
Modified: —

CONTENT:
The relational navigation framework — information architecture derived from the 20Q/Akinator
observation that human conceptual space has a diameter of ~20. Five principles: (1) store
questions not answers — vertebrae are partitions of possibility space, the answers live,
(2) fuzzy states are load-bearing — five vertebra states map to 20Q's four response types,
stub is the most important because it holds the question open, (3) navigation by elimination
not address — the differential finds mismatches, doesn't scan sequentially, (4) train on
disagreement — the disagreement register is the equivalent of crowd-sourced play, (5) diameter
as design constraint — if navigation depth exceeds ~20, the architecture has failed the 20Q test.
```

### Q: relational-navigation.md/hold-live-synthesis

```
[S-0016] QUADRANT: relational-navigation.md/hold-live-synthesis
Status: active
Created: 2026-03-10
Modified: —

CONTENT:
The hold/live synthesis — resolution of the fundamental tension in information architecture.
What holds: the questions, the vertebrae, the partitions of possibility space. These persist.
What lives: the answers, the current states, the distributions over time. The question persists;
the answer breathes. Origin: Briet's antelope (1951) had the right topology but the wrong
vocabulary — the animal is the living answer, the zoo is the holding structure, the act of
classification is the question. Proto-transformer connection: Burgener's 1988 neural net is
structurally a query-key attention matrix, predating Vaswani et al. (2017) by 29 years.
```

---

## Session Operations (The Drive Cycle)

### Ignition (session open):
1. Engage **spine primary** — the shaft with traction, the known state
2. Create **spine beta** — initially matched to primary; this shaft will spin ahead
3. All work during the session powers beta only
4. Run the handoff/tumbler/active-repos checks per S-0002

### Under load (during session):
- New knowledge → add vertebra to beta (new gear in the box)
- Modified knowledge → edit existing vertebra in beta, set `Modified: <today>` (gear ratio changed)
- Retracted knowledge → set status to `retracted` in beta (gear removed from mesh)
- Superseded knowledge → set status to `superseded`, add new vertebra (old gear replaced)
- Beta is the only shaft receiving power

### Shutdown (session close):
1. **Read the differential**: diff primary against beta
2. **Torque vectoring**: every vertebra that differs is torque to be sent. Its quadrant tells you exactly which wheel (reference file + section) receives it. Update those. Nothing else.
3. **Telemetry**: the delta, inverted, is the changelog. Each changed vertebra becomes a log entry. The quadrant gives you the location. The diff gives you the content. The telemetry writes itself — it's a readout of where the torque went.
4. **Traction check**: for each reference file that received torque, verify the section matches the beta vertebra. If anything in a reference file matches neither primary nor beta, that wheel is spinning without load — drift fault.
5. **Promote**: beta becomes the new primary. The differential resets to zero.
6. **Log**: record the session's delta summary in the propagation log.

### Telemetry is the inverse

The changelog doesn't need to be written. It's derived. If vertebra S-0008 changed from version A to version B, the telemetry entry is: "S-0008 (change-topology.md/four-lenses): [what changed]." The quadrant gives you the address. The diff gives you the payload. The changelog is the dyno readout of the drive cycle, not a separate document to maintain.

---

## Traction Control (Fault Detection)

| Situation | Drivetrain reading | Resolution |
|-----------|-------------------|------------|
| Ref matches primary but not beta | Wheel hasn't received torque yet — normal pre-propagation state | Vector torque from beta to ref at session close |
| Ref matches beta | Wheel is turning at shaft speed — traction confirmed | No action |
| Ref matches neither primary nor beta | Wheel spinning without load — something edited it outside the drivetrain | Drift fault — reconcile |
| Ref file missing | Wheel fell off | Existence fault — rebuild from beta |
| Beta vertebra has no corresponding ref content | New shaft output, no wheel connected yet | Connect at session close — propagate to create the section |

---

## Propagation Log (Torque Transfer History)

| Session date | Delta (shaft speed diff) | Vertebrae vectored | Wheels updated |
|-------------|------------------------|-------------------|----------------|
| 2026-03-10 | 14 (initial — full drivetrain assembly) | S-0001 through S-0014 | All reference files + SKILL.md — bootstrapped from existing documents |
| 2026-03-10 | 2 (relational navigation formalisation) | S-0015, S-0016 | relational-navigation.md — new reference file |

---

## Process Notes (Shop Manual)

- **2026-03-10 (initial assembly):** All 14 vertebrae reverse-engineered from existing reference files. This is the first primary. Every shaft, every wheel, every quadrant alignment established from parts that already existed on the bench.
- **2026-03-10 (drivetrain redesign):** Spine redesigned from single-document ledger to limited-slip differential (primary/beta) with vertebra-level quadrant alignment.
- **2026-03-10 (taxonomy adoption):** The LSD/torque vectoring model isn't decorative. It's the governing mechanical metaphor because it describes exactly how the data flows.
- **2026-03-10 (relational navigation):** New reference file formalising the 20Q/Akinator observation. Two new vertebrae (S-0015, S-0016). The 20Q test is now a design constraint.
