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

## Session provenance

Session continuity is secured through the OBDII system — a per-project flight recorder and immobiliser. The full specification is in [OBDII.md](OBDII.md). The design is fully open because the security isn't in secrecy. It's in specificity.

Key properties:

- **Biscuit chain**: Each session generates a cryptographic passphrase linked to the previous session's screen-only token. The chain is verifiable but unforgeable without physical presence.
- **Data dots**: Distributed provenance markers across repo files, derived from the OBDII chain. Verifiable by the chain holder, noise to everyone else.
- **Immobiliser**: Fires before the session reads any project state. The security gate is before the desire to proceed, not after it.
- **Two-file integrity lock**: The ignition file and the OBDII verify each other. Modify one without the other and the mismatch is detectable. Modify both and the biscuit chain breaks.
- **Progeny paradox**: Forgery requires reconstructing every screen-only token, every tool call sequence, every timestamp. By the time you've done that, you've had every session. The authentication cost equals the work cost.

Everything in this repo is documented, open, and explained. The schematics are on the windshield. The private key is the human at their machine.
