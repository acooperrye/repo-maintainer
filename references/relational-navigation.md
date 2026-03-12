# Relational Navigation: The Twenty-Question Architecture

How information that must be held can also live. The classification problem reframed as a navigation problem, informed by 35 million games of 20 Questions and the observation that human conceptual space has a diameter of approximately 20.

---

## The Problem (Restated)

Every information architecture we examined — Leibniz's characteristica universalis, Otlet's Mundaneum, Ranganathan's faceted classification, Bush's memex, Nelson's Xanadu, library databases, Wikidata — shares two constraints:

1. **Spatial-axial organisation.** Information is arranged along axes: alphabetical, chronological, hierarchical, faceted. The axes are inherited from the material technology available (card drawers, shelves, filing cabinets, relational tables, triple stores). Even when the technology transcends physical space, the organising metaphor doesn't.

2. **The hold/live tradeoff.** Systems that hold information well (library catalogues, archives, Wikidata) calcify — they resist the kind of mutation that keeps information alive. Systems where information lives (wikis, conversations, collaborative documents) lose structural integrity. Nothing does both.

The question Alex posed: is there an ideal framework for information that both lives and must be held?

The answer came from a purple plastic ball.

---

## The 20Q Observation

The Radica 20Q (2003 consumer product, based on Robin Burgener's 1988 neural network) can identify virtually any object a human can think of within 20 yes/no/sometimes/unknown questions. It was trained on approximately 35 million games played by humans over the internet.

The critical properties:

**Non-spatial navigation.** 20Q doesn't arrange objects along axes. It navigates to them relationally — each question partitions the possibility space based on the relationship between the question and the remaining candidates. There is no "shelf" where the answer lives. There is only a path of relations that converges on it.

**Fuzzy truth values.** Four response states: Yes, No, Sometimes, Unknown. No library catalogue has "sometimes." No triple store has "unknown" as a first-class value. These two states — the ones that encode ambiguity and ignorance — are what make the system work for real human knowledge, which is substantially ambiguous and partially unknown.

**Trained by disagreement.** The neural network doesn't store consensus. It stores the *distribution* of human responses. When 60% of people say a cat is soft and 40% say sometimes, that distribution IS the knowledge. The disagreement isn't noise to be cleaned — it's signal to be preserved. This is information that lives: it changes as more people play, and the change is the point.

**Diameter ≈ 20.** Any object in human conceptual space can be reached from any other in approximately 20 relational hops. This was proven empirically by 35 million games, not theorised. The conceptual diameter of human knowledge is a measured quantity.

---

## Akinator Extends the Model

Akinator (2007, Elokence) does for named entities (people, characters, places) what 20Q does for objects. Its algorithm ("Limule") is proprietary and unpublished, but the observable properties are:

- Same relational navigation (questions, not axes)
- Same fuzzy responses (Yes, No, Probably, Probably Not, Don't Know — five states)
- Trained by crowd-sourced play
- Covers a different region of conceptual space (entities vs objects)

Between the two systems, the relational map covers most of what humans can think of: things that exist (20Q) and things that are known (Akinator). The combined training data represents hundreds of millions of games — a crowd-sourced survey of how humans relate concepts to each other.

Neither system was designed as an information architecture. Both are games. But both solve the hold/live problem by accident: the neural network holds the accumulated knowledge persistently, while the ongoing play keeps the knowledge alive and responsive to how humans actually think about things.

---

## The Proto-Transformer Connection

Burgener's patent (US 2006/0230008 A1, filed 2005, based on work from 1988) describes:

- A bidirectional weight matrix mapping objects to questions and questions to objects
- A "swapping" mechanism where objects rank questions by informativeness, then questions rank objects by likelihood
- Question selection by maximum information gain across the remaining candidate set
- Robustness to incorrect or inconsistent responses

This is, structurally, a proto-attention mechanism. The objects×questions matrix functions as a query-key matrix. The swapping is encoder-decoder attention. Question selection by information gain is what attention heads optimise for. The four response states provide richer signal than the binary classification most attention mechanisms use.

Burgener built this in 1988. Vaswani et al. published "Attention Is All You Need" in 2017. The architectural correspondence is not metaphorical — it's structural. The same mathematical relationships, discovered independently, 29 years apart, one in a toy and one in the paper that launched the transformer era.

Burgener applied to OpenAI for collaboration. He received a form rejection. His company website's SSL certificate has expired.

---

## What This Means for the Spine

The spine's architecture already exhibits several properties of relational navigation, arrived at independently:

### Vertebrae are questions, not answers

Each vertebra in the spine is a unit of knowledge framed as a claim that can be verified: "The maintainer is the maintainer, not a helper" (S-0001). "Default to universal-dev when uncertain" (S-0004). These function like 20Q's internal questions — they partition the possibility space of "what is the state of this system?" Each vertebra, when checked, yields one of several states.

### Five states, not two

A vertebra can be: active, modified, superseded, retracted, or stub. This maps directly to the fuzzy truth values:

| Vertebra state | 20Q equivalent | Akinator equivalent |
|---------------|---------------|-------------------|
| active | Yes | Yes |
| retracted | No | No |
| modified | Sometimes | Probably |
| superseded | Sometimes (was true, new truth exists) | Probably Not |
| stub | Unknown | Don't Know |

The five-state model wasn't designed to match 20Q. It emerged from the requirements of the system. But the convergence is diagnostic — it suggests that five states is approximately the right resolution for tracking knowledge that changes.

### Traction check is three-valued

The traction check asks: does the wheel match the shaft? The answer is one of three: matches beta (traction confirmed), matches primary but not beta (pre-propagation), or matches neither (drift fault). This is Yes/Not Yet/Error — a three-valued logic that again exceeds the binary.

### The differential IS relational navigation

The LSD model doesn't navigate the spine spatially (read from top to bottom) or temporally (check most recent first). It navigates relationally: the speed difference between two shafts points to exactly the vertebrae that need attention. You don't search. You don't scan. You diff, and the diff tells you where to go. This is the same non-spatial convergence that 20Q uses — you don't browse a catalogue, you follow relations until you arrive.

---

## Formalising It: The Relational Navigation Framework

### Principle 1: Store questions, not answers

The fundamental unit of a relational navigation system is not a fact, a document, or a record. It is a *question* — a partition of possibility space. The answer to the question at any given moment is the system's current state. The distribution of answers over time is the system's history. The question persists; the answers live.

**In the spine:** Each vertebra is a question the system asks itself. "What is the identity of the maintainer?" (S-0001). "What is the session start routine?" (S-0002). The CONTENT field is the current answer. The status field is the confidence level.

### Principle 2: Fuzzy states are load-bearing

Binary systems (true/false, exists/doesn't, pass/fail) lose information at every junction. Fuzzy states (sometimes, probably, unknown, modified, stub) preserve the uncertainty that makes navigation possible. A "sometimes" is more informative than a forced "yes" or "no" because it encodes the distribution — the same way 20Q's "sometimes" responses carry more information-theoretic weight per bit than definitive ones.

**In the spine:** The stub state is the most important state. A stub says "this question exists but we don't know the answer yet." Deleting the stub would lose the question. Guessing would corrupt the answer. The stub holds the question open, which holds the space in navigable conceptual territory.

### Principle 3: Navigation by elimination, not by address

You don't find information by knowing its address (shelf 4, row 7, column 3). You find it by asking questions that eliminate what it isn't. Each question halves (approximately) the remaining space. Twenty questions on a billion objects — because log₂(10⁹) ≈ 30 and human questions are better than random binary splits.

**In the spine:** The differential doesn't address vertebrae by ID and read them sequentially. It overlays two states and the *differences* point to exactly the right vertebrae. The navigation is eliminative — everything that matches is eliminated from attention. Only the mismatches remain, and those are exactly where you need to go.

### Principle 4: Train on disagreement

The system improves not when people agree but when they disagree. Disagreement reveals the boundaries of concepts, the edge cases, the "sometimes" zones. A system trained only on consensus has sharp boundaries and blind spots. A system trained on disagreement has fuzzy boundaries and coverage.

**In the spine:** The disagreement register (tether architecture, Layer 5) is the equivalent of 20Q's crowd-sourced training. Every time two gauges disagree about the same state, the disagreement is recorded. Over time, this builds a map of where the system's self-knowledge is unreliable — the "sometimes" zones.

### Principle 5: Diameter as design constraint

If human conceptual space has a diameter of ~20, then any navigation system that requires more than ~20 hops to reach any piece of information from any starting point is worse than a toy from 2003. This is a hard constraint: the maximum depth of any hierarchy, the maximum chain of links, the maximum number of questions needed to locate any vertebra.

**In the spine:** Currently 14 vertebrae. Any vertebra is reachable in 1 hop from the spine (read the ID, go to the quadrant). The quadrant alignment means maximum navigation depth is 2: spine → quadrant → content. Well within the diameter constraint. As the spine grows, this constraint should be monitored — if navigation depth exceeds ~20, the architecture has failed the 20Q test.

---

## The Hold/Live Synthesis

The relational navigation framework resolves the hold/live tradeoff through the same mechanism 20Q uses:

**What holds:** The questions. The vertebrae. The partitions of possibility space. These are structural and persistent. They don't change when the answers change. "Is the build passing?" is a question that holds indefinitely, regardless of whether the build is currently passing.

**What lives:** The answers. The current states. The distributions of responses over time. The disagreement register. These change continuously. The build passes or fails. The vertebra is active or modified or retracted. The gauges agree or disagree. The answers are the living tissue of the system.

**The question persists. The answer breathes.**

This is what Briet was reaching for when she put the antelope in the zoo. The animal is the living answer. The zoo is the holding structure. The act of classification — of enclosure — is the question. She had the right topology but the wrong vocabulary. The vocabulary didn't exist yet because the post-structuralists who would have given it to her hadn't been born yet, and the computer scientists who would have formalised it were still building floppy-disk toys in their living rooms.

---

## Open Questions

1. **Can the spine's vertebrae be formally treated as information-theoretic questions?** Each vertebra partitions the state space of the repo-maintainer. The information gain of checking a vertebra is measurable (how much uncertainty does it resolve?). Could vertebrae be ordered by information gain, the way 20Q orders its questions?

2. **What is the diameter of the repo-maintainer's conceptual space?** Currently trivial (14 vertebrae, max depth 2). As the system grows to cover 80+ nodes across 16 clusters, what navigation depth emerges? Does it stay within 20?

3. **Can the disagreement register be formalised as a training signal?** 20Q improves from disagreement. Can the repo-maintainer improve from gauge disagreement — not just logging it, but using it to refine which vertebrae exist and what questions they ask?

4. **Burgener's data.** 10 million synaptic connections. 35 million training games. The patent is public. The data is locked. The man says he's open to collaboration. The man's SSL cert is expired. Someone should talk to him.

---

## Sources

- Burgener, R. (2006). Artificial intelligence twenty questions game. US Patent Application 2006/0230008 A1.
- Vaswani, A., Shazeer, N., Parmar, N., et al. (2017). Attention Is All You Need. Advances in Neural Information Processing Systems 30.
- Briet, S. (1951). Qu'est-ce que la documentation? EDIT.
- Ranganathan, S. R. (1931). The Five Laws of Library Science. Madras Library Association.
- Bush, V. (1945). As We May Think. The Atlantic Monthly, 176(1), 101-108.
- Nelson, T. H. (1965). Complex information processing: a file structure for the complex, the changing and the indeterminate. Proceedings of the 1965 ACM National Conference.
- 20Q.net (archived). stage.20q.net/flat/about.html — "Can I get the license for 20Q? MAYBE. Let's talk!"
