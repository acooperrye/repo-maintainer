# GitHub Official Standards

Distilled from GitHub's own documentation on repository best practices, security, branching, community health, and file management. This is the baseline — what GitHub itself says a well-maintained repo should have. The repo-maintainer treats this as the floor, not the ceiling.

Source: https://docs.github.com/en/repositories/creating-and-managing-repositories/best-practices-for-repositories and all linked pages.

---

## The Checklist: What Every Repo Needs

### 1. README.md
- Lives in root, `.github/`, or `docs/` (GitHub checks in that order)
- Answers: what is this, why does it exist, how do I use it, how do I contribute, who maintains it
- GitHub auto-generates a table of contents from section headings
- Truncated at 500 KiB — keep it under that
- Use relative links, not absolute, so they work across branches and clones
- Longer documentation goes in wikis, not READMEs

### 2. LICENSE
- Must be per-repo — cannot be set as a default across an org
- Without one, default copyright applies and nobody can legally use the code
- This is the one file GitHub says cannot be inherited from a `.github` defaults repo

### 3. CONTRIBUTING.md
- Lives in root, `.github/`, or `docs/`
- GitHub surfaces a link to it when someone opens a PR or issue
- Should include: how to submit issues, how to submit PRs, coding standards, testing expectations, and what gets rejected
- Priority order when multiple exist: `.github` → root → `docs`

### 4. CODE_OF_CONDUCT.md
- Defines community engagement standards
- Can be set as an org-wide default in the `.github` repo

### 5. SECURITY.md
- Instructions for reporting vulnerabilities
- Should include: supported versions, reporting process, expected response time
- Lives in root, viewable through the repo's Security tab
- Can also be set as an org-wide default

### 6. SUPPORT.md
- Where to get help (docs, forums, email, etc.)
- Prevents the issue tracker from becoming a help desk

### 7. FUNDING.yml
- Displays a sponsor button
- Optional but GitHub supports it natively

### 8. GOVERNANCE.md
- Documents how decisions are made in the project
- Who has commit access, how disputes are resolved, how maintainers are chosen

---

## Security: What GitHub Recommends Enabling

### Free for all public repos:

**Dependabot alerts** — notifies when dependencies have known vulnerabilities. Enable in Settings → Security.

**Secret scanning** — detects API keys, tokens, and credentials committed to the repo. Alerts the maintainer when found.

**Push protection** — blocks pushes that contain detected secrets BEFORE they reach the repo. This is prevention, not detection.

**Code scanning (CodeQL)** — static analysis that identifies vulnerabilities in the code itself. Default setup auto-detects languages and sets scan triggers. Advanced setup allows custom workflows.

### Recommended practices:

- Enable **Dependabot security updates** for automatic PRs that fix vulnerable deps
- Enable **Dependabot version updates** via `dependabot.yml` to keep deps current
- Configure **auto-triage rules** to automatically handle low-priority alerts
- Use **private security advisories** to discuss and fix vulnerabilities before public disclosure
- Enable **dependency review** on PRs so you can see what dependency changes a PR introduces before merging

---

## Branching Strategy

### GitHub's core recommendation:
"Favor branching over forking." Regular collaborators should work from a single repository, creating PRs between branches rather than between repos.

### Protected branches:
- Force pushes disabled by default
- Deletion disabled by default
- Can require: PR reviews, status checks, signed commits, linear history, conversation resolution, successful deploys
- Only one branch protection rule applies at a time (use rulesets for layered rules)
- Admins are exempt unless "do not allow bypassing" is enabled

### Rulesets (GitHub Team/Enterprise):
- Named collections of rules that can apply to branches, tags, or all pushes
- Multiple rulesets can apply simultaneously (unlike branch protection rules)
- Most restrictive rule wins when rulesets overlap
- Can be enabled/disabled without deletion
- Can restrict: file paths, file extensions, file sizes, commit metadata
- Push rulesets apply across the entire fork network
- Up to 75 rulesets per repo, 75 org-wide

### Key difference between branch protection and rulesets:
Branch protection: one rule at a time, admin-only visibility, binary on/off.
Rulesets: multiple simultaneous, read-access visibility, enable/disable states.

---

## File Size Management

### Hard limits:
- 100 MiB: blocked entirely — GitHub will not accept it
- 50 MiB: warning on push
- 25 MiB: max for browser upload

### Repository size:
- Recommended: under 1 GB
- Hard guidance: under 5 GB
- Oversized repos trigger outreach from GitHub Support

### Git LFS (Large File Storage):
- Stores pointer files in the repo, actual files in separate storage
- Pointer files contain: LFS version, unique file ID (oid), file size
- Max file size varies by plan: 2 GB (Free/Pro), 4 GB (Team), 5 GB (Enterprise)
- Cannot be used with GitHub Pages or template repos

### Large file alternatives:
- GitHub Releases: up to 2 GiB per file
- External file-sharing services for databases and large binaries
- Use `git filter-repo` to remove large files from history if accidentally committed

---

## Community Health Defaults

An organisation (or personal account) can create a **public** repo called `.github` to hold default community health files. These apply to every repo in the org that doesn't have its own version.

### Supported defaults:
CODE_OF_CONDUCT.md, CONTRIBUTING.md, discussion category forms, FUNDING.yml, GOVERNANCE.md, issue/PR templates, SECURITY.md, SUPPORT.md

### NOT supported as defaults:
LICENSE (must be per-repo)

### Discovery order (per repo):
`.github/` folder → root → `docs/`
If nothing found in the repo, falls back to the org's `.github` repo.

---

## Issue and PR Templates

### Issue templates:
- Live in `.github/ISSUE_TEMPLATE/` on default branch
- Can be Markdown (`.md`) or YAML forms (`.yml`)
- Require `name:` and `about:` (md) or `name:` and `description:` (yml)
- Template chooser customised via `config.yml` in the same directory
- Issue forms allow structured web form fields (dropdowns, checkboxes, text areas)

### PR templates:
- Live in root, `docs/`, or `.github/`
- Auto-populate the PR body when a new PR is opened
- Can be `.md` or `.txt`

---

## What This Means for the Repo-Maintainer

The maintainer's **new repo setup** checklist should produce:

1. README.md — what, why, how, who
2. LICENSE — appropriate for the project
3. CONTRIBUTING.md — how to participate
4. SECURITY.md — how to report vulnerabilities
5. .gitignore — language/framework appropriate
6. CI config — GitHub Actions workflow for tests and linting
7. Branch protection on main — require PR reviews, require status checks
8. Dependabot enabled — alerts + security updates + version updates
9. Secret scanning + push protection enabled
10. CLAUDE.md — project-specific instructions for future Claude sessions

For Alex's projects specifically:
- Enable push protection (prevents accidental credential commits)
- Use rulesets if available (layered rules, better visibility)
- Keep repos under 1 GB (the site assets for atcooper.net need monitoring)
- Issue templates with structured forms for bug reports and feature requests
- PR template that includes an intent annotation field (ties into change-topology.md)

The maintainer handles items 1-9 autonomously. Item 10 is the letter to the next amnesiac self.
