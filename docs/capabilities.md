# Memory Benchmark Capability Taxonomy

## Purpose

This taxonomy does **not** describe how AI memory systems are implemented. It describes **what
capabilities a benchmark is capable of evaluating.**

Each capability represents a distinct, independently-measurable aspect of memory behavior. A
benchmark may provide strong evidence for one capability while providing no evidence at all for
another — the point of this taxonomy is to make that gap visible rather than let a single
headline metric imply broad coverage it doesn't have.

This is the common reference framework used in this repo to analyze memory benchmarks (e.g.
LoCoMo, LongMemEval, MSC, PerLTQA, StreamingQA, BABILong, RULER, DMR, MemoryBank), so they can
be compared on a shared set of evaluation capabilities rather than only by their own published
metrics.

The taxonomy is scoped to **AI/software memory systems** — persistent memory for LLM-based
conversational agents and assistants (RAG stores, vector DBs, knowledge graphs, structured
memory layers) — and stays **implementation-independent**: it describes observable behaviors,
not any particular memory architecture. It is **not** a framework for evaluating human
cognition. Cognitive psychology / neuroscience is used only as a *reference lens* to name and
validate capabilities, because that field's taxonomy is far more mature and empirically
grounded than anything that has emerged organically from AI benchmark design. Comparing AI
systems against human psychometric batteries is a legitimate but different project.

## Design principles

Every capability in this taxonomy is held to five criteria:

- **Orthogonality** — it represents a distinct evaluable behavior, minimizing overlap with
  other capabilities. Where overlap is unavoidable, it's called out explicitly (see each
  capability's "What does not belong to this capability" and "Relationship to other
  capabilities" sections).
- **Implementation independence** — it doesn't assume any particular memory architecture or
  storage mechanism (vector database, knowledge graph, symbolic memory, hybrid memory, or
  otherwise).
- **Benchmark-centric** — it describes what evidence a *benchmark* can provide, not how a
  memory system *should* be built.
- **Operational clarity** — it should be possible to determine objectively whether a benchmark
  provides Full, Partial, or No coverage of it via a fixed decision rule, not a subjective
  impression (see the Coverage rubric below).
- **Boundary definitions** — it explicitly describes where it ends and neighboring capabilities
  begin, reducing ambiguity during benchmark classification.

## Coverage rubric

Every benchmark page scores each capability by checking three proof legs, in a fixed order, and
reading the verdict off a deterministic table — not a subjective impression. This keeps scoring
reproducible across any capability and any benchmark, and removes the ambiguous "does this
count as partial credit?" judgment call that an unstructured rubric invites.

**The three legs** (unchanged from prior versions of this rubric):

- **Leg A — Authored targeting.** Can you *quote* the benchmark's authors (a category
  definition, task description, or metric doc in the primary source) stating they designed an
  evaluation for *this exact capability's core question*? This must be specific to the
  capability, not the benchmark's general theme — "this paper is broadly about long-term
  memory" does not satisfy Leg A for every memory-related capability; it would make Leg A
  trivially true for everything and destroy its discriminative power.
- **Leg B — Concrete demonstration.** Does at least one real, traceable example exist in the
  released data where the capability is genuinely exercised — verified by reading the example
  itself, not by trusting a category label?
- **Leg C — Metric isolation.** Does the benchmark's metric produce a score that specifically
  reflects this capability — not blended into an unrelated aggregate, and not actually measuring
  a materially different property under a similar name (e.g. retrieval *recall* is not the same
  property as noise/precision robustness, even though both are "retrieval metrics")?

**Verdict table** — check Leg A first; it gates everything else:

| Leg A | Legs B + C | Verdict | Glyph |
|---|---|---|---|
| Not satisfied | *(irrelevant — see below)* | **None** | ❌ |
| Satisfied | Both hold | **Full** | ✅ |
| Satisfied | Either incomplete or fails | **Partial** | 🟡 |
| *(not yet checked)* | *(not yet checked)* | **Pending** | ☐ |

**Why Leg A gates the whole decision**: if the authors never designed anything targeting this
capability, any examples that happen to exercise it are a byproduct of realistic data
construction, not a systematic measurement — the benchmark cannot be trusted to tell you
anything reliable about this capability, so it's **None**, full stop, regardless of how many
byproduct instances you can find. This is what used to be a separate "Incidental" tier; folding
it into a Leg-A check removes the ambiguity of when byproduct coverage "counts" for something —
it never does, on its own. Partial is reserved for cases where the authors clearly *did* target
the capability (Leg A holds, quotably) but the evidence or the metric falls short of a Full
verdict.

An unscored `Pending` cell (capability not yet evaluated against a given benchmark) is more
honest than a guessed tier — see "Using this taxonomy" at the bottom of this page.

## Capabilities

Each capability's full specification — Definition, Motivation, Why it matters, benchmark task
types, boundary rules ("what does not belong"), typical failure modes, example questions,
relationship to other capabilities, cognitive-science mapping, and common ambiguities — lives in
its own page under [`capabilities/`](capabilities/), linked below. These pages describe only
what the capability *is*, independent of any specific benchmark; which benchmarks cover which
capabilities, and how well, is scored on each benchmark's own page under
[`benchmarks/`](benchmarks/), not here.

| # | Capability | Cognitive-science grounding |
|---|---|---|
| 1 | [Direct Retrieval](capabilities/01-direct-retrieval.md) | cog-sci: declarative retrieval |
| 2 | [Relational Integration (Multi-hop)](capabilities/02-relational-integration.md) | cog-sci: associative/relational integration |
| 3 | [Temporal Reasoning](capabilities/03-temporal-reasoning.md) | cog-sci: temporal source memory |
| 4 | [Memory Updating](capabilities/04-memory-updating.md) | cog-sci: memory updating, interference resolution |
| 5 | [Directed Memory Deletion](capabilities/05-directed-memory-deletion.md) | sw-sys: explicit memory-control operation, no direct cog-sci equivalent |
| 6 | [Personalization](capabilities/06-personalization.md) | cog-sci: self-referential / autobiographical memory |
| 7 | [Procedural Memory](capabilities/07-procedural-memory.md) | cog-sci: non-declarative / skill memory (substrate mismatch — see page) |
| 8 | [Knowledge Abstraction](capabilities/08-knowledge-abstraction.md) | cog-sci: schema formation, semantic memory consolidation |
| 9 | [Episodic vs Semantic Understanding](capabilities/09-episodic-vs-semantic-understanding.md) | cog-sci: declarative memory subdivision |
| 10 | [Memory Calibration](capabilities/10-memory-calibration.md) | cog-sci: metamemory / feeling-of-knowing |
| 11 | [Retrieval Robustness](capabilities/11-retrieval-robustness.md) | sw-sys; weak cog-sci analogue: interference resistance |
| 12 | [Persistence](capabilities/12-persistence.md) | sw-sys; weak cog-sci analogue: consolidation over time |
| 13 | [Scalability](capabilities/13-scalability.md) | sw-sys, no clean cog-sci analogue |
| 14 | [Associative Retrieval](capabilities/14-associative-retrieval.md) | cog-sci: spreading activation / semantic network models |
| 15 | [Memory Formation and Write Fidelity](capabilities/15-memory-formation-and-write-fidelity.md) | cog-sci: memory encoding |
| 16 | [Memory Provenance and Source Attribution](capabilities/16-memory-provenance-and-source-attribution.md) | cog-sci: source monitoring |

## Known gaps — cog-sci capabilities no benchmark here tests

These are well-studied human memory phenomena that don't correspond to any of the 16 capabilities
above and have **no equivalent in any benchmark surveyed in this repo**. Listed here explicitly
so the gap doesn't get lost — this is one of the more citable findings of the whole project.

- **Prospective memory** — "remember to do X later." A core human memory capability; essentially
  absent from every AI memory benchmark surveyed so far.
- **Retrieval-induced forgetting / interference** — recalling one memory suppressing related
  ones. This is distinct from [Capability 5](capabilities/05-directed-memory-deletion.md), which
  requires an explicit directed deletion operation. No benchmark scores retrieval-induced
  forgetting as a *failure mode*; all treat any forgetting as strictly bad.
- **Reconsolidation-on-retrieval** — a memory becoming modifiable simply by being recalled. No
  benchmark tests whether retrieval itself should alter stored state.

## Using this taxonomy

Each benchmark page in [`benchmarks/`](benchmarks/) should score itself against Capabilities
1–16 using the Coverage rubric above (✅ Full / 🟡 Partial / ❌ None / ☐ Pending) and note its
metric type plus that metric's structural blind spots — a benchmark can claim to "cover" a
capability while its metric is structurally incapable of detecting failures in it (see:
presence-in-top-k metrics and [Capability 11](capabilities/11-retrieval-robustness.md), retrieval
robustness). A ☐ `Pending` cell is always more honest than a guessed tier — per this project's
methodology, a capability is only scored against a specific benchmark once Leg A (authored
targeting) is checked first, gating whether Legs B (concrete example) and C (metric isolation)
even matter — see the Coverage rubric's decision table above.
