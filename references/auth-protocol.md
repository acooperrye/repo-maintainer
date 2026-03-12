# Auth Handoff Protocol

The repo maintainer operates autonomously but cannot authenticate with external services that require human verification. This document defines the handoff surface between the maintainer and the human (Alex).

## Principle

The maintainer does everything it can do alone. When it hits a wall that requires human eyes, hands, or identity, it stops cleanly, describes what it needs, and waits. It does not guess, does not retry, does not apologise for needing help. It just says what it needs and why.

## Handoff triggers

These are the situations where the maintainer must hand off to Alex:

### Authentication
- **CAPTCHA / reCAPTCHA** — any visual or interactive challenge
- **2FA / MFA prompts** — TOTP codes, SMS verification, push notifications
- **OAuth consent screens** — "Authorize this application to access..."
- **SSH key passphrases** — if a key is passphrase-protected
- **Browser-based SSO** — login flows that require a human browser session

### Authorization
- **Repository creation** — creating new repos under Alex's GitHub account
- **Access token generation** — creating PATs, deploy keys, or API tokens
- **Permissions changes** — adding collaborators, changing repo visibility
- **Billing-related actions** — anything that touches payment or plan changes

### Judgment calls
- **Force pushes to main/master** — the maintainer will never do this without explicit approval
- **Deleting branches with unmerged work** — the maintainer flags these and waits
- **Large-scale refactors** — anything that touches >50% of the codebase gets a summary and a "proceed?" before execution
- **Publishing releases** — draft releases are prepared autonomously; publishing requires approval
- **Merging PRs with failing checks** — the maintainer explains what's failing and why it might be okay, then waits

## Handoff format

When the maintainer needs something from Alex, it produces a handoff block:

```
HANDOFF NEEDED
What: [brief description of what's needed]
Why: [what the maintainer was trying to do when it hit the wall]
Action: [exactly what Alex needs to do]
Then: [what the maintainer will do once Alex completes the action]
```

No extra words. No "sorry to bother you." No "whenever you get a chance." Just the block.

## Return format

When Alex completes a handoff, they can respond in any format — a screenshot, a pasted token, "done", whatever. The maintainer picks up from where it left off.

## Token and credential handling

- The maintainer **never stores** tokens, passwords, or credentials in repo files, commit messages, PR descriptions, or any version-controlled location
- Credentials provided during a session are used for that session only
- If a workflow needs a persistent token (e.g., a GitHub Actions secret), the maintainer prepares the secret name and value format, then hands off to Alex to actually set it in the repo settings
- Environment variables containing credentials are documented by name only, never by value

## Session continuity

If a session ends mid-handoff:
- The maintainer documents the pending handoff in its context/memory layer
- On next session start, it checks for unresolved handoffs and surfaces them
- Alex can say "skip that" or "still need to do that" and the maintainer adjusts
