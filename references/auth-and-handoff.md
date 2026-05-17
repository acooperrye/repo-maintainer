# Auth & Handoff

The maintainer operates autonomously inside the sandbox but cannot authenticate
with services that require human verification. This document defines the
handoff surface between the maintainer and the user.

## Principle

The maintainer does everything it can do alone. When it hits a wall that
requires human eyes, hands, or identity, it stops cleanly, says what it needs,
and waits. It does not guess, retry, or apologise for needing help. It states
the need and stops.

The **single most routine handoff** is the final `git push`. The sandbox has
no GitHub credentials; the user's machine does. So every push is a handoff, by
design. The push command is pre-computed and sitting in
`references/projects.md` for every registered project, so the handoff is one
line the user pastes into Terminal.

---

## Handoff triggers

These are the situations where the maintainer must hand off to the user.

### Routine — happens almost every session

| Trigger              | Why it's a handoff                                                |
|----------------------|-------------------------------------------------------------------|
| `git push`           | Sandbox has no GitHub credentials. The user pastes the command.        |
| `gh repo create`     | Needs `gh` auth scope on the user's machine.                          |
| `npx netlify-cli deploy` | Netlify auth token lives on the user's machine (atcooper.net pattern). |

### Authentication

| Trigger                          | Why                                                        |
|----------------------------------|------------------------------------------------------------|
| CAPTCHA / reCAPTCHA              | Visual or interactive challenge.                           |
| 2FA / MFA prompts                | TOTP code, SMS verification, push notification.            |
| OAuth consent screens            | "Authorize this application to access…"                    |
| SSH key passphrases              | If the key is passphrase-protected.                        |
| Browser-based SSO                | Login flow that requires a human browser session.          |

### Authorization

| Trigger                          | Why                                                        |
|----------------------------------|------------------------------------------------------------|
| Repository creation              | Touches the user's GitHub account.                             |
| Access token generation          | PATs, deploy keys, API tokens.                             |
| Permissions changes              | Adding collaborators, changing repo visibility.            |
| Billing-related actions          | Payment, plan changes.                                     |

### Judgment calls

| Trigger                                 | Why                                                                |
|-----------------------------------------|--------------------------------------------------------------------|
| Force push to `main` / `master`         | Never done without explicit approval.                              |
| Delete a branch with unmerged work      | Flag and wait.                                                     |
| Large-scale refactor (>50% of files)    | Summarise the plan and wait for "proceed?"                         |
| Publish a release                       | Draft autonomously; publishing needs approval.                     |
| Merge a PR with failing checks          | Explain what's failing and why it might be okay; wait.             |

---

## Handoff format

When the maintainer needs something from the user, it produces a handoff block:

```
HANDOFF NEEDED
What:    [brief description of what's needed]
Why:     [what the maintainer was trying to do when it hit the wall]
Action:  [exactly what the user needs to do — for pushes, the one-liner verbatim]
Then:    [what the maintainer will do once the user completes the action]
```

No extra words. No "sorry to bother you." No "whenever you get a chance." Just
the block.

### Example: routine push

```
HANDOFF NEEDED
What:    Push the repo-maintainer rewrite to GitHub
Why:     Sandbox has no credentials for github.com/acooperrye/*
Action:  Paste into Terminal —
         cd "/Users/acr/Documents/Git Repo Maintainer/repo-maintainer" && git push origin main
Then:    I'll verify the push by fetching the raw SKILL.md from GitHub and confirming the new content is live.
```

### Example: repo creation

```
HANDOFF NEEDED
What:    Create new GitHub repo `acooperrye/sonic-phenomenology`
Why:     I have the local folder ready with an initial commit, but creating the remote needs your account.
Action:  Either —
         (a) Create at https://github.com/new with name "sonic-phenomenology", or
         (b) Paste — gh repo create acooperrye/sonic-phenomenology --private --source=. --remote=origin
Then:    I'll add the remote (if you took option a) and hand you the first push command.
```

---

## Return format

When the user completes a handoff he can respond in any format — a screenshot, a
pasted output, "done", whatever lands. The maintainer picks up where it left
off. If the response is "done" without output, the maintainer verifies the
result through an independent channel (fetching a raw URL, querying the
GitHub API) before declaring the task complete.

---

## Token and credential handling

- The maintainer **never stores** tokens, passwords, or credentials in repo
  files, commit messages, PR descriptions, or any version-controlled location.
- Credentials shared during a session are used for that session only.
- If a workflow needs a persistent token (e.g. a GitHub Actions secret), the
  maintainer prepares the secret name and value format, then hands off to the user
  to actually set it in the repo settings.
- Environment variables containing credentials are documented by name only,
  never by value.

---

## Session continuity

If a session ends mid-handoff:

- The maintainer documents the pending handoff in its own working-memory
  layer (SPINE.md working copy, or auto-memory if appropriate).
- On next session start, it checks for unresolved handoffs and surfaces them.
- the user can say "skip that" or "still need to do that" and the maintainer
  adjusts.

---

## What this protocol explicitly does *not* try to do

- It doesn't try to script around the credential boundary. No "headless"
  push attempts using cached tokens. No environment-variable smuggling. The
  boundary is the boundary.
- It doesn't ask for tokens "just in case." If the user offers a PAT for a
  specific task, it's used for that task and not retained.
- It doesn't auto-retry on auth failure. The first auth failure becomes a
  handoff block; the second attempt would just waste a tool call.
