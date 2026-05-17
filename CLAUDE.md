# CLAUDE.md — repo-maintainer

This repository contains the repo-maintainer Cowork skill. If you're a Claude
session working on this repo *itself* (as opposed to using it to maintain a
different repo), here's what you need to know.

## What this repo is

A Cowork skill — a directory of Markdown documents that instruct Claude how
to maintain GitHub repositories well. The "software" is the prose; there is
no code to build. The methodology is the content.

## Rules for working on this repo

- **Plain language only.** No vehicle, drivetrain, engine, or other
  mechanical metaphors. Earlier versions of this skill were saturated with
  them; the explicit rewrite goal was to remove them. If you find one that
  survived, fix it.
- **Methodology over inventory.** The 80-node catalogue in
  `references/nodes.md` is a menu, not the heart of the system. The heart
  is the workflow contract, the multi-channel verification, and the
  two-state diff in `SPINE.md`.
- **Stubs are valid.** Some nodes are listed without full definitions
  because their purpose is uncertain. Don't fill them in by guessing.
- **One entry, one target.** In `SPINE.md`, each entry maps to exactly one
  reference file + section. If an entry would need to publish to two places,
  split it.
- **The workflow contract is load-bearing.** Edits happen in the local
  project folder; the final `git push` is handed to the human via a
  one-liner. Don't try to push from inside the agent's sandbox.

## Files in this repo

| File | Purpose |
|------|---------|
| `SKILL.md`                              | Entry point. Read first. |
| `SPINE.md`                              | The two-state working-memory model. |
| `README.md`                             | Public-facing description. |
| `references/projects.md`                | Per-project registry of paths, remotes, push commands. |
| `references/auth-and-handoff.md`        | When and how to hand off to the human. |
| `references/github-standards.md`        | The baseline GitHub recommends for any repo. |
| `references/change-topology.md`         | Four-layer model for change on top of git. |
| `references/verification-layers.md`     | Multi-channel verification + inspection rotation. |
| `references/existing-skills-bridge.md`  | Cooperation pattern with other Cowork skills. |
| `references/nodes.md`                   | Catalogue of capabilities by cluster. |

## Owner

Alexander Cooper-Rye (@acooperrye).
