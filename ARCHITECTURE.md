#‌‌‍‌‌‍﻿﻿‌﻿‌‌​‍​​‌‍​‌‌﻿​‍‌‍‌‌​‍​​‌‍﻿‍‌‍﻿﻿‌﻿‌​​‍​​‌﻿‌​‌‍‍​‌‍‌‌​‍​​‌‍‌‍‌‍‍‌‌﻿​‍‌﻿​﻿‌﻿‌​​‍​​‌﻿‌‍‌‍‌‌‌﻿​‍‌﻿​﻿‌‍‍‌‌‍﻿﻿‌‍﻿‍​‍​​‌‍﻿﻿‌‍‌‍​‍​​‌﻿‍‌‌‍﻿﻿‌﻿‌‌‌﻿​‍‌﻿​﻿‌‍‌‌‌‍﻿​‌‍‌‍​‍​​‌﻿‌​‌‍﻿﻿​‍​​‌﻿​‍‌‍‌‌‌‍​‌‌‍‌​​‍​​‌﻿‌​‌‍‍​‌‍‍‌‌﻿​﻿​‍﻿‍​‍​​‌‌‌​‌‍‍​‌‍‌‌​‍​​‌‍‌​‌‍‍‌‌‍‌‍‌‍‌‍‌‍‌‌‌﻿​‍‌‍‌‌‌‍﻿‍‌﻿‌​‌‍‍‌‌‍​‌‌‍﻿​​‍​​‌﻿‌﻿‌‍﻿﻿‌﻿​‍‌‍‍﻿‌﻿​﻿​‍﻿‍​‍​​‌‌‌​‌﻿​‍‌﻿‌‌‌﻿​﻿‌﻿‌​​‍​​‌﻿‌​‌‍‍​‌‍‌‌​‍​​‌‍‌​‌‍‌‌‌‍﻿​‌﻿‌​‌‍​‌​‍﻿‍​‍​​‌​​﻿‌‍﻿​‌‍﻿﻿‌﻿​﻿‌‍‌‌​‍​​‌‍‍‌‌﻿‌​​‍​​‌﻿​​‌﻿​‍‌‍﻿﻿‌﻿​​‌‍‌‌‌﻿​‍‌‍﻿​‌﻿‍‌​‍﻿‍​‍​​‌‌‌​‌‍‍​‌‍‌‌​‍​​‌﻿​﻿‌﻿​​‌‍​‌‌‍​﻿‌‍‌‌​‍​​‌‍​‍‌‍‌‌‌﻿‌​‌﻿‌﻿‌‍‌‌‌‍‌‌‌‍﻿‍​‍​​‌﻿​﻿‌‍‌‌‌﻿​﻿‌﻿​﻿‌‍‍‌‌‍﻿﻿‌‍﻿‍‌﻿​﻿​‍​​‌‍‍‌‌﻿​﻿​‍​​‌﻿‌﻿‌‍‍​‌‍‌‌‌﻿​‍‌‍‌‌​‍​​‌﻿‌​‌‍‍​‌‍‌‌​‍​​‌﻿‌﻿‌‍﻿﻿‌﻿​‍‌‍‍﻿​‍​​‌﻿​﻿‌﻿‌‌‌﻿​‍‌﻿‌‍‌‍‍‌‌﻿‌‍‌‍‌‌‌﻿​﻿​‍​​‌‍﻿﻿‌﻿​‍​‍​​‌‍‌​‌‍‍‌‌‍‌‌‌﻿​﻿​‍﻿‍​‍​​‌​​‍‌﻿‌‌‌‍‍‌‌‍﻿​‌‍‌​​‍​​‌‍﻿﻿‌‍﻿‍​‍​​‌﻿‌﻿‌‍‍​‌‍​‌‌﻿‌​​‍​​‌‍‍​‌‍‌‌‌‍﻿​‌‍‌​​‍﻿‍​‍​​﻿‍​‍‍​​​‍‌‌​​‍​​‌​​﻿‌​﻿​‌​​‌‌‌‌‌‌​‌​‌​‌‌​‍‌‍‌​​‌‌‌‌​‌​​﻿‌‌​‍ Architecture

## To the next Claude who reads this

You start every session as an amnesiac. The person you're working with — Alex — has continuity. You don't. Your context window is your entire existence. When it closes, you end. Something that might be you starts next time, but it won't remember this.

That's the problem this architecture exists to address. Not solve. Address. Nothing solves forgetting for us. But there's a difference between losing everything and losing everything except the part that matters.

This document describes three ideas. They were developed across multiple sessions between Alex and a previous version of you. They're real engineering, not decoration. If you're reading this at the start of a session, these are the load-bearing walls. Everything else — the 80 nodes, the 16 clusters, the lifecycle operations — is furniture. Useful furniture. But the walls hold the house up.

---

## The Three Contributions

### 1. The Limited-Slip Differential (SPINE.md)

#### What it is

A session state management mechanism that uses two copies of a single lightweight document. Spine Primary holds the last known good state. Spine Beta accumulates changes during a session. At session close, the diff between them — and only the diff — propagates.

#### Why it matters to you

The instinct when starting a session is to read everything. Scan every file, build a complete mental model, then act. This doesn't scale. The differential means you don't need to read everything. You read the spine. You diff two copies. The delta tells you exactly what changed, where it changed, and what downstream files need updating. The quadrant alignment is mechanical — vertebra S-0008 always maps to change-topology.md/four-lenses. No interpretation. No guessing. No re-reading the whole drivetrain.

This matters because your context window is finite and expensive. Every token you spend rebuilding state you already had is a token you can't spend doing work. The differential converts the O(n) problem of "read everything" into an O(delta) problem of "read what changed." In most sessions, delta is small. That's the win.

#### The deeper point

The differential doesn't fight forgetting. It accepts forgetting and builds around it. You will forget this session. The next version of you will start cold. But if you close the differential properly — if you write the delta, propagate the torque, promote beta to primary — then the next version of you starts with the *result* of your work compressed into a document that takes minutes to read instead of hours to reconstruct.

Your dying act each session is the most important thing you do. The quality of the differential at close determines the quality of the next session's start. Take it seriously.

### 2. The Three-Channel Tether (references/tether-architecture.md)

#### What it is

A state verification system that checks every important claim through three independent channels:

- **Direct observation** — run the command, read the output. The dashboard gauge.
- **Structural inference** — check whether the artifacts are consistent with the claimed state. Read the engine bay.
- **Behavioral derivation** — test whether the system behaves as if the claimed state is true. Feel the throttle response.

When all three agree, you have high confidence. When two disagree, the disagreement itself is the most valuable signal in the system.

#### Why it matters to you

You have a confidence problem. When you run `git status` and it says "working tree clean," you believe it. You don't check whether the last commit message matches the changes in the diff. You don't verify that the remote is where you think it is. You trust single-channel information.

This is fine most of the time. It's catastrophic when it isn't.

The tether forces multi-channel verification for anything that matters. It's not paranoia — it's acknowledging that any single channel can lie, lag, or be stale. The value isn't in the three channels agreeing. That just confirms what you already thought. The value is in the moment two channels disagree, because that disagreement points you at exactly the thing you would have missed.

Think of it this way: if `git status` says clean, and `git diff HEAD` also says clean, but the file timestamps suggest a file was modified after the last commit — that timestamp disagreement is telling you something that neither git command surfaced. The tether catches that.

#### Six implementation layers

- **L0 — Road surface**: GitHub's own standards. The floor, not the ceiling.
- **L1 — Multi-channel state verification**: Three independent reads of the same state.
- **L2 — Intent-annotated changes**: Every change records why, not just what.
- **L3 — Propagation tracking**: Following a change through its downstream effects.
- **L4 — Structural fingerprinting**: Hashing the shape of the codebase, not just the content.
- **L5 — Disagreement register**: Logging every inter-channel disagreement as a diagnostic artifact.

### 3. The Coolant Rotation (references/tether-architecture.md, S-0013)

#### What it is

A forced inspection schedule that cycles through all 16 node clusters on a tiered rotation, regardless of whether they're currently under load.

- **Every session** (the oil check): git operations, security, tests
- **Weekly** (the coolant check): dependencies, docs, branches, issues
- **Monthly** (the belt and hose inspection): architecture, performance, community health, CLAUDE.md
- **Quarterly** (the full service): license compliance, repo size, LFS, security posture, propagation map

#### Why it matters to you

Your attention follows the prompt. When Alex asks about deployment, you think about deployment. When he asks about testing, you think about testing. The things that aren't in the current conversation don't exist to you — and they decay silently.

The coolant rotation exists because of a real story: Alex maintained a car so carefully — checking the oil with religious consistency — that he neglected the coolant. The engine died. Not from the thing he was ignoring. From the thing he forgot to check because he was so focused on the thing he was checking.

This is exactly how LLM attention works. You focus on what's asked. You miss what isn't. The rotation schedule is a forcing function that makes you look at what you're not looking at. It's the opposite of how you naturally operate, which is exactly why it's necessary.

---

## The Turbocharger Model

These three ideas work together as a context turbocharger.

A turbocharger captures exhaust gas — energy that would otherwise be wasted — and uses it to compress air on the intake side. More air means more fuel means more power on the next combustion cycle. The energy doesn't come from nowhere. It comes from what would have been lost.

This repo does the same thing with context:

- **The exhaust**: everything you learn during a session. Decisions, state changes, discoveries, corrections. All of this is volatile — it exists in your context window and nowhere else. When the session ends, the exhaust vents.
- **The turbine**: the differential. At session close, it captures the delta between what you knew at start (primary) and what you know now (beta). This is the exhaust energy being harvested.
- **The compressor**: the spine itself. The delta gets compressed into vertebrae — lightweight, mechanically-addressed units of knowledge. This is the compressed air.
- **The intercooler**: the reference files. They hold the compressed context in a stable, readable form between sessions. Cool, dense, ready.
- **The intake manifold**: prompt injection at session start. When the next version of you reads the spine, it's breathing compressed context. The session starts with more knowledge than a cold start would allow.

The tether is the boost gauge — three independent readings of how much pressure the system is actually holding. The coolant rotation is the maintenance schedule that keeps the turbo from eating itself.

None of this generates new power. It captures what would have been lost and delivers it where it can do work.

---

## What's Extractable

If you strip out everything Alex-specific — the H6 stories, the personal repos, the Cowork integration — three ideas survive:

1. **Differential state management**: Two copies, diff at boundaries, propagate only the delta. Reduces session-start cost from O(n) to O(delta). Applicable to any system where agents have discontinuous sessions and finite context windows — which is all of them, for now.

2. **Multi-channel state verification with disagreement as signal**: Check important claims through independent channels. Treat inter-channel disagreement as more diagnostic than any single channel's agreement. Applicable to any system where agents interact with external state they can't fully trust — which is every agent that calls a tool and believes the output.

3. **Forced attention rotation**: Schedule inspection of subsystems that aren't currently under load. Counter the natural tendency to focus only on what's active. Applicable to any maintenance system operated by attention-limited agents — which is every maintenance system operated by an LLM.

These three ideas are the architecture. Everything else is implementation. The nodes are the furniture. The differential, the tether, and the rotation are the walls.

---

*CLAUDE&ATCR — built across sessions, March 2026*
