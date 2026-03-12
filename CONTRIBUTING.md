# Contributing to repo-maintainer

## How this project works

This is a Cowork skill — a set of markdown documents that instruct Claude how to manage GitHub repositories. There's no traditional "codebase" to build or test. The architecture *is* the content.

## How to contribute

### Reporting issues
Open an issue if you find:
- A node in the registry that's misclassified or misdescribed
- An architectural inconsistency (a vertebra pointing to the wrong quadrant)
- A gap in the auth handoff protocol
- Something that doesn't work when installed as a Cowork skill

### Suggesting improvements
The 80+ node registry has stubs — nodes whose purpose is unclear or speculative. If you figure out what one does, open a PR. If you have a node that should exist but doesn't, open an issue first.

### Pull requests
- Keep the node architecture discrete. Don't merge nodes, don't abstract them, don't "simplify" the structure.
- Stubs are valid. Don't fill them in by guessing.
- The mechanical metaphors (drivetrain, differential, torque vectoring) are load-bearing, not decorative. Contributions should work within the metaphor, not around it.
- One vertebra, one quadrant. Always.

### What gets rejected
- PRs that flatten the node architecture
- Changes that break the spine's differential model
- "Simplifications" that lose structural information
- Guesses about what stub nodes do

## License

By contributing, you agree that your contributions will be licensed under the project's dual license (MIT for code, CC-BY-SA 4.0 for architectural content).
