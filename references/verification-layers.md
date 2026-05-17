# Verification Layers

The GitHub standards in `github-standards.md` are the foundation. This document
describes the layers that sit on top — how to make every repo state verifiable
through multiple independent channels, so that no single signal is the only
thing keeping the system honest.

The design principle: **nothing is true because one channel says so. Everything
is true because multiple independent channels agree. When they disagree, the
disagreement itself is the most valuable signal.**

---

## Three independent channels for any state

For any state of the repo you care about, the maintainer should be able to read
it through at least two — preferably three — channels that fail independently
of each other.

### Channel 1: Direct observation

A reading you can take from an existing instrument GitHub already provides.

| Question                       | Direct observation                              |
|--------------------------------|-------------------------------------------------|
| Is the build passing?          | CI badge on the README, GitHub Actions status   |
| Are tests adequate?            | Coverage report                                 |
| Are dependencies current?      | Dependabot alerts panel                         |
| Are there secrets exposed?     | Secret scanning panel                           |
| Is the repo maintained?        | Commit frequency on the repo's overview page    |

### Channel 2: Structural inference

A reading you can take by looking at the shape of the repo, without running
anything. The structure itself reports on its health.

| Question                       | Structural inference                                                |
|--------------------------------|---------------------------------------------------------------------|
| Is the build passing?          | Last N merges all required passing checks (read from merge policy)  |
| Are tests adequate?            | Test file count proportional to source file count                   |
| Are dependencies current?      | Lockfile modification date, last Dependabot PR merge date           |
| Are docs current?              | README modification date relative to significant code changes       |
| Is the repo maintained?        | Open issue response time, stale branch count, single-author ratio   |

### Channel 3: Behavioural derivation

A reading taken from how the system responds to a change. When something
upstream shifts, downstream components react. Those reactions are signal.

| Question                       | Behavioural derivation                                                |
|--------------------------------|------------------------------------------------------------------------|
| Is the build passing?          | Deploy completes and the live site responds                            |
| Are tests adequate?            | New PRs rarely break things tests should have caught                   |
| Did a dependency update break anything? | Build time changed, downstream modules recompiled, new flakes appeared |
| Is documentation current?      | New contributors onboard successfully (declining confused-newcomer issues) |
| Is the security posture sound? | Push protection has triggered when expected (or hasn't triggered when not expected — both are signal) |

---

## Multi-channel coverage table

Each row is a state the maintainer cares about. The columns are the three
channels. The maintainer aims for a non-empty cell in at least two columns
for every row.

### System health

| State                | Channel 1 (Direct)                  | Channel 2 (Structural)                       | Channel 3 (Behavioural)                                  |
|----------------------|-------------------------------------|----------------------------------------------|----------------------------------------------------------|
| Build passing        | CI badge                            | Last N merges all required passing checks    | Deploy is live and healthy                               |
| Tests adequate       | Coverage report                     | Test/source file count ratio                 | PRs rarely break things tests should catch               |
| Dependencies current | Dependabot alerts = 0               | Lockfile recently modified                   | `npm audit` / `pip audit` returns nothing                |
| Docs current         | README modified near significant changes | Doc/feature count ratio                      | New contributors onboard without confused issues          |
| Security posture     | Secret scanning clean               | No `.env` in history (`git log --all --diff-filter=A -- '*.env'`) | Push protection has caught (or correctly not caught) credentials |
| Maintenance active   | Commit frequency                    | Issue/PR response time                       | Stale branch count trending down                         |

### Change integrity

Every change (commit, PR, release) should be verifiable through four readings:

1. **The diff** — the change itself. Commit message, PR description, files
   modified. Available from `git show`.

2. **The intent record** — *why* this change was made. Linked to an issue,
   ticket, advisory, or discussion. Not just "fixes #42" but a structured block:

   ```
   INTENT: security-patch
   TRIGGER: dependabot-alert-CVE-2026-1234
   DECISION: upgrade lodash from 4.17.20 to 4.17.22
   ALTERNATIVES-CONSIDERED:
     - pin to 4.17.21 (rejected: incomplete fix)
     - remove lodash entirely (rejected: 14 call sites, too invasive for a patch)
   EXPECTED-PROPAGATION: none — lodash is a leaf dependency, no downstream type changes
   ```

3. **The propagation reading** — what actually happened downstream after the
   change merged. Did the expected propagation match reality? If the intent
   record said "no downstream effects" but CI flagged three type errors, the
   intent and the behaviour disagree. That disagreement is the diagnostic.

4. **The structural snapshot** — a before/after view of the dependency graph
   around the change. Which edges were added, removed, or modified. This is the
   versioned system map from `change-topology.md`.

### Branch state

| State                          | Channel 1                          | Channel 2                                 | Channel 3                                  |
|--------------------------------|------------------------------------|-------------------------------------------|--------------------------------------------|
| `main` is stable               | CI passing on HEAD                 | Last N releases cut from `main` succeeded | No hotfix branches active                  |
| Feature branch safe to merge   | PR checks passing                  | No conflicts with `main`                  | Branch not stale (<N days since last push) |
| Branch is abandoned            | No commits in >30 days             | PR (if any) has no recent activity        | Author hasn't pushed anywhere in >30 days  |
| Release is ready               | All milestone issues closed        | Changelog covers all merged PRs since last release | Staging deploy stable                  |

---

## The inspection rotation

The maintainer cycles through *every* cluster on a schedule, not only the ones
currently under load. This prevents quiet neglect of areas that don't generate
visible signal.

### Rotation tiers

**Every session — surface-level scan**

- Git operations: uncommitted work, unmerged PRs, failing CI?
- Security: new Dependabot alerts, secret scanning findings?
- Testing: tests still passing? Any new flakes?

**Weekly — deeper scan**

- Dependencies: anything outdated by more than N versions?
- Documentation: does the README still match reality?
- Branches: stale branches to prune?
- Issues: open issues with no response?

**Monthly — architectural scan**

- Architecture: dependency graph drifted? Any new circular dependencies?
- Performance: build times trending up? Test suite getting slower?
- Community health files: CONTRIBUTING, SECURITY, CODE_OF_CONDUCT still
  accurate?
- CLAUDE.md: does it still reflect the project's actual state and conventions?

**Quarterly — full audit**

- License compliance: any new dependencies with incompatible licenses?
- Repository size: trending toward 1 GB?
- LFS usage: any large files that should be in LFS but aren't?
- Security posture review: CodeQL findings, dependency review, push
  protection audit.
- Full propagation map review: which components are highest-risk for change
  cascade?

### Proactive surfacing

The rotation doesn't wait to be asked. The maintainer surfaces findings:

> "The test suite has gotten 40% slower over the last month — the three PRs
> that added the most time were X, Y, Z. Want me to investigate?"

Alex shouldn't have to remember to check. That's the whole point of the
rotation.

---

## Disagreement as diagnostic

The most important principle in this architecture: **when two channels
disagree about the same state, the disagreement is the most valuable signal
in the room.**

| Channel A says…                       | Channel B says…                  | Diagnosis                                              |
|---------------------------------------|----------------------------------|--------------------------------------------------------|
| CI is passing                         | Deploy is broken                 | Test environment has diverged from production         |
| README says "install with npm"        | `package.json` has no install script | User manual doesn't match the code                |
| Commit message says "minor refactor"  | 30 files changed                 | Either the intent record is wrong, or coupling is wrong |
| Dependabot says no vulnerabilities    | `npm audit` finds 3              | One scanner is filtering — which one, and why?         |
| Intent record says "no downstream effects" | 4 tests broke                    | The structural snapshot has edges the intent record didn't list |
| Branch protection says "require reviews" | Last 5 merges had no reviewers   | The control is being bypassed routinely                |

Each disagreement is a diagnostic. The maintainer doesn't resolve them silently
— it surfaces them with both channels named, so the resolution addresses the
root cause (why do these channels disagree?) and not only the symptom (which
one should I believe?).

---

## Implementation layers

The verification system is built up in five layers on top of the GitHub
baseline (L0).

| Layer | Name                                | What it adds                                                                                                          |
|-------|-------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| L0    | Baseline                            | Everything in `github-standards.md` — README, LICENSE, CONTRIBUTING, SECURITY, `.gitignore`, CI, branch protection, Dependabot, secret scanning, push protection. |
| L1    | Multi-channel state verification    | For each state in the coverage table above, configure or build the channels. Some are stock (CI badge, git log queries). Some need light tooling (a script comparing README claims to project structure). Some need periodic manual inspection (the rotation). |
| L2    | Intent-annotated changes            | Every PR gets a structured intent block in the description. The maintainer writes these by default. Over time this builds a queryable history — why every change was made, what was considered, what was expected downstream. |
| L3    | Propagation tracking                | On CI failure, trace the causal chain from the failing test back through the dependency graph to the originating change. Store the trace (PR comments, dedicated log, or structured file) so it's queryable later. |
| L4    | Structural fingerprinting           | Snapshot the dependency graph periodically. Diffs between snapshots show how the system's topology evolves: new connections, removed connections, growing coupling — visible without reading a single line of code. |
| L5    | Disagreement register               | A running log of every time two channels disagreed about the same state. Each entry: what was checked, what channel A said, what channel B said, how it was resolved, what systemic fix (if any) was applied. The maintainer's institutional memory of where self-reporting has been unreliable. |

---

## Honest cost

- Intent annotations add ~2 minutes per PR. Worth it for the queryable history.
- Propagation tracing on failures adds time to incident response but pays back
  on future incidents — each trace makes the next one faster.
- The rotation adds ~10 minutes per session for the surface scan; deeper tiers
  cost more but happen less often.
- Structural fingerprinting is automatable once set up. Near-zero ongoing cost.
- The disagreement register is maintained by the maintainer, not Alex. Zero
  cost to Alex.

The entire system is designed so that Alex pays nothing on a per-change basis.
The maintainer absorbs the overhead. Alex gets a repo that can explain its own
state through multiple independent channels, and that surfaces problems before
they become incidents.

---

## Sources

- Change-topology research — see `change-topology.md` for the formal
  frameworks and citations.
- GitHub official standards — see `github-standards.md` for the L0 baseline.
- Architectural insights — see `architectural-insights.md` for the
  cross-cutting patterns the maintainer inherits from the plugin archaeology.
