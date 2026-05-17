# repo-maintainer

A Cowork skill that turns a Claude session into a competent GitHub repository
maintainer — one that proactively manages commits, reviews, releases,
dependencies, documentation, and CI/CD across multiple repos, with clean
boundaries around what requires human credentials.

This is a body of instructional prose, not executable code. The "software" is
a directory of Markdown documents that Claude reads. The methodology is the
content.

---

## Who this is for

- People who want their AI agent to maintain a repository with the same
  rigor a senior maintainer would: methodical, multi-channel, proactive about
  surfacing latent issues — without paying the senior maintainer's rate.
- People who already use Claude for repo work but keep re-explaining the same
  process every session.
- People who want a transparent, opinionated methodology they can read, fork,
  and tune — rather than a black-box "AI assistant for GitHub."

If you want a code-completion tool, this is not it. If you want a structured
operating system for repo maintenance, this is what it is.

---

## What's in the box

| File | Owns                                                                     |
|------|--------------------------------------------------------------------------|
| `SKILL.md`                              | Entry point. Workflow contract, operating principles, lifecycle operations. |
| `SPINE.md`                              | The skill's own working-memory model (two-state diff over a single document). |
| `CLAUDE.md`                             | Per-project instructions for any Claude session working *on this repo itself*. |
| `references/projects.md`                | Project registry — per-repo local path, remote, default branch, push command. |
| `references/auth-and-handoff.md`        | When the agent stops and asks the human (auth, judgment calls). |
| `references/github-standards.md`        | Baseline: what GitHub itself recommends for any well-maintained repo. |
| `references/change-topology.md`         | Four-layer model for recording and reasoning about change beyond `git log`. |
| `references/verification-layers.md`     | Multi-channel state verification + the inspection rotation schedule. |
| `references/existing-skills-bridge.md`  | Pattern for cooperating with other Cowork skills (specialist vs coordinator). |
| `references/nodes.md`                   | Catalogue of functional capabilities organised by cluster. |

---

## The contributions

Three pieces of this methodology are genuinely novel; several others sharpen
existing practice. The README is honest about which is which.

### 1. Two-state diff over a single working-memory document (`SPINE.md`)

Instead of re-reading the whole skill at the start of every session, the
maintainer holds two copies of one document — an **anchor** (state at session
open) and a **working copy** (state during the session). Edits go to the
working copy only. At session close, the diff between the two — and only the
diff — propagates to the reference files.

Why it matters: the naive "what changed since last session?" question forces
re-reading every reference file end-to-end. This costs tens of thousands of
tokens just to learn that nothing changed. The two-state model collapses that
to a diff over one file. Empty diff → zero further reads. Non-empty diff →
read only the targeted files.

The pattern is applicable beyond this skill — anywhere an agent's instruction
set needs to evolve across sessions and the cost of "scan everything" is
prohibitive.

### 2. The workflow contract (Claude edits, the human pushes)

Most agent tools either ask permission every step or quietly require token
storage. This skill draws the credential boundary explicitly: the agent's
sandbox has no GitHub credentials and never tries to acquire any. It edits,
stages, and commits inside the local project folder. The final `git push` is a
one-liner the agent hands to the human to paste into Terminal. The push
command for every registered project is pre-baked in `references/projects.md`
so the handoff is instant.

The same pattern works for any credentialed action: deploy commands, package
publishes, signing operations. The boundary is the boundary.

### 3. Multi-channel state verification with disagreement as signal

Every important repo state is readable through at least three independent
channels: direct observation (an existing instrument like a CI badge),
structural inference (the shape of the repo — commit cadence, branch count,
test/source ratio), and behavioural derivation (downstream responses to
change — build time, test failures, recompile counts). When all channels
agree, confidence is high. When two disagree, the disagreement itself is the
most diagnostic signal in the system.

This idea has formal grounding in industrial engineering literature
(degradation-rate-interaction modelling, failure-propagation hypergraphs).
Applying it to repos is the contribution.

### Sharper reframing of existing practice

| Concept                              | Source                                            | What this skill adds                                  |
|--------------------------------------|---------------------------------------------------|-------------------------------------------------------|
| Intent-annotated changes             | Cederbladh et al. 2025 (systems engineering)      | A structured INTENT/TRIGGER/DECISION block on every PR description. |
| Inspection rotation                  | Standard ops practice                             | Tiered cadence across *every* cluster, not just hot areas — proactive surfacing of latent issues. |
| Change topology beyond `git log`     | Multiple cited papers (full list in `change-topology.md`) | A four-layer model — structure, dynamics, management, 4D view — wrapped around git, not replacing it. |
| Project registry                     | Common in CI/CD config                            | Per-project local path + remote + push command baked in, eliminating per-action permission asks. |

### What's not actually distinctive

The 80-node cluster catalogue in `references/nodes.md` is an inventory of
plugin capabilities that exist elsewhere; it doesn't itself constitute a
system. It's the menu the methodology cooks from, not the methodology itself.

---

## Compared to alternatives

| Alternative | What it does | Why this is different |
|-------------|--------------|-----------------------|
| GitHub Copilot / Cursor / Windsurf  | Pair-programming assistants — help you write code in an IDE. | These are completion-oriented. This skill is *maintenance-oriented*: it operates on a repo's overall health, not just the line under your cursor. |
| Dependabot / Renovate               | Automated dependency updates. | Narrow scope. This skill subsumes the dependency-update workflow inside a broader rotation that also covers docs, branches, security posture, and architecture drift. |
| Claude/ChatGPT with manual prompting | General-purpose AI you re-prime each session. | You re-explain context every time. This skill is the persistent instructions — once installed, every session inherits the methodology and the project registry. |
| Project-management tools (Linear, Jira) | Issue tracking and team workflow. | These track work; this skill *does* work. They complement, they don't overlap. |
| Bash scripts in a `Makefile`        | Local automation. | Scripts handle deterministic operations. This skill handles judgment-laden operations — code review, intent annotation, surfacing problems that aren't yet in any tracker. |

---

## Installation

This is a [Cowork](https://claude.ai) skill for Claude. Two ways to install:

1. **From a built `.skill` bundle** (when releases exist) — download
   `repo-maintainer.skill` from the
   [latest release](../../releases/latest) and import it via Cowork's
   plugins panel.

2. **From source** — clone this repo, then point Cowork at the directory:

   ```bash
   git clone https://github.com/acooperrye/repo-maintainer.git
   ```

   Then add the skill from the local path in Cowork's plugin settings.

After installation, set up the project registry by editing
`references/projects.md` with your own repos — local paths, GitHub remotes,
push commands. The first time you ask Claude to push a project, Claude will
add it for you and you won't have to specify the remote again.

---

## How to use it

Once installed, the skill activates whenever you mention repo work — commits,
PRs, releases, dependencies, CI, branches, push, deploy. Typical patterns:

- *"Push this to GitHub"* → the agent looks up the project in the registry,
  stages and commits, hands you the one-line push command.
- *"What's in flight across my repos?"* → the agent runs the every-session
  rotation and surfaces open PRs, failing CI, stale branches, security
  alerts.
- *"Set up a new repo for &lt;project&gt;"* → the agent creates the local
  scaffold (README, LICENSE, CONTRIBUTING, SECURITY, `.gitignore`, CI),
  hands you the GitHub-side actions (repo create) as a handoff, registers
  the project, then hands you the first push command.
- *"Review the dependency graph for drift"* → the agent runs the
  structural-fingerprinting layer and reports new edges, removed edges, and
  growing coupling.

The agent surfaces findings proactively. You don't have to remember to ask
about stale branches or dependency drift — the rotation runs whether you
mention it or not.

---

## License

Dual-licensed:

- **Code and functional components** — MIT (see `LICENSE`).
- **Architecture, design, and written prose** — CC-BY-SA 4.0.

The CC-BY-SA license on the prose means you're welcome to fork, adapt, and
build on the methodology — provided you attribute the source and license
your adaptations under the same terms.

---

## Contributing

See `CONTRIBUTING.md`. The short version: open an issue if you find a node
that's misclassified or a methodology gap; open a PR if you have a concrete
improvement. The methodology evolves; that's the point. What doesn't change
without good reason is the workflow contract — that's the load-bearing wall.
