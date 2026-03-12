# repo-maintainer

A context turbocharger for LLM workspace sessions.

## What this is

Every LLM session ends in exhaust — decisions made, state discovered, corrections applied, context built — all of it volatile, all of it lost when the window closes. This repo is a turbocharger: it captures that exhaust energy and compresses it for injection into the next session's prompt.

The turbine is a **limited-slip differential** that diffs two copies of a lightweight spine document at session close, extracting only what changed. The compressor turns those deltas into vertebrae — mechanically-addressed units of knowledge that map to specific reference files. The intercooler holds compressed context between sessions. At next session start, the intake manifold feeds it back in: the new session breathes compressed context instead of starting cold.

On top of this runs an orchestration layer of 80+ discrete functional nodes across 16 clusters — git operations, testing, deployment, security, documentation, and more. The nodes are the furniture. The differential, the tether, and the rotation are the walls.

## The three contributions

This repo contains three ideas that survive being extracted from their original context:

**1. Differential state management** ([SPINE.md](SPINE.md)) — Two copies of a single document. Primary holds known state. Beta accumulates changes. At session boundaries, diff them. The delta — and only the delta — propagates. Converts the O(n) problem of "read everything" into an O(delta) problem of "read what changed."

**2. Multi-channel state verification** ([references/tether-architecture.md](references/tether-architecture.md)) — Check every important claim through three independent channels: direct observation, structural inference, and behavioral derivation. When two channels disagree, the disagreement itself is the most valuable signal in the system.

**3. Forced attention rotation** ([references/tether-architecture.md](references/tether-architecture.md), S-0013) — A tiered inspection schedule that cycles through all subsystems regardless of current load. Counters the natural tendency of attention-limited agents to focus only on what's active while adjacent systems decay silently.

Read [ARCHITECTURE.md](ARCHITECTURE.md) for the full technical discussion — including why these ideas matter, where they came from, and what they look like extracted from this specific implementation.

## Files

- `SKILL.md` — entry point, operating principles, lifecycle operations
- `SPINE.md` — the differential, vertebrae, session state management
- `ARCHITECTURE.md` — technical deep-dive on the three core contributions
- `references/` — 8 reference documents covering nodes, auth protocol, skill integration, change topology, tether architecture, GitHub standards, architectural insights, and relational navigation

## Installation

This is a [Cowork](https://claude.ai) skill for Claude. To install:

1. Download `repo-maintainer.skill` from the [latest release](../../releases/latest)
2. Open it in Cowork — it installs automatically

Or clone into your Cowork skills directory:

```
git clone https://github.com/acooperrye/repo-maintainer.git \
  ~/Documents/Claude/Cowork/skills/repo-maintainer
```

## Origin

Built across multiple Claude sessions as a collaboration between Alexander Cooper-Rye and Claude. The architectural insights draw from a semantic archaeology of 99 single-install Claude Code plugins, mechanical metaphors from a 2003 Subaru Liberty H6, and an information architecture framework derived from 20 Questions.

The skill's first act as maintainer was pushing itself to GitHub. The ouroboros is load-bearing.

## License

Dual-licensed:
- **MIT** — code and functional components
- **CC-BY-SA 4.0** — architecture, design, and written content

See [LICENSE](LICENSE) for details.

## Maintainer

Alexander Cooper-Rye ([@acooperrye](https://github.com/acooperrye))
