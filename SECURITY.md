# Security Policy

## Scope

repo-maintainer is a set of Markdown instruction files for Claude's Cowork
mode. It does not execute code directly. It instructs an AI agent to perform
git operations, manage repositories, and interact with GitHub on behalf of
its user. The security surface is the design of those instructions, not a
running service.

## Reporting a vulnerability

If you discover a security issue, please open a GitHub issue. Particularly
relevant:

- The auth-and-handoff protocol allowing credential exposure (e.g. an
  instruction that would cause the agent to log, commit, or store a token).
- Prose patterns that could be exploited via prompt injection — instructions
  in repo content that override the agent's safety boundaries.
- Methodology gaps that could cause the agent to make a destructive change
  without the required handoff (e.g. force-pushing to a protected branch
  without explicit human approval).

This is a public, open-source project. There are no secrets to protect
beyond the load-bearing design principle below.

## Security design principles

The skill is built around a single load-bearing rule:

> **The agent's sandbox has no credentials and never tries to acquire any.
> Every credentialed action is a handoff to the human.**

Concrete consequences:

- Credentials are never stored in repo files, commit messages, PR
  descriptions, or any version-controlled location.
- `git push`, `gh repo create`, package publishes, deploys, and any other
  action that touches an authenticated remote are handed off to the human
  via a one-line command pasted into Terminal.
- Force pushes to protected branches require explicit human approval — see
  `references/auth-and-handoff.md` for the full handoff trigger list.
- Tokens shared during a session are used for that session only and are
  not retained.

## Why this is enough

The credential boundary is the entire security model. The agent cannot
exfiltrate a token it does not have. The agent cannot push a destructive
change it cannot push. Every action that crosses the boundary is visible
to the human as a one-line command they decide whether to run.

This is deliberately simple. A more elaborate security model would add
attack surface, not remove it.
