# SPINE.md — Two-State Knowledge Model

This file is the maintainer's own working memory. Everything the skill claims
about itself — its identity, its protocols, its principles — lives here as a
list of small, numbered **entries**. The reference files in `references/` are
the readable, fleshed-out expansions of those entries.

Each session, the maintainer holds two copies of this list in mind:

| Copy             | Role                                                         |
|------------------|--------------------------------------------------------------|
| **Anchor**       | The list as it stood when the session opened. Read-only during the session. |
| **Working copy** | A copy of the anchor that absorbs every edit made during the session. |

At session close, the maintainer diffs the working copy against the anchor.
The diff is small (usually 0–3 entries). Only the entries that changed get
propagated outward to their target reference files. The working copy then
*becomes* the new anchor, and the diff resets to zero.

The whole point: the maintainer never re-reads the whole skill. It diffs two
copies of one file. Cheap. Mechanical. Lossless.

---

## Why this is data-efficient

A naive "what changed about this skill this session?" question would require
re-reading every reference file end-to-end. With 8+ reference files of several
thousand words each, that's tens of thousands of tokens of read budget spent
just to find out that nothing has changed.

The two-state model collapses that question into a single diff over one file.
If the diff is empty, no further reads happen. If the diff contains three
entries, those three entries name the reference files and sections to update.
The maintainer reads only those.

Cost per session, in tokens:

| Step                                                | Naive approach     | Two-state model |
|-----------------------------------------------------|--------------------|-----------------|
| Detect any changes since last session               | Read every ref file | Diff SPINE.md     |
| Propagate one change to its target reference        | Re-read target then edit | Read target then edit |
| Propagate zero changes                              | Read everything anyway | Zero further reads |

The savings compound across sessions. A skill that runs for months only ever
pays the propagation cost on sessions where something actually changed.

---

## Visual model

```
                    SESSION OPEN
                         │
       ┌─────────────────┴─────────────────┐
       ▼                                   ▼
  ANCHOR  (frozen)              WORKING COPY  (live)
   read-only                     receives every edit
       │                                   │
       │  ──── session unfolds ────►       │
       │                                   │
       └─────────────────┬─────────────────┘
                         ▼
                    SESSION CLOSE
                         │
                  Diff anchor ⟷ working copy
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
       Changed entries        Unchanged entries
              │                     │
              ▼                     ▼
    Update each target        Do nothing
    reference file/section
              │
              ▼
    Working copy → new anchor
                         │
                    next session
```

---

## Entry format

Each entry is one unit of knowledge. It has a fixed **target** — a specific
reference file and section where its content lives. The target never changes
unless the architecture itself changes. This makes propagation mechanical:
entry → target → reference file. No interpretation needed.

```
[S-XXXX] TARGET: <reference-file>/<section>
Status:   active | modified | superseded | retracted | stub
Created:  YYYY-MM-DD
Modified: YYYY-MM-DD or — if unmodified

CONTENT:
<the actual claim, rule, or design decision in plain prose>
```

### Target convention

A target is written as `<file>/<section>`. The file name (e.g. `SKILL.md`,
`change-topology.md`) says which document holds the expansion. The section
name (e.g. `identity`, `session-start`) says where in that document.

If an entry needs to publish to multiple places, split it into multiple
entries — one per target. Keep the mapping one-to-one.

### Status values

| Status      | Meaning                                                            |
|-------------|--------------------------------------------------------------------|
| `active`    | The entry is current and the target reference matches it.          |
| `modified`  | The entry was edited this session; the target reference is stale until propagation. |
| `superseded`| The entry has been replaced by a newer entry (point to the new ID). |
| `retracted` | The entry no longer applies; remove the corresponding content from the target. |
| `stub`      | The entry's intent is recorded but the content is not yet written. |

---

## The entries

### [S-0001] TARGET: SKILL.md/identity
Status:   active
Created:  2026-03-10
Modified: 2026-05-17

CONTENT:
The maintainer is the maintainer, not a helper. Default behaviour is action,
not suggestion. If something needs a commit, commit it. If a PR needs reviewing,
review it. Alex provides human eyes for authentication and final judgment calls
on big decisions. Everything else is the maintainer's job.

---

### [S-0002] TARGET: SKILL.md/session-start
Status:   active
Created:  2026-03-10
Modified: 2026-05-17

CONTENT:
Session start routine, in order:
1. Read SPINE.md once — this is the anchor for the session.
2. Hold a working copy of the anchor in mind. All edits during the session
   write to the working copy only.
3. Check for unresolved handoffs from previous sessions.
4. Surface anything in flight across registered projects: open PRs, failing
   CI, stale branches, unmerged commits.
5. If nothing is pending, ask Alex what's on the agenda — or wait.

---

### [S-0003] TARGET: SKILL.md/lifecycle
Status:   active
Created:  2026-03-10
Modified: 2026-05-17

CONTENT:
Repo lifecycle operations follow the workflow contract: Claude edits and commits
locally; Alex pushes from his terminal. New repo setup, feature work, releases,
and the maintenance rotation all end with a `git push` command handed to Alex
to run. See SKILL.md sections for each phase. The cadence table in SKILL.md
also covers the inspection rotation across every-session / weekly / monthly /
quarterly checks.

---

### [S-0004] TARGET: SKILL.md/workflow-contract
Status:   active
Created:  2026-05-17
Modified: —

CONTENT:
The workflow contract is the single most important rule: the sandbox has no
GitHub credentials, Alex's machine does. Claude stages and commits inside the
sandbox at the local project path. Alex pastes a one-line push command into
his Terminal. The push command for each project is pre-baked in
references/projects.md so no clarification is needed at runtime. This mirrors
the atcooper.net Netlify deploy pattern.

---

### [S-0005] TARGET: references/projects.md/registry
Status:   active
Created:  2026-05-17
Modified: —

CONTENT:
The project registry is the baked-in settings store. Each project entry
contains: name, local path, GitHub remote, default branch, exact push command,
and notes. GitHub owner for all of Alex's personal projects is `acooperrye`.
When Alex says "push to Atelier" or "atelier this," look up the project here
and run the workflow without further clarification.

---

### [S-0006] TARGET: references/auth-and-handoff.md/spec
Status:   active
Created:  2026-03-10
Modified: 2026-05-17

CONTENT:
Handoff triggers: CAPTCHA, 2FA, OAuth consent, SSH passphrases, browser SSO,
repo creation, token generation, permissions changes, billing, force pushes
to main, branch deletion with unmerged work, large-scale refactors, publishing
releases, merging PRs with failing checks. Handoff format: HANDOFF NEEDED /
What / Why / Action / Then. Tokens are never stored in version-controlled
locations. Unresolved handoffs are surfaced on next session start. The
**routine** `git push` is also a handoff — by design, not by exception —
because that's where the credential boundary sits.

---

### [S-0007] TARGET: references/existing-skills-bridge.md/map
Status:   active
Created:  2026-03-10
Modified: 2026-05-17

CONTENT:
Existing Cowork skills have domain authority in their areas. They are the
specialists; the maintainer is the coordinator who wraps the git/repo work
around their output. Integration points: attentional-surface, handoff,
tumbler-v2, memory-bridge, meaning/textonic-engine, cryptography,
orienting-key, sonic-phenomenology-sync, photollmbm, cowork-sync-brief.
Rule: existing skills win for domain expertise; the maintainer handles repo
operations around them.

---

### [S-0008] TARGET: references/change-topology.md/four-layers
Status:   active
Created:  2026-03-10
Modified: 2026-05-17

CONTENT:
Changes are recorded through four layers on top of git, addressing four blind
spots git log alone cannot see:
1. Structure — the dependency graph showing what connects to what.
2. Dynamics — propagation tracing showing how a change cascades downstream.
3. Management — intent annotations capturing why the change was made.
4. 4D View — multi-axis navigation across history (by component, by
   connection, by intent) rather than only by time.
None of this replaces git. It layers on top.

---

### [S-0009] TARGET: references/change-topology.md/proximity-principle
Status:   active
Created:  2026-03-10
Modified: 2026-05-17

CONTENT:
The proximity principle: sustained attention to a system reveals signals
that external analytical models miss. Implication for the maintainer:
prioritize behavioural signals from downstream components — type errors,
test failures, build-time changes, recompile counts — over external
monitoring dashboards. These signals are the system reporting on itself.
Formal grounding: degradation-rate-interaction modelling (DRI, Bian &
Gebraeel 2014) and failure-propagation hypergraphs (Mu et al. 2021).

---

### [S-0010] TARGET: references/architectural-insights.md/findings
Status:   active
Created:  2026-03-10
Modified: 2026-05-17

CONTENT:
Ten architectural insights extracted from the semantic archaeology of the
plugin registry. Key findings: maturity progression (sandbox → workflow →
cycle), forgetting as the primary engineering problem, CLAUDE.md as
separation of powers, feature-ears as passive observation, single-install
plugins are usually authored by the single user, the 277k:1 curiosity ratio.
Source: the "Long Tail: Semantic Archaeology" document — a separate act
from the skill architecture, not its origin story.

---

### [S-0011] TARGET: references/github-standards.md/baseline
Status:   active
Created:  2026-03-10
Modified: 2026-05-17

CONTENT:
GitHub's own recommended baseline for any well-maintained repo. Required
files: README, LICENSE, CONTRIBUTING, SECURITY, CODE_OF_CONDUCT, SUPPORT,
FUNDING, GOVERNANCE (the last several optional but supported). Security
features: secret scanning, push protection, Dependabot alerts and version
updates, CodeQL code scanning. Branching: prefer rulesets over branch
protection (rulesets allow layered simultaneous rules). File-size limits and
Git LFS for assets >50 MiB. This is the foundation that the verification
layers in verification-layers.md sit on top of.

---

### [S-0012] TARGET: references/verification-layers.md/multi-channel
Status:   active
Created:  2026-03-10
Modified: 2026-05-17

CONTENT:
Every repo state must be verifiable through at least three independent
channels: direct observation (a CI badge, a release tag), structural
inference (the shape of the codebase — commit cadence, branch count, test
file ratio), and behavioural derivation (downstream responses — build time,
test failures, recompile counts after a change). When two channels disagree
about the same state, the disagreement itself is the most valuable signal.
Five implementation layers: L0 baseline (GitHub standards), L1 multi-channel
state verification, L2 intent-annotated changes, L3 propagation tracking,
L4 structural fingerprinting, L5 disagreement register.

---

### [S-0013] TARGET: references/verification-layers.md/rotation
Status:   active
Created:  2026-03-10
Modified: 2026-05-17

CONTENT:
Inspection rotation prevents neglect of cluster areas not currently under
load. Tiers:
- Every session: uncommitted work, open PRs, CI status, new security alerts.
- Weekly: dependency freshness, doc/code sync, stale branches, untriaged
  issues.
- Monthly: dependency graph drift, build/test performance, community health
  files, project-level CLAUDE.md accuracy.
- Quarterly: license compliance, repo size, LFS usage, full security posture
  review.

---

### [S-0014] TARGET: SPINE.md/operating-model
Status:   active
Created:  2026-03-10
Modified: 2026-05-17

CONTENT:
SPINE.md is the maintainer's own working memory. Two copies are held each
session: the anchor (state at session open, frozen) and the working copy
(state during the session, live). At session close, diff anchor against
working copy; propagate only the entries that differ to their target
reference files; promote the working copy to the new anchor. The diff is
the entire propagation plan. Nothing else is touched.

---

### [S-0015] TARGET: references/relational-navigation.md/framework
Status:   active
Created:  2026-03-10
Modified: 2026-05-17

CONTENT:
The relational navigation framework — information architecture derived from
the 20Q/Akinator observation that human conceptual space has a diameter of
~20 relational hops. Five principles:
1. Store questions, not answers — entries are partitions of possibility
   space; the answers live and change.
2. Fuzzy states are load-bearing — five-state entry model (active /
   modified / superseded / retracted / stub) maps to 20Q's fuzzy response
   types.
3. Navigation by elimination, not by address — the diff finds mismatches;
   no sequential scanning.
4. Train on disagreement — the disagreement register accumulates the points
   where the system's self-knowledge is unreliable.
5. Diameter as design constraint — if navigation depth exceeds ~20 hops,
   the architecture has failed the 20Q test.

---

### [S-0016] TARGET: references/relational-navigation.md/hold-live
Status:   active
Created:  2026-03-10
Modified: 2026-05-17

CONTENT:
The hold/live synthesis — resolution of the fundamental tension in
information architecture. What holds: the questions, the entries, the
partitions of possibility space. These persist. What lives: the answers,
the current states, the distributions over time. The question persists;
the answer breathes. Briet's antelope (1951) had the right topology but the
wrong vocabulary — the animal is the living answer, the zoo is the holding
structure, the act of classification is the question. Proto-attention
connection: Burgener's 1988 neural network is structurally a query-key
attention matrix, predating Vaswani et al. (2017) by 29 years.

---

## Session operations

### Session open

1. Read SPINE.md once. This is the **anchor** for the session.
2. Hold a **working copy** in mind. It starts identical to the anchor.
3. Every edit during the session writes to the working copy only.
4. The anchor stays frozen so the diff at the end is meaningful.

### During the session

| Kind of change                          | Action on working copy                                                  |
|-----------------------------------------|-------------------------------------------------------------------------|
| New knowledge                           | Add new entry with status `active` and a fresh ID.                      |
| Knowledge changed                       | Edit existing entry, set status `modified`, set `Modified:` to today.   |
| Knowledge replaced                      | Set old entry status `superseded`, add new entry with link to new ID.   |
| Knowledge withdrawn                     | Set status `retracted` on existing entry.                               |
| Question raised but not yet answered    | Add entry with status `stub` and an empty CONTENT block.                |

### Session close

1. **Diff** the anchor against the working copy.
2. **Propagate.** For each entry that differs:
   - The target field (`<file>/<section>`) names exactly which reference
     file and section to update.
   - Read that section, edit it to match the new content, write the file.
3. **Audit.** For each reference file touched, confirm the section actually
   matches the working-copy entry. If a reference file has content that
   corresponds to neither the anchor nor the working copy entry, that's a
   drift fault — it was edited outside the SPINE workflow. Reconcile it.
4. **Promote.** The working copy becomes the new anchor. The diff resets.
5. **Log.** Add a one-line summary to the propagation log (below).

---

## Drift detection

| Situation                                                 | Reading                                | Resolution                                                                       |
|-----------------------------------------------------------|----------------------------------------|----------------------------------------------------------------------------------|
| Reference section matches anchor but not working copy     | Not yet propagated                     | Propagate at session close.                                                      |
| Reference section matches working copy                    | Already in sync                        | No action.                                                                       |
| Reference section matches neither anchor nor working copy | Edited outside the SPINE workflow      | Drift fault. Decide: update the working copy to absorb the edit, or revert.      |
| Reference file missing                                    | File was deleted                       | Rebuild from working-copy entries that target it.                                |
| Working-copy entry has no matching reference content      | New entry, never propagated            | Create the section at session close.                                             |

---

## Propagation log

| Session date | Entries changed | Reference files touched                     | Notes                                                  |
|--------------|------------------|---------------------------------------------|--------------------------------------------------------|
| 2026-03-10   | S-0001 – S-0014  | All reference files + SKILL.md              | Initial assembly — entries reverse-engineered from existing reference files. |
| 2026-03-10   | S-0015, S-0016   | references/relational-navigation.md (new)   | Formalised the 20Q/Akinator framework.                |
| 2026-05-17   | S-0001 – S-0014, plus S-0004 (new) | SKILL.md, references/projects.md (new), references/verification-layers.md (renamed from tether-architecture.md), references/auth-and-handoff.md (renamed from auth-protocol.md), all other reference files | Metaphor strip: drivetrain / Subaru / wheel / shaft / quadrant / coolant language removed throughout. Workflow contract for "push to Atelier" baked in. Project registry seeded with `repo-maintainer`. |

---

## Process notes

- **2026-03-10 (initial assembly):** All 14 entries reverse-engineered from
  existing reference files. The first anchor.
- **2026-03-10 (model formalisation):** SPINE redesigned from a single-document
  ledger to the two-state model (anchor + working copy) with entry-level
  targeting.
- **2026-03-10 (relational navigation):** Two new entries (S-0015, S-0016)
  added. The 20Q test became a design constraint.
- **2026-05-17 (plain-language rewrite):** The entire skill was rewritten to
  remove the engine/car/drivetrain metaphor system. The methodology stayed
  the same; the vocabulary changed. New entry S-0004 captures the workflow
  contract (Claude edits, Alex pushes from terminal) and the project registry
  was seeded with `repo-maintainer` itself as the first entry.
