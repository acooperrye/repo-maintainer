# Security Policy

## Scope

repo-maintainer is a set of markdown instruction files for Claude's Cowork mode. It does not execute code directly, but it instructs an AI agent to perform git operations, manage repositories, and interact with GitHub on behalf of its user.

## Reporting a vulnerability

If you discover a security issue — particularly anything related to:
- The auth handoff protocol allowing credential exposure
- Instructions that could lead to unintended data disclosure
- Patterns that could be exploited via prompt injection

Please report it by opening a GitHub issue. This is a public, open-source project — there are no secrets to protect beyond the design principle that credentials should never be stored in version-controlled locations.

## Security design principles

- Credentials are never stored in repo files, commit messages, or PR descriptions
- The auth protocol hands off to the human for all authentication operations
- Force pushes require explicit human approval
- Token handling is session-scoped, not persistent
