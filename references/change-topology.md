# Change Topology: Beyond the Flat Changelog

Research-informed design for how the repo-maintainer records and reasons about change.

## The Problem with Git Log

Git log is a changelog. It records what changed, when, and by whom. It preferences the time axis — most recent first, reverse chronological. This creates three blind spots:

1. **Spatial blindness** — You can see that `auth.ts` changed, but not where it sits in the system's dependency graph, what depends on it, or what it depends on.
2. **Propagation blindness** — You can see that the build broke, but not the causal chain from the originating change through to the failure point.
3. **Intent blindness** — You can see the diff, but not why the change was made, what decision it enacted, or what it was responding to.

## What Industrial Engineering Does Differently

### Layer 1: Structure (What connects to what)

The OSysRec ontology (Qasim et al., 2023, Systems Engineering) defines a structural layer that maps:
- Every component (resource) in the system
- The functions each component provides
- The connections between components

**Repo equivalent:** A dependency graph that goes beyond `package.json`. Every file, module, function, and data flow mapped as a graph. When something changes, you can see what it touches.

**Implementation:** The maintainer generates and maintains a system map for each repo. Not a static architecture diagram — a live graph that updates with each change. When a PR is opened, the maintainer can show: "This change touches these 4 files, which are depended on by these 12 other files, which serve these 3 user-facing features."

### Layer 2: Dynamics (What happened and why it propagated)

From DRI modeling (Bian & Gebraeel, 2014, Naval Research Logistics) and failure propagation hypergraphs (Mu et al., 2021, Quality and Reliability Engineering International):

The key insight is that in connected systems, a change in one component changes the behaviour of downstream components. This propagation follows the topology. You don't need a separate monitor — the system's connections ARE the detection network.

**Repo equivalent:** When a change breaks something downstream, record not just "test failed" but the propagation path: "Change to auth.ts → altered return type of `validateToken()` → broke `middleware/session.ts` which expected the old type → caused 3 integration tests to fail." The path is the diagnostic.

**Implementation:** The maintainer traces failure propagation paths on every CI failure. Not just "red X on the build" but the causal chain. Over time, this builds a map of which components are high-propagation-risk (changes there tend to break things far away) and which are contained (changes there stay local).

### Layer 3: Management (What decisions were made)

From intent-annotated deltas (Cederbladh et al., 2025, Systems Engineering):

Changes are recorded as transactional units ("deltas") annotated with intent. Not just "what changed" but "why this specific change was made." These get stored in a queryable structure.

**Repo equivalent:** Every PR and commit gets an intent annotation beyond the commit message. The maintainer records:
- What triggered this change (issue, bug report, feature request, dependency update, security advisory)
- What decision was made (fix the bug, work around it, refactor the whole module, defer)
- What alternatives were considered and rejected
- What the expected downstream effects are

**Implementation:** PR descriptions become structured intent records. The maintainer can query: "Show me every change triggered by a security advisory in the last 6 months." "Show me every change to the auth module and what triggered each one." "Show me every change where we chose to work around rather than fix."

### Layer 4: The 4D View (Time as a navigable dimension)

From digital twin architecture (Kerkeni et al., 2025; Protyai et al., 2026):

The digital twin is a virtual representation of the entire system that can be rewound, fast-forwarded, and inspected at any point in time. Every state change is recorded with full spatial context.

**Repo equivalent:** Git already stores every historical state. What's missing is the ability to navigate that history by component, by connection, by intent — not just by time. The 4D view would let you:
- Pick any component and see its full history across all dimensions (time, connections, intent, propagation effects)
- Pick any point in time and see the full system state
- Pick any failure and trace backwards through the propagation path to the originating change
- Pick any intent category and see all changes that share that intent, regardless of when they happened

**Implementation:** This is the long game. It requires:
1. The structural graph (Layer 1) to be versioned — not just the code, but the map of the code
2. Propagation paths (Layer 2) to be stored alongside CI results
3. Intent annotations (Layer 3) to be queryable
4. A viewer that lets you navigate all four dimensions

## How Components Self-Report

The industrial literature calls it Degradation-Rate-Interaction: when a component degrades, it changes the behaviour of its neighbours. The neighbours don't need external monitoring — their own performance shift IS the signal.

In a repo, the equivalent is:
- **Type errors** — a changed interface ripples through every file that imports it
- **Test failures** — a changed behaviour breaks every test that depends on it
- **Performance degradation** — a changed algorithm slows down every feature that calls it
- **Build failures** — a changed dependency breaks every module that depends on it

The maintainer should treat these not as "errors to fix" but as **the system's self-diagnostic output**. Every test failure is a component downstream from a change telling you: "something upstream from me shifted." The propagation path from the failure back to the originating change is the diagnostic.

## What This Adds to the Repo Process

The existing git/GitHub workflow already handles:
- Version control (time axis)
- Branching and merging (parallel development)
- Code review (quality gates)
- CI/CD (automated testing and deployment)

What this research suggests adding on top:
1. **System maps** — live dependency graphs, updated with each change
2. **Propagation tracing** — causal chains from change to failure, stored and queryable
3. **Intent records** — structured "why" annotations on every change
4. **Multi-axis navigation** — browse history by component, by connection, by intent, not just by time

None of this replaces git. It layers on top. Git is the storage engine. These are the lenses.

## Sources

- Bian, L., & Gebraeel, N. (2014). Stochastic framework for partially degradation systems with continuous component degradation-rate-interactions. Naval Research Logistics, 61(4), 286-303. https://doi.org/10.1002/nav.21583
- Mu, L., Zhang, Y., Wang, X., & Zhou, Y. (2021). Application of hypergraph theory in the analysis of the failure propagation and diffusion behaviour of machining centre. Quality and Reliability Engineering International, 38(2), 659-678. https://doi.org/10.1002/qre.3007
- Huang, J., Meng, T., Deng, Y., et al. (2021). Catching Critical Transition in Engineered Systems. Mathematical Problems in Engineering, 2021(1). https://doi.org/10.1155/2021/5589429
- Qasim, L., Hein, A. M., Olaru, S., et al. (2023). System reconfiguration ontology to support model-based systems engineering. Systems Engineering, 26(4), 347-364. https://doi.org/10.1002/sys.21661
- Cederbladh, J., Kamburjan, E., et al. (2025). Traceability Support for Engineering Reviews of Horizontal Model Evolution. Systems Engineering, 28(6), 720-736. https://doi.org/10.1002/sys.70001
- Roldán, M. L., Gonnet, S., & Leone, H. (2012). Knowledge representation of the software architecture design process based on situation calculus. Expert Systems, 30(1), 34-53. https://doi.org/10.1111/j.1468-0394.2012.00620.x
- Kerkeni, R., Mhalla, A., et al. (2025). Unsupervised Learning and Digital Twin Applied to Predictive Maintenance for Industry 4.0. Journal of Electrical and Computer Engineering, 2025(1). https://doi.org/10.1155/jece/3295799
- Protyai, M. I., Shariar, R., & Mural, H. (2026). Digital Twin Integration in Project Life Cycle Management — A Review. Engineering Reports, 8(3). https://doi.org/10.1002/eng2.70668
- Nenes, G., & Panagiotidou, S. (2011). A Bayesian model for the joint optimization of quality and maintenance decisions. Quality and Reliability Engineering International, 27(2), 149-163. https://doi.org/10.1002/qre.1101
- Engel, A., Teller, A., et al. (2021). Robust design under cumulative damage due to dynamic failure mechanisms. Systems Engineering, 24(5), 322-338. https://doi.org/10.1002/sys.21588
- English, L., Yunusa-Kaltungo, A., et al. (2021). Development of a complementary framework for implementing asset register solutions. Engineering Reports, 4(6). https://doi.org/10.1002/eng2.12493
- Capilla, R., Dueñas, J. C., & Krikhaar, R. (2012). Managing software development information in global configuration management activities. Systems Engineering, 15(3), 241-254. https://doi.org/10.1002/sys.20205
