# Cooperating with Other Cowork Skills

The repo-maintainer is one skill in a workspace that may have many. Other
skills bring domain expertise the maintainer doesn't have. This document
describes how the maintainer cooperates with them.

## The pattern

Other skills are **specialists** — they own a domain (a specific creative
project, a publishing pipeline, an analytical workflow). The maintainer is
the **coordinator** — it owns the git/repo operations around whatever the
specialists produce.

Rule of priority: when a task could be handled by both the maintainer and a
specialist skill, the specialist wins for its domain. The maintainer wraps
the version-control work around the specialist's output, not the other way
around.

This is a one-way deference. The maintainer never tries to replicate a
specialist's expertise. If the user has a skill that produces encoded HTML
pages, the maintainer doesn't try to do the encoding itself — it commits
what the specialist produced.

## How to recognise a specialist domain

A specialist skill is in play whenever:

- The user mentions the skill by name (`"use the attentional-surface skill"`).
- The task involves a domain the user has previously delegated to a specific
  skill (creative content, encoding, a named project's workflow).
- The output the user is asking for has a recognisable signature of another
  skill (a particular file format, a particular file location).

In those cases, defer to the specialist for the domain work, then handle the
git/repo operations around the result.

## Generic deference table

| Specialist domain | Maintainer's role |
|---|---|
| A creative project with its own publishing workflow | Git operations around the project source — commits, PRs, releases, dependency updates, CI/CD. |
| Context-bridging between Claude sessions or Cowork modes | Acting on bridged content — creating issues, branches, PRs based on what was handed off. |
| Between-session reflection or async idea processing | Turning surfaced insights into concrete repo actions (issues, spikes, branches). |
| Memory extraction from Chat sessions | Using the extracted context to inform repo decisions. |
| Research transliteration (between domains, registers, vocabularies) | Integrating research findings into documentation, ADRs, or implementation. |
| Encoding / decoding / steganography | Committing encoded outputs and managing the files they live in. |
| Document integrity markers / structural beacons | Embedding the markers in generated content; preserving them in commits. |
| Project-specific sync protocols | Repo operations for the specific project; deferring to the specialist for sync logic. |
| Scrapbook or asset generation | Committing generated assets, managing their directory layout. |
| Chat→Cowork session priming | Awareness only — the priming itself is the specialist's job, not the maintainer's. |

## What never gets delegated

These remain the maintainer's job regardless of which specialists are
installed:

- `git add`, `git commit`, branch management, tag management.
- The push handoff (one-liner to the user's terminal).
- Reviewing diffs before commit.
- Writing structured intent annotations on PRs.
- Running the multi-channel verification rotation.
- Maintaining the project registry in `references/projects.md`.
- Surfacing CI failures, security alerts, and stale branches.

## Example: a specialist + maintainer handoff

A user has a creative-publishing skill that generates encoded pages for a
website. The maintainer's flow:

1. User asks the maintainer to publish a new page.
2. Maintainer recognises the encoding work belongs to the specialist —
   hands the content generation to it.
3. Specialist produces the encoded page.
4. Maintainer takes the output, stages it in the project folder, commits
   with an intent annotation, hands the user the push command.
5. User pushes from Terminal; maintainer verifies the change is live on the
   deployed site (via the verification layers in
   `verification-layers.md`).

The handoff in the middle is the entire point. Neither side does the other's
work.
