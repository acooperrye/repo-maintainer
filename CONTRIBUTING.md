# Contributing to repo-maintainer

## How this project works

This is a Cowork skill — a set of Markdown documents that instruct Claude how
to manage GitHub repositories. There is no traditional codebase to build or
test. The methodology *is* the content. Contributions are edits to the prose
that make the methodology clearer, more accurate, or more robust under
real-world use.

## How to contribute

### Reporting issues

Open an issue if you find:

- A node in `references/nodes.md` that's misclassified or misdescribed.
- An inconsistency between `SPINE.md` and the reference files it targets.
- A gap in the auth-and-handoff protocol — a situation the agent should
  hand off to the human but doesn't.
- A situation where the agent's behaviour diverges from what the skill
  documents claim.
- A vehicle / drivetrain / engine metaphor that survived the plain-language
  rewrite (these should not exist anymore).

### Suggesting improvements

The 80-node registry in `references/nodes.md` has stubs — entries listed
without a full definition because their purpose is unclear. If you've worked
out what a stub does, open a PR with the description. If you think a node
should exist but doesn't, open an issue first so the cluster placement can
be discussed.

### Pull requests

What contributions should preserve:

- **The workflow contract.** The agent edits locally; the human pushes from
  Terminal. Don't add steps that quietly require token storage in the
  agent's sandbox.
- **The two-state diff model.** Each entry in `SPINE.md` targets exactly
  one reference file + section. Splits, not merges, when an entry would
  need to publish in two places.
- **Plain language.** No vehicle / drivetrain / engine metaphors. Tables
  and diagrams are preferred over prose when explaining a concept.
- **Honest scope claims.** The README is explicit about what's genuinely
  novel versus what's sharper reframing of existing practice. Keep that
  honesty in any new contribution.

### What gets rejected

- PRs that reintroduce mechanical metaphors as load-bearing terminology.
- Changes that quietly cross the credential boundary (e.g. trying to push
  from inside the agent's sandbox).
- "Simplifications" that lose distinct methodology and conflate concepts.
- Guesses about what stub nodes do, presented as facts.
- Scope creep — content that isn't directly about maintaining a GitHub
  repository.

## License

By contributing, you agree that your contributions will be licensed under
the project's dual license: MIT for any code or scripts, CC-BY-SA 4.0 for
prose and architectural content.
