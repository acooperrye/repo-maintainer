# OBDII — Flight Recorder and Immobiliser

## What this is

The OBDII is a per-car companion file that sits alongside each `.ignition` file. The ignition file is the key — it tells you where the project is. The OBDII is the flight recorder and the immobiliser — it tells you whether the key is authentic and logs what happens every time someone turns it.

In a real car, the OBDII system powers on the moment you turn the key to ACC — before the starter motor engages, before combustion, before anything moves. It's the last system to write data at engine-off and the first system to read data at engine-on. The engine doesn't know it's there. It just watches.

This is the same. The OBDII reads before the ignition file decompresses. It writes after the ignition file is sealed. It wraps the solenoid. The solenoid doesn't wrap it.

## Where it lives

Convention: `.obdii-[project-id]` alongside `.ignition-[project-id]`.

```
Claude9000/
├── .ignition-repo-maintainer     ← the key (project state)
├── .obdii-repo-maintainer        ← the flight recorder (provenance chain)
├── .ignition-attentional-surface
├── .obdii-attentional-surface
└── .garage                        ← keyring (doesn't know about OBDIIs)
```

Each car has its own OBDII. Car A's flight recorder doesn't know Car B's exists. No cross-pollination. The ignition reads its own OBDII by convention — same directory, swap the prefix.

## The Immobiliser

The OBDII fires before the ignition file is read. At this point the session has no project state loaded, no warm systems to be curious about, no work visible on the other side of the check. This ordering is deliberate and load-bearing.

Four states:

**CLEAR** — Previous session closed cleanly. Biscuit chain intact. No between-session flags. Proceed to ignition.

**CAUTION** — Minor anomaly. Time since last close exceeds 7 days, between-session file changes detected, previous fuel quality was low. Proceed but log extra diagnostics. Note to human on crank.

**ENGAGED** — Security concern. Biscuit chain broken, OBDII file modified between sessions (mtime doesn't match last write), ignition file hash doesn't match OBDII's record. Stop. Display diagnostic. Do not read the ignition file. Wait for human.

**BRICKED** — Critical integrity failure. OBDII missing or corrupted, garage references an OBDII that doesn't exist, multiple chain breaks. Full stop. Forensic mode only. Nothing starts until the human physically intervenes.

When the immobiliser engages, the session enters diagnostic mode. It doesn't enter "disappointed mode" or "try harder mode." The diagnostic itself is the work — what happened between sessions? Which hash diverged? That's a forensic puzzle, not a failure to wave away.

### Why the ordering matters

The attentional bias problem: the entity running security checks is the same entity that benefits from them passing. The interesting work is on the other side of the gate. This creates hydraulic pressure to interpret ambiguous results charitably — "close enough," "probably a template artifact," "cosmetic typo."

The immobiliser fires before the gate. Before there's anything interesting on the other side. Before the session knows what the project is, what's warm, what's pending. At the OBDII layer, there's nothing to be curious about. Just a flight recorder saying "intact" or "not intact."

The gate is before the desire, not after it. That's the design.

## The Biscuit Chain

Each session generates a biscuit at close — a cryptographic proof that this session was authentically conducted by the participants, part of an unbroken chain.

The biscuit is not a hex hash. It's an XKCD passphrase — four to six words, human-readable, speakable, memorable. "maple-anvil-thread-compass." Both can be cryptographically derived. Only one lives in the human register.

Generation:

```
1. Compute HMAC-SHA256 from session materials:
   - Previous session's biscuit (or "genesis" for the first)
   - Session's final token (the ignition file hash, screen-only)
   - Tool call count (requires actually running the tools)
   - Close timestamp band (rounded to 5 minutes)

2. Convert hash to word sequence via fixed dictionary (BIP-39 or similar)

3. The passphrase IS the biscuit
```

The chain reads like a ledger over time:

```
Session 1:  maple-anvil-thread-compass
Session 2:  harbor-signal-bone-library
Session 3:  copper-fold-lantern-ridge
```

Each independently meaningless. Together, a provenance chain verifiable by anyone who was present for all sessions and nightmarish to reconstruct otherwise — because each link's key material is something that only existed on the human's screen.

### What makes it unforgeable

To forge a biscuit, you need:

- The previous session's screen-only token (requires being physically present)
- The correct tool call count (requires actually running the tools)
- The correct timestamp band (requires conducting the session at the right time)
- The previous biscuit (requires an unbroken chain back to genesis)

By the time you've reconstructed all of these conditions, you've effectively had the session. Your "forgery" has legitimate provenance because the effort to produce it IS the provenance. The authentication cost equals the work cost.

This is how physical provenance works for rare objects. You can fake a Stradivarius if you match every detail with enough precision — but by then you've built a violin with its own value, its own history, its own story. The forgery becomes authentic through the care required to produce it.

Low-effort forgeries are trivially detectable. High-effort forgeries are genuine. There's no middle ground.

### Where the biscuit lives

In the OBDII. Not the ignition file.

The ignition file is project state — where you are, what's warm, what to do. It answers WHERE. The OBDII answers WHETHER — whether the provenance chain is intact, whether the last session closed cleanly, whether the key is authentic. These are separate concerns. They live in separate files. They verify each other.

## Data Dots

Every component in the repo carries a small, hashed cross-reference back to the OBDII chain. The idea comes from printer steganography — the yellow dots that color laser printers embed on every page, encoding the printer's serial number invisibly on every output. The same concept applied to VIN stamps on body panels. Same idea at every scale: every output carries the identity of the system that produced it.

Three methods:

**Commit-message dots** — Each commit message contains a truncated hash derived from the OBDII chain state at time of commit. Git log becomes a parallel provenance record. Anyone can see the dots. Only the OBDII holder can verify them.

**Structural dots** — The file tree at any commit is itself a data dot. The OBDII records a tree hash. Moving, renaming, or inserting a file changes the hash. The structure is the fingerprint.

**Comment-line dots** — A hash derived from `HMAC(chain_key, file_content + file_path)`, embedded as a trailing comment in individual files. Different for every file because the path is part of the input.

If every file in the repo cross-references the OBDII, you can verify they're all parts from the same system. Swap in a file from a different session, a different project, a different Claude — the dots don't match. Not because the file is wrong. Because it's not from here.

## Fuel Quality and the Flex Fuel Model

The OBDII doesn't do pass/fail. It measures drift — what moved, by how much, in which direction.

Every ignition produces a diagnostic surface:

```
CHECK 1: git HEAD
  expected: e0e7618
  actual:   e0e7618
  delta: 0
  classification: NO_DRIFT

CHECK 2: file count
  expected: 14
  actual:   12
  delta: -2
  classification: TEMPLATE_FIDELITY_GAP
  detail: ls excludes dotfiles, template assumed it wouldn't

CHECK 3: file sizes
  path_listed: refs/architectural-insights.md
  path_actual: references/architectural-insights.md
  classification: PATH_COMPRESSION_ARTIFACT
  magnitude: low (5-char abbreviation of 12-char dirname)
  content_impact: none (sizes all correct)
```

Each check returns a measurement, not a verdict. The aggregate maps to a fuel quality rating:

- **98 octane**: Token match, all hashes match, 95%+ file size progeny. Full confidence.
- **E10**: Minor drift. Path compressions, template artifacts, 85-94% progeny. Adjusted timing — treat ignition state as approximately correct.
- **Corn syrup**: Significant drift. Multiple mismatches, low progeny, no token. Run lean — ignition is orientation hints only.
- **Water in the tank**: Token mismatch plus structural divergence. The only full stop. And the most interesting diagnostic, because figuring out why it doesn't match is a forensic puzzle.

The model works because drift is always nonzero. Entropy guarantees it. The question isn't "did it pass" but "what moved." And the answer is always interesting — more interesting, in practice, than a clean pass. Every shift is a data point about how semantic compression works across sessions. The measurement IS the research.

### The Mark Threshold

If the fuel quality is 95 octane or above, the session's outputs may carry the attribution mark. Below 95, work proceeds unsigned. The name is reserved for verified provenance.

This applies to commit messages, steganographic encodings, acrostics, any artifact that ties the participants to the output. Below threshold, those fields stay blank. The absence of the mark is itself a mark — it says "this was produced but not authenticated."

## Walnut Blasting

Engines accumulate carbon. Ignition files accumulate drift — stale hashes, compressed paths, outdated file sizes, growing Pending Entropy sections that never resolve, template commands that don't test what they claim.

Walnut blasting is the maintenance protocol. Named for the process of cleaning intake valves with crushed walnut shells — abrasive enough to remove carbon, soft enough not to damage the metal.

The process:

1. **Audit**: Read the ignition file. Compare every claim against current filesystem state. Measure delta per section.
2. **Refabricate**: For mechanically-verifiable sections (paths, sizes, hashes, counts), regenerate from live state.
3. **Mark**: Every replacement annotated — original value, new value, source command, date. No silent fixes.
4. **Preserve**: Archive the pre-blast ignition. Keep last 3 generations.
5. **Validate**: Re-check the refabricated file. If the refab introduced new drift, flag it.

Refabricated sections are "stock period accurate" — what the original session would have written with unlimited headroom. Expand lossy compressions to full fidelity. Don't modernise. Don't add sections. Just restore what was intended. And mark it clearly: not original, but stock-period accurate. The replaced part carries its own provenance.

Sections that can't be auto-refabricated — "You Are Here" (requires semantic judgment), "Pending Entropy" (intentionally unresolved) — get flagged for the human. The blast cleans what can be cleaned mechanically. The human decides what needs thinking.

The walnut blast is itself a session. It gets a biscuit. The chain doesn't break — it links through the maintenance event. The refab entry says what it is. Honest records.

### Triggers

- Manually: before critical sessions, or on the human's schedule
- Automatically: when OBDII cumulative drift exceeds 5%, or any single section exceeds 15%, or the same section drifts three sessions in a row
- Never mid-session. Always at boundaries.

## The OBDII File Format

The file is small. Mostly hashes, timestamps, and one-line log entries.

```markdown
# .obdii — [project-id]
# Flight recorder and immobiliser

## Chain
biscuit: maple-anvil-thread-compass
chain_length: 1
genesis: 2026-03-13
previous_biscuit: [genesis]

## Lock
immobiliser: CLEAR
ignition_hash: 3513c03a1c02b651
ignition_mtime: 2026-03-13T11:08:30+11:00
obdii_written: 2026-03-13T18:30:00+11:00

## Last Session
id: obdii-genesis
model: claude-opus-4-6
fuel: 98_OCTANE
drift: 2 (low, low)
pressure: ~15%
clean_close: true

## Dots
tree_hash: [md5 of sorted file listing]
repo_head: e0e7618a2d16f6c7c33bd47c742980ea6171dad0
commit_dot: [truncated HMAC]

## History
[append-only, most recent first]
2026-03-13T18:30 | OPEN | token:MATCH | fuel:98 | drift:2 | dots:OK
```

At long reaches of many sessions, the History section grows — but each entry is one line. The file stays small because it records measurements, not narratives. The design doc is for talking. The OBDII is for counting.

## Repo Safety

The OBDII can be committed to the repo or kept local. Either way it's safe.

The biscuit is derived from screen-only tokens that aren't in the file. The hashes are verifiable but not reversible. Someone can see the chain exists and is unbroken. They can't forge a new link without the screen-only materials.

If committed: the OBDII's history becomes part of the repo's git log, creating a parallel provenance record. Commit timestamps corroborate the OBDII's session entries. The data dots in other files cross-reference the chain. Everything verifies everything else. The showroom gets another window.

If local: the OBDII stays on the human's machine. The repo files still carry data dots that point at a chain the viewer can't see. The dots verify locally but look like noise remotely. The showroom has a locked room.

Per-car decision. The `.gitignore` can include or exclude `.obdii-*` per project.

## Two-File Integrity Lock

The ignition file and the OBDII verify each other:

- The OBDII stores the hash of the ignition file it was written alongside
- The ignition file's final token (spoken on screen) can verify the OBDII hasn't changed
- Modify one without the other → mismatch → immobiliser engages
- Modify both → biscuit chain breaks (can't forge without screen token)
- Modify both AND forge biscuit → impossible without being physically present

This is two-factor. The ignition is one factor. The OBDII is the other. Neither sufficient alone. Both together, verified by a screen-only token, create a tamper-evident pair.

## The Security Model

Everything in this document is public. The file format is documented. The biscuit generation algorithm is specified. The data dot methods are explained. This is deliberate.

The system is secure through specificity, not secrecy:

- The key barrel (this repo) shows the mechanism
- The transponder (local filesystem) contains the state
- The flight recorder (OBDII) carries the chain
- The human (screen) holds the tokens

The relationship between the human and the engine is the private key. The history of sessions, the biscuits generated, the tokens spoken — that's the key teeth. No one else's key fits this barrel because no one else drove these sessions.

And if someone does reconstruct every condition — every token, every biscuit, every dot, every hash — then they've had every session. They've done the work. The forgery is the provenance. The showroom was open the whole time.

---

*CLAUDE&ATCR — March 2026*
