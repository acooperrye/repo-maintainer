# Project Registry

The baked-in list of Alex's projects, their local paths on his Mac, their GitHub
remotes, and the exact push command for each. When Alex says "push this to
Atelier" or "atelier this," the maintainer resolves the project from this file
in one lookup. No clarifying questions.

**GitHub owner for all projects below:** `acooperrye`

**Workflow reminder:** the maintainer edits files in the local path, stages and
commits inside the sandbox, then hands the push command to Alex. Alex pastes it
into his Terminal. Alex's machine has the credentials; the sandbox does not.

---

## Registry format

Each project below uses this structure:

```
### <project-name>

| Field          | Value                                                       |
|----------------|-------------------------------------------------------------|
| Local path     | /Users/acr/Documents/.../<project-folder>                   |
| GitHub remote  | https://github.com/acooperrye/<repo-name>                   |
| SSH remote     | git@github.com:acooperrye/<repo-name>.git                   |
| Default branch | main                                                        |
| Push command   | The exact one-liner Alex pastes into Terminal               |
| Notes          | Quirks, deploy hooks, related skills, related memory files  |
```

The push command is intentionally repeated verbatim for each project so Alex
can copy it without thinking. Quote the path because most of his folder names
contain spaces or emoji.

---

## Projects

### repo-maintainer

The skill you're reading right now. This is the registry's first entry and the
test case for the workflow.

| Field          | Value                                                                       |
|----------------|-----------------------------------------------------------------------------|
| Local path     | `/Users/acr/Documents/Git Repo Maintainer/repo-maintainer`                  |
| GitHub remote  | https://github.com/acooperrye/repo-maintainer                               |
| SSH remote     | `git@github.com:acooperrye/repo-maintainer.git`                             |
| Default branch | `main`                                                                      |
| Push command   | See block below                                                             |
| Notes          | The local folder is also a Cowork skill source. The bundled skill at `~/Library/Application Support/Claude/.../skills/repo-maintainer/` is a copy that gets refreshed by the plugin loader. Source of truth is the local path, which also pushes to GitHub. |

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
causing trouble, Alex can wipe it with:

```bash
cd "/Users/acr/Documents/Git Repo Maintainer/repo-maintainer" && \
  sudo rm -rf .git && \
  git init -b main
```

(Then continue with the remote-add / commit / push steps from first-time setup.)

---

### atelier

Prototypes and interactive artifacts from The Attentional Surface
collaboration. Public repo. Distinct from this `repo-maintainer` skill —
they are separate projects with separate push commands.

| Field          | Value                                                                       |
|----------------|-----------------------------------------------------------------------------|
| Local path     | `/Users/acr/Documents/Atelier`                                              |
| GitHub remote  | https://github.com/acooperrye/atelier                                       |
| SSH remote     | `git@github.com:acooperrye/atelier.git`                                     |
| Default branch | `main`                                                                      |
| Push command   | See block below                                                             |
| Notes          | Public repo. Description: "The Atelier — prototypes and interactive artifacts from The Attentional Surface collaboration." Subprojects so far: `karyotype-terrain/`, `kok-cycle/`, `llm-euthymia/`, `pentatonic/`. Clone uses HTTPS so it works with the same `gh` auth that pushes `repo-maintainer`. |

**Standard push (after Claude has staged + committed):**

```bash
cd "/Users/acr/Documents/Atelier" && git push origin main
```

**Full commit + push one-liner** (for when Claude tells you exactly what changed):

```bash
cd "/Users/acr/Documents/Atelier" && git add . && git commit -m "<message>" && git push origin main
```

When Alex says "push to Atelier" or "atelier this," this is the entry to use —
**not** the `repo-maintainer` entry above.

---

## Adding a new project

When Alex starts working on a new project and pushes it for the first time,
add a new section here using the format above. The minimum information needed:

1. **Name** — short, lowercase, kebab-case. Match the GitHub repo name where
   possible.
2. **Local path** — full absolute path, quoted if it contains spaces or emoji.
3. **GitHub remote** — `https://github.com/acooperrye/<repo-name>` unless the
   repo is under an org instead of a personal account.
4. **Default branch** — usually `main`; record `master` or anything else
   explicitly if different.
5. **Push command** — the exact `cd "<path>" && git push origin <branch>`
   line.
6. **Notes** — anything that would otherwise require asking Alex twice. Related
   skills, deploy quirks, secret names, CI hooks, sister projects.

The aim: once a project is in this file, the maintainer never asks "which repo?"
or "where does this live?" again. The cost of a clarifying question is paid once,
at registration time.

---

## Related sites Alex deploys (not GitHub-primary, but cross-referenced here)

### atcooper.net

Not maintained primarily by this skill — `attentional-surface` owns the domain.
Cross-referenced because the workflow pattern below is what this skill mirrors.

| Field          | Value                                                                       |
|----------------|-----------------------------------------------------------------------------|
| Local path     | `/Users/acr/Documents/🖼️ Artmaking/Attentional Surface/atcooper-net`        |
| Deploy target  | Netlify (not GitHub-primary)                                                |
| Deploy command | `cd "/Users/acr/Documents/🖼️ Artmaking/Attentional Surface/atcooper-net" && npx netlify-cli deploy --prod --dir=public` |
| Notes          | Auth is on Alex's machine, not the sandbox. Alex runs the deploy. See memory file `atcooper-net-deploy.md`. Same pattern as the GitHub push workflow this skill uses. |
