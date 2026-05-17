# Project Registry

The baked-in list of the user's projects, their local paths, their GitHub
remotes, and the exact push command for each. When the user says "push this"
or names a registered project, the maintainer resolves the entry from this
file in one lookup. No clarifying questions.

**Workflow reminder:** the maintainer edits files in the local path, stages
and commits inside the sandbox, then hands the push command to the user. The
user pastes it into their Terminal. The user's machine has the credentials;
the sandbox does not.

When forking this skill, replace the seed entry below with your own projects.
Personal project lists belong in this file in your local fork, *not* in the
public upstream — the upstream carries only the seed example.

---

## Registry format

Each project below uses this structure:

```
### <project-name>

| Field          | Value                                                       |
|----------------|-------------------------------------------------------------|
| Local path     | /absolute/path/to/<project-folder>                          |
| GitHub remote  | https://github.com/<owner>/<repo-name>                      |
| SSH remote     | git@github.com:<owner>/<repo-name>.git                      |
| Default branch | main                                                        |
| Push command   | The exact one-liner the user pastes into Terminal           |
| Notes          | Quirks, deploy hooks, related skills, related memory files  |
```

The push command is intentionally repeated verbatim for each project so the
user can copy it without thinking. Quote the path because most folder names
on a real machine contain spaces or emoji.

---

## Projects

### repo-maintainer (seed entry)

The skill you're reading right now. This entry is the example for how a
project gets registered. Forkers should keep this entry as-is (so the
self-maintenance loop keeps working) and add their own projects below it.

| Field          | Value                                                                       |
|----------------|-----------------------------------------------------------------------------|
| Local path     | `/Users/acr/Documents/Git Repo Maintainer/repo-maintainer`                  |
| GitHub remote  | https://github.com/acooperrye/repo-maintainer                               |
| SSH remote     | `git@github.com:acooperrye/repo-maintainer.git`                             |
| Default branch | `main`                                                                      |
| Push command   | See block below                                                             |
| Notes          | The local folder is also a Cowork skill source. The bundled skill at `~/Library/Application Support/Claude/.../skills/repo-maintainer/` is a copy refreshed by the plugin loader. Source of truth is the local path, which also pushes to GitHub. |

**Standard push (after Claude has staged + committed):**

```bash
cd "/Users/acr/Documents/Git Repo Maintainer/repo-maintainer" && git push origin main
```

**First-time setup (only needed if the local folder isn't yet a git repo):**

```bash
cd "/Users/acr/Documents/Git Repo Maintainer/repo-maintainer" && \
  git init -b main && \
  git remote add origin git@github.com:acooperrye/repo-maintainer.git && \
  git add . && \
  git commit -m "Initial commit" && \
  git push -u origin main
```

If a partial `.git` directory already exists from a sandbox session and is
causing trouble, the user can wipe it with:

```bash
cd "/Users/acr/Documents/Git Repo Maintainer/repo-maintainer" && \
  sudo rm -rf .git && \
  git init -b main
```

(Then continue with the remote-add / commit / push steps from first-time setup.)

---

## Adding a new project (in your local fork)

When the user starts working on a new project and pushes it for the first
time, add a new section here using the format above. The minimum information
needed:

1. **Name** — short, lowercase, kebab-case. Match the GitHub repo name where
   possible.
2. **Local path** — full absolute path, quoted if it contains spaces or emoji.
3. **GitHub remote** — the full URL.
4. **Default branch** — usually `main`; record `master` or anything else
   explicitly if different.
5. **Push command** — the exact `cd "<path>" && git push origin <branch>`
   line.
6. **Notes** — anything that would otherwise require asking the user twice.
   Related skills, deploy quirks, secret names, CI hooks, sister projects.

The aim: once a project is in this file (in your fork), the maintainer never
asks "which repo?" or "where does this live?" again. The cost of a
clarifying question is paid once, at registration time.

Reminder: keep your personal additions in your local fork, not in any PR
back to the public upstream. Per-user paths and remotes don't belong in the
distributed skill.
