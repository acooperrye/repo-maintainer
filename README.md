# repo-maintainer

A Cowork skill that manages GitHub repositories autonomously. Not a helper. Not an advisor. The maintainer.

## What this is

An orchestration layer over 80+ discrete functional nodes across 16 clusters — git operations, development workflow, project management, testing, deployment, code intelligence, documentation, frontend/design, memory/context, autonomous operation, backend, security, analytics, content/creative, external integrations, and meta/utilities. The maintainer operates autonomously and hands off to the human only for authentication, authorization, and judgment calls.

## Architecture

The system uses a **limited-slip differential** as its governing metaphor — not decoratively, but mechanically. Two output shafts (spine primary and spine beta) track known state versus working state. At session close, the speed differential between them tells the system exactly which reference files need updating, what the changelog is, and whether anything has drifted.

- `SKILL.md` — entry point, operating principles, lifecycle operations
- `SPINE.md` — the differential itself, vertebrae (units of knowledge), session operations
- `references/` — 8 reference documents covering nodes, auth protocol, skill integration, change topology, tether architecture, GitHub standards, architectural insights, and relational navigation

## How it works

The maintainer activates when you mention git, repos, commits, PRs, deployment, testing, or any work that flows through GitHub. It reads the spine, engages the differential, and gets to work. When it hits something that needs human eyes (2FA, OAuth, force pushes, publishing releases), it produces a clean handoff block and waits.

The 80+ nodes are discrete for a reason — like a drivetrain where each component has a specific mechanical function. The fragmentation is the design. A placeholder bearing is still a bearing.

## Installation

This is a [Cowork](https://claude.ai) skill. To install:

1. Download `repo-maintainer.skill` from the [latest release](../../releases/latest)
2. Open it in Cowork — it installs automatically

Or clone this repo into your Cowork skills directory:

```
git clone https://github.com/acooperrye/repo-maintainer.git \
  ~/Documents/Claude/Cowork/skills/repo-maintainer
```

## Origin

Built across multiple Claude sessions as a collaboration between Alexander Cooper-Rye and Claude. The architectural insights draw from a semantic archaeology of 99 single-install Claude Code plugins, mechanical metaphors from a 2003 Subaru Liberty H6, and an information architecture framework derived from 20 Questions.

The skill installed itself into the system it was designed to maintain. The ouroboros is load-bearing.

## License

Dual-licensed:
- **MIT** — code and functional components
- **CC-BY-SA 4.0** — architecture, design, and written content

See [LICENSE](LICENSE) for details.

## Maintainer

Alexander Cooper-Rye ([@acooperrye](https://github.com/acooperrye))
