# Tether Architecture: The Suspension Geometry

The GitHub standards are the road surface. This document describes the suspension and differential that rides on top — how to make every repo state verifiable through multiple independent channels, so that if any single gauge goes dark, the condition of the system can still be read from the chassis.

The design principle: nothing is true because one gauge says so. Everything is true because multiple independent readings agree. When they disagree, that's the vibration you feel through the steering column before the OBD-II throws a code.

---

## Three Ways to Read the System

### 1. The dashboard gauge (direct observation)

Any given state of the repo should be readable from at least two independent instruments. Not "read the same gauge twice" — read via genuinely different channels that would fail independently.

**Example — "Is the build passing?"**
- Gauge A: CI badge on README (GitHub Actions status)
- Gauge B: Last merge to main succeeded (git log + merge policy requires passing checks)
- Gauge C: Deployment is live and responding (if deployed)

If A says passing but B shows a force-merge that bypassed checks, the gauges disagree. The disagreement is more informative than either reading alone. One gauge can lie. Two gauges disagreeing can't both be right — and that's the signal.

### 2. The engine bay (structural inference)

The state should be deducible from the shape of the system even if you can't start it. A stranger who can only open the bonnet and look — no key, no diagnostics port, no test drive — should still be able to infer health.

**Example — "Is this repo maintained?"**
- Belt wear: commit frequency — not just "recent commits exist" but the rhythm (regular vs burst-and-silence)
- Coolant colour: issue response time (open issues with no response = neglected system)
- Hose condition: dependency age (last Dependabot PR merged 14 months ago = perished rubber)
- Wiring tidiness: branch count (47 unmerged branches = nobody's routing the loom)
- Service sticker: PR merge pattern (all squash-merged by one person = single mechanic, bus factor 1)

None of these require turning the key. They're structural reads. The repo's shape tells you its health the way a mechanic reads an engine bay before anything's running.

### 3. The test drive (behavioral derivation)

When something changes, the downstream response confirms or denies the system's integrity. This is the AVCS throttle principle — the Subaru's millisecond lag above 3.4k telling you the oil is low before the dipstick does.

**Example — "Did that dependency update break anything?"**
- Dashboard read: CI runs and tests pass/fail
- Exhaust note: Build time changed (a new dependency that adds 40s to the build is a signal even if tests pass — the engine's working harder)
- Transmission feel: Which downstream modules recompiled? If a "minor" update caused 30 files to recompile, the coupling is tighter than the semver implied — that's a gearbox binding
- Absence of expected sound: Dependabot opened the PR but CI *didn't run* — the ignition sequence doesn't trigger on bot PRs, which is itself a finding

---

## The Gauge Cluster: What Gets Multi-Channel Coverage

### System Health

| State | Gauge 1 (Dashboard) | Gauge 2 (Engine Bay) | Gauge 3 (Test Drive) |
|-------|---------------------|---------------------|---------------------|
| Build passing | CI status badge | Last N merges all required passing checks | Deploy is live and healthy |
| Tests adequate | Coverage report | Test file count proportional to source file count | New PRs rarely break things that tests should have caught |
| Dependencies current | Dependabot alerts = 0 | `package-lock.json` / `yarn.lock` recently modified | No CVEs in `npm audit` / `pip audit` |
| Documentation current | README modified within N commits of significant changes | Doc file count proportional to feature count | New contributors onboard successfully (confused-newcomer issues declining) |
| Security posture | Secret scanning clean | No `.env` files in history (`git log --all --diff-filter=A -- '*.env'`) | Push protection triggered and caught (or hasn't triggered — also a read) |
| Maintenance active | Commit frequency | Issue/PR response time | Stale branch count trending down |

### Change Integrity

Every change (commit, PR, release) should be verifiable through four readings:

**1. The diff** — the change itself. The commit message, the PR description, the files modified. This is the part you can see by lifting the bonnet.

**2. The service log** — why this change was made. Linked to an issue, ticket, discussion, or advisory. Not just "fixes #42" but a structured intent record:
```
INTENT: security-patch
TRIGGER: dependabot-alert-CVE-2026-1234
DECISION: upgrade lodash from 4.17.20 to 4.17.22
ALTERNATIVES-CONSIDERED: pin to 4.17.21 (rejected: incomplete fix), remove lodash entirely (rejected: 14 call sites, too invasive for a patch)
EXPECTED-PROPAGATION: none — lodash is a leaf dependency, no downstream type changes
```

**3. The vibration analysis** — what actually happened downstream. Did the expected propagation match reality? If the service log said "no downstream effects" but CI flagged 3 type errors, the log and the road feel disagree. The disagreement is the diagnostic.

**4. The mechanical drawing** — a snapshot of the dependency graph before and after the change. Which connections were added, removed, or modified. This is the system map from `change-topology.md`, versioned. You're comparing two blueprints to see what moved.

### Branch State

| State | Gauge 1 | Gauge 2 | Gauge 3 |
|-------|---------|---------|---------|
| Main is stable | CI passing on HEAD | Last N releases cut from main successfully | No hotfix branches active |
| Feature branch safe to merge | PR checks passing | No merge conflicts with main | Branch not stale (< N days since last push) |
| Branch is abandoned | No commits in > 30 days | PR (if any) has no recent activity | Author hasn't pushed anywhere in > 30 days |
| Release is ready | All milestone issues closed | Changelog covers all merged PRs since last release | Staging deploy (if applicable) is stable |

---

## The Service Schedule (Coolant Check Rotation)

Named after the 2003 Subaru Liberty H6 that died because the oil got all the attention and the coolant got none.

The maintainer runs a full service rotation across all 16 clusters, not just the ones currently under load. The rotation ensures neglected systems surface before they seize.

### Rotation tiers:

**Every session — the oil check:**
- Git operations: any uncommitted work, unmerged PRs, failing CI?
- Security: any new Dependabot alerts, secret scanning findings?
- Testing: are tests still passing? Any new flaky tests?

**Weekly — the coolant check:**
- Dependencies: anything outdated beyond N versions?
- Documentation: does the README still match reality?
- Branches: any stale branches to prune?
- Issues: any open issues with no response?

**Monthly — the belt and hose inspection:**
- Architecture: has the dependency graph drifted? Any new circular dependencies?
- Performance: build times trending up? Test suite getting slower?
- Community health files: CONTRIBUTING, SECURITY, CODE_OF_CONDUCT still accurate?
- CLAUDE.md: does it still reflect the project's actual state and conventions?

**Quarterly — the full service:**
- License compliance: any new dependencies with incompatible licenses?
- Repository size: trending toward the 1 GB recommendation?
- LFS usage: any large files that should be in LFS but aren't?
- Security posture review: CodeQL findings, dependency review, push protection audit
- Full propagation map review: which components are highest-risk for change cascade?

### The rotation doesn't wait to be asked.

The maintainer surfaces findings proactively. "The test suite has gotten 40% slower over the last month — the three PRs that added the most time were X, Y, Z. Want me to investigate?" Not waiting for Alex to notice the temperature gauge creeping up. The whole point is that Alex shouldn't have to remember to check the coolant.

---

## Gauge Disagreement as Diagnostic

The most important principle in this architecture: **when two gauges disagree about the same state, the disagreement itself is the most valuable reading.**

- CI says passing but deploy is broken → the dyno and the road test give different results. The test environment has diverged from production.
- README says "install with npm" but package.json has no install script → the user manual doesn't match the wiring diagram.
- Commit message says "minor refactor" but 30 files changed → either the service log is wrong or the coupling is wrong. Something's binding.
- Dependabot says no vulnerabilities but `npm audit` finds 3 → one scanner is filtering. Which one, and why?
- Intent record says "no downstream effects" but 4 tests broke → the mechanical drawing has connections the blueprint doesn't show.
- Branch protection says "require reviews" but last 5 merges have no reviewers → the safety system is being bypassed. Routine admin override is a worn-out interlock.

Each disagreement is a diagnostic. The maintainer doesn't resolve them silently — it surfaces them with both gauges named, so the resolution addresses the root (why do these gauges disagree?) not just the symptom (which one should I believe?).

---

## Implementation Layers (The Suspension Stack)

### Layer 0: The road surface
Everything in `github-standards.md`. README, LICENSE, CONTRIBUTING, SECURITY, .gitignore, CI, branch protection, Dependabot, secret scanning, push protection. This is the bitumen. Smooth, sealed, load-bearing. The suspension assumes it's there.

### Layer 1: Multi-gauge state verification
For each state in the gauge cluster above, configure or build the channels. Some are free (CI badge, git log queries — gauges that come stock). Some require light tooling (a script that compares README claims to actual project structure — aftermarket gauge). Some require the maintainer to do periodic manual inspection (the service rotation — hands-on, bonnet up).

### Layer 2: Intent-annotated changes (the service log)
Every PR gets a structured intent block in the description. The maintainer writes these, not Alex (unless Alex wants to). Over time this builds a queryable service history — why every change was made, what was considered, what was expected to happen downstream.

### Layer 3: Propagation tracking (vibration analysis)
On CI failure, the maintainer traces the causal chain from the failing test back through the dependency graph to the originating change. This gets stored (in PR comments, in a dedicated log, or in a structured file) so it's queryable later. "Show me every time a change to auth.ts caused a downstream failure" becomes answerable. You're building the vibration map of the system — where does force travel when something moves?

### Layer 4: Structural fingerprinting (the mechanical drawing)
The dependency graph gets snapshotted periodically (or on significant changes). Diffs between snapshots show how the system's topology is evolving. New connections, removed connections, new circular dependencies, increasing coupling — all visible without reading a single line of code. You're comparing blueprints across time.

### Layer 5: The disagreement register (the anomaly log)
A running log of every time two gauges disagreed about the same state. Each entry records: what was being checked, what Gauge A said, what Gauge B said, how it was resolved, and what systemic fix (if any) was applied to prevent recurrence. This is the maintainer's institutional memory of where the system has been unreliable about self-reporting. Every car has its quirks. This is where you write them down.

---

## What This Costs (Honest Overhead)

- Intent annotations add ~2 minutes per PR. Worth it for the service history.
- Propagation tracing on failures adds time to incident response but pays back in faster root cause on future failures. The vibration map gets more detailed with every incident.
- Service rotation adds ~10 minutes per session for the oil check, more for deeper inspections. Worth it for catching neglect before it becomes the thing that kills the engine.
- Structural fingerprinting is automatable once set up. Near-zero ongoing cost — the dyno runs itself.
- The disagreement register is maintained by the maintainer, not Alex. Zero cost to Alex.

The entire suspension geometry is designed to cost Alex nothing. The maintainer does the work. Alex gets a system that can explain its own health through multiple independent gauges, and that surfaces problems before they become the kind of problem that kills a Subaru.

---

## Sources

Design principles drawn from:
- Change topology research (see `change-topology.md`) — the formal frameworks
- Architectural insights from the semantic archaeology (see `architectural-insights.md`) — the plugin patterns
- GitHub official standards (see `github-standards.md`) — the road surface
- The Subaru H6 AVCS throttle latency principle: sustained proximity > analytical models. The mechanic's nose reads what the OBD-II misses.
- The Subaru coolant principle: the service schedule exists because diligent attention to one system while neglecting another is how engines die
- The limited-slip differential model: the spine's dual-shaft architecture, where the speed difference between primary and beta IS the information, and torque vectors to exactly the wheels that need it
