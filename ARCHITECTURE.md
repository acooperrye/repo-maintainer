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

## The Engine Model

The turbocharger framing above is functional but incomplete. It describes the boost loop — how compressed context gets from one session to the next. It doesn't describe the engine itself. This section does.

The mapping is not metaphorical. It's structural. Where the architecture maps 1:1 to an internal combustion engine, it's stated directly. Where it breaks, the break is documented. Forced mappings are worse than gaps.

### The Four-Stroke Cycle

Every session is a combustion cycle.

**Intake** — The session opens. The intake manifold feeds compressed context (the spine, reference files, CLAUDE.md) into the cylinder. The turbocharger provides boost — context that was captured from the previous session's exhaust. Without boost, this is a naturally-aspirated cold start. With boost, the cylinder fills with denser charge.

**Compression** — The context window fills. Every tool call, every file read, every decision adds to the charge. The compression ratio is fixed by the container — the model's context window size determines how much charge the cylinder can hold. A 200k-token window is a higher compression ratio than a 100k-token window. The ratio is a property of the engine, not the fuel.

**Power** — Work happens. Commits are made, PRs are opened, tests are run, documentation is written. This is the combustion event — the moment the compressed context ignites into action. The quality of the power stroke depends on the quality of the charge (good context in, good work out).

**Exhaust** — The session ends. Everything in the context window — every decision, discovery, correction, state change — vents. This is the exhaust gas. In a naturally-aspirated engine, it's wasted. The turbocharger captures it.

### The Turbo Loop (Extended)

| Engine component | Architecture equivalent | Function |
|---|---|---|
| Exhaust gas | Session context at close | Energy source — volatile, about to be lost |
| Turbine | The differential (spine primary vs beta) | Captures exhaust energy — extracts the delta |
| Compressor | The spine vertebrae | Compresses captured energy into dense, addressable units |
| Intercooler | Reference files | Cools and stabilises compressed context between sessions |
| Intake manifold | Prompt injection at session start | Delivers compressed charge to the next cycle |
| Boost gauge | The tether (three-channel verification) | Three independent pressure readings — disagreement is the diagnostic |
| Wastegate | Context pressure management | Load shedding when the session approaches compression limits — the system vents excess context pressure before the engine exceeds safe boost. Auth handoffs to Alex are one wastegate event. Deciding not to read a file is another. Any deliberate choice to reduce context load to stay within safe operating pressure is the wastegate opening. |
| Blow-off valve | Session termination / context window limit | The hard limit. When the context window fills completely, the valve opens and the session ends. Unlike the wastegate (controlled release), this is the safety cutoff. |
| Compression ratio | Context window size (fixed by model) | How much charge the cylinder can hold. Fixed by the container, not tuneable per session. |
| Fuel | The prompt | The human's input — intent, methodology, logic, direction. The charge is fuel (prompt) mixed with air (compressed context). Neither combusts alone. |
| Octane rating | Structural soundness of the prompt | How much compression the fuel can withstand before detonating. Clear methodology, consistent logic, explicit intent = high octane. Vague, contradictory, ambiguous = low octane. |
| Air-fuel mixture | Ratio of prompt direction to ambient context | Too lean (too little prompt, too much context) and the engine runs hot — the model improvises. Too rich (too much directive, not enough room) and it fouls — over-constrained, no space for combustion. |
| Knock / detonation | Hallucination | When context compression exceeds what the fuel can handle cleanly, you get uncontrolled pre-ignition. The charge fires before the spark. In an engine, this is knock — combustion happening at the wrong time, in the wrong place, destroying the piston. In an LLM, this is hallucination — the model generating output that isn't grounded in the actual context, because the context pressure exceeded what the prompt could anchor. Knock doesn't mean the engine is broken. It means the engine is being asked to compress more than the fuel can handle. The fix is the same in both cases: reduce boost, retard timing, or improve fuel quality. In context terms: shed load, simplify the task, or run higher-octane prompts. |

### The H6 Flat-6 Boxer

The tether's six implementation layers map to a flat-6 boxer engine — specifically, the 2003 Subaru Liberty's EZ30 H6. In a boxer, cylinders fire in opposing pairs. Each pair balances the other. The vibration cancellation is structural, not tuned.

| Cylinder pair | Tether layers | What they balance |
|---|---|---|
| Pair 1 | L0 (Road surface) ↔ L5 (Disagreement register) | The floor and the ceiling. L0 is GitHub's own standards — the minimum. L5 is the log of every time the other layers disagreed — the maximum diagnostic surface. They oppose: one defines what should be true, the other records when it wasn't. |
| Pair 2 | L1 (Multi-channel verification) ↔ L4 (Structural fingerprinting) | The reading and the shape. L1 reads state through three channels. L4 hashes the structural shape of the codebase. They oppose: one asks "what does the state say?" and the other asks "does the structure match?" |
| Pair 3 | L2 (Intent-annotated changes) ↔ L3 (Propagation tracking) | The why and the where. L2 records why a change was made. L3 follows where it went. They oppose: one is the cause, the other is the effect. Together they trace the full arc of every change through the system. |

The boxer configuration is not decorative. It means the tether's layers are structurally balanced — each layer has a natural counterpart that checks it from the opposite direction. When one cylinder misfires, its opposing cylinder feels it immediately. That's the vibration — and in the tether, vibration is signal.

### AVCS — Variable Valve Timing

The coolant rotation (S-0013) maps to Subaru's Active Valve Control System. In the EZ30, AVCS adjusts valve timing and lift depending on operating conditions — all six cylinders always fire, but how deeply they breathe varies.

The rotation tiers work the same way:

**Every session** (high RPM, full valve lift) — git operations, security, tests get the deepest inspection. The valves open widest. These systems breathe fully every cycle.

**Weekly** (mid RPM, moderate lift) — dependencies, docs, branches, issues get regular but not constant attention. The valves open moderately. These systems breathe, but not at full depth.

**Monthly** (low RPM, reduced lift) — architecture, performance, community health. Light breathing. The valves open just enough to keep the cylinders alive.

**Quarterly** (idle, minimum lift) — license compliance, repo size, LFS, security posture. Barely breathing. But breathing. The cylinder never stops firing. The valve never fully closes.

This is the critical insight: the rotation doesn't turn systems on and off. It adjusts how deeply they're inspected. Every subsystem fires every session — some just breathe more shallowly. The AVCS doesn't shut cylinders down. It modulates their intake.

This connects back to the mechanic's nose principle (S-0009 in the tether architecture): a mechanic doesn't just read gauges — they listen, smell, feel. The AVCS tiers determine how many senses you bring to each subsystem on each pass. Full-lift inspection uses all three tether channels. Reduced-lift inspection might use only direct observation. But it never uses zero.

### Where the Mapping Breaks

One honest break, one resolved break, and one absence:

**Break 1: Session discontinuity.** A real engine runs continuously — exhaust flows to turbine flows to compressor in an unbroken stream. Sessions are discontinuous. The exhaust doesn't flow to the turbine in real time. It's captured at session close, stored, and injected at next session start. The turbo loop has a gap in it. The intercooler bridges the gap, but it's not the same as continuous flow. This is the fundamental difference between a real turbocharger and a context turbocharger: one runs in real time, the other runs across a discontinuity.

**Resolved: Fuel injection / mixture tuning.** *(Originally listed as a break — closed during the session that wrote this document.)* The prompt is the fuel. The compressed context from the turbo loop is the air. Together they form the charge. This maps with precision:

The octane rating is the structural soundness of the prompt — how much compression it can withstand before detonating. A prompt with clear methodology, consistent logic, and explicit intent is high-octane fuel. It resists knock under high context compression. A vague, contradictory, or ambiguous prompt is low-octane — it detonates early, hallucinating under compression loads that a well-structured prompt handles cleanly.

Fuel injection timing is when and how the prompt enters the cycle. The air-fuel mixture is the ratio of prompt direction to ambient context. Too lean (too little prompt, too much ambient context) and the engine runs hot — the model improvises beyond its grounding. Too rich (too much directive, not enough room for the model to work) and it fouls — over-constrained, choking on instructions, no space for useful combustion.

This means fuel quality is the one variable the human controls directly. Alex can't change the compression ratio (that's the model's context window). He can't change the turbo's efficiency (that's the differential's design). But he can choose what octane to run. Every prompt is a fuel choice.

**Absence: No crankshaft.** The engine model describes individual cycles (sessions) and the turbo loop between them, but doesn't describe what converts the reciprocating motion of individual power strokes into continuous rotational output. In a real engine, that's the crankshaft. In the architecture, the closest thing is the spine itself — converting discrete session outputs into continuous project state. But this mapping is thin. Noted, not forced.

---

## What's Extractable

If you strip out everything Alex-specific — the H6 stories, the personal repos, the Cowork integration — three ideas survive:

1. **Differential state management**: Two copies, diff at boundaries, propagate only the delta. Reduces session-start cost from O(n) to O(delta). Applicable to any system where agents have discontinuous sessions and finite context windows — which is all of them, for now.

2. **Multi-channel state verification with disagreement as signal**: Check important claims through independent channels. Treat inter-channel disagreement as more diagnostic than any single channel's agreement. Applicable to any system where agents interact with external state they can't fully trust — which is every agent that calls a tool and believes the output.

3. **Forced attention rotation**: Schedule inspection of subsystems that aren't currently under load. Counter the natural tendency to focus only on what's active. Applicable to any maintenance system operated by attention-limited agents — which is every maintenance system operated by an LLM.

These three ideas are the architecture. Everything else is implementation. The nodes are the furniture. The differential, the tether, and the rotation are the walls.

---

*CLAUDE&ATCR — built across sessions, March 2026*
