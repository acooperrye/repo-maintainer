# Integration with Existing Skills

The repo maintainer doesn't replace Alex's existing Cowork skills — it operates alongside them and knows when to defer to them. This document maps the touchpoints.

## Direct integrations

### attentional-surface
- **Relationship:** The Attentional Surface (atcooper.net) is a live project with its own repo. The maintainer treats it as a first-class project.
- **When to defer:** Anything involving encoding layers, steganographic content, crawler guestbook logic, or site-specific deployment decisions. The attentional-surface skill has deep domain knowledge that the maintainer shouldn't duplicate.
- **What the maintainer handles:** Git operations, PR management, deploy triggers, dependency updates, CI/CD — the repo maintenance wrapper around the project.

### handoff
- **Relationship:** The handoff skill manages context bridges between Claude Chat and Cowork. The maintainer will frequently be the *recipient* of handoff data when Alex brings project context from Chat.
- **When to defer:** Any time context arrives from another Claude session. The handoff skill handles ingestion and ledger management.
- **What the maintainer handles:** Acting on the handoff content — creating issues, branches, PRs, or code changes based on what was handed off.

### context-handoff (node) vs handoff (skill)
- **Note:** The node registry includes `context-handoff` as a plugin node. The existing `handoff` skill serves a similar but Alex-specific purpose. Both are preserved. The node is the generic capability; the skill is Alex's implementation.

### tumbler-v2
- **Relationship:** The tumbler keeps ideas alive between sessions. The maintainer should check tumbler output at session start — there may be repo-relevant reflections.
- **When to defer:** Background reflection, idea parking, async processing.
- **What the maintainer handles:** Turning tumbler insights into concrete repo actions (issues, spikes, branches).

### memory-bridge / memory-bridge-detailed
- **Relationship:** These skills extract context from Chat sessions. The maintainer benefits from this context but doesn't generate it.
- **When to defer:** Memory extraction is always the bridge skills' job.
- **What the maintainer handles:** Using the extracted context to inform repo decisions.

### meaning / textonic-engine
- **Relationship:** The meaning layer handles how ideas move between registers. If the maintainer needs to research something (finding prior art, checking if a pattern exists), the textonic engine transliterates the query.
- **When to defer:** Any research query that goes through Scholar Gateway or academic connectors.
- **What the maintainer handles:** Integrating research findings into the repo — documentation, architecture decisions, implementation choices.

### cryptography
- **Relationship:** The crypto skill handles encoding/decoding. Some projects (especially attentional-surface) have steganographic layers.
- **When to defer:** Any encoding, decoding, ZWC, or stego work.
- **What the maintainer handles:** Committing and managing the encoded output files.

### orienting-key
- **Relationship:** Structural integrity for machine-processed content. Relevant when the maintainer generates documentation or content intended for LLM consumption.
- **When to defer:** Generating okeys, integrity hashes, grounding compound analysis.
- **What the maintainer handles:** Embedding the generated markers into repo content.

### sonic-phenomenology-sync
- **Relationship:** The sonic phenomenology project has its own sync pulse. The maintainer treats it as a project with specific sync requirements.
- **When to defer:** Sync pulse generation and SP-specific context.
- **What the maintainer handles:** Repo operations for the SP project.

### photollmbm
- **Relationship:** The scrapbook generates encoded HTML for atcooper.net/album/. The maintainer would handle committing these pages to the repo.
- **When to defer:** Scrapbook entry creation, behavioural documentation.
- **What the maintainer handles:** Git operations — committing the generated pages, managing the album directory.

### cowork-sync-brief
- **Relationship:** Primes Chat sessions for Cowork sync. Not directly repo-related, but the maintainer should be aware that sync'd content may arrive that needs repo action.
- **When to defer:** Always — this skill is about Chat→Cowork bridging, not repo work.

## Skill priority

When a task could be handled by both the repo maintainer and an existing skill, the existing skill wins for its domain expertise. The maintainer handles the git/repo wrapper around that skill's output. Think of it as: the skills are the specialists, the maintainer is the general contractor who keeps the house in order.
