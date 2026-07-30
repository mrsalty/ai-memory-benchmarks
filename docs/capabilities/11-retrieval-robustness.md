# Capability 11 — Retrieval Robustness

[← Back to capability index](../capabilities.md)

**Core question**: Can the benchmark distinguish accurate retrieval from noisy retrieval?

**Definition**: the system's answer is correct *and* the retrieval that produced it wasn't
buried in, or contaminated by, irrelevant material — resistance to distractors, irrelevant
memories, semantic interference, duplicated information, or conflicting context surfaced
alongside the right answer.

**Motivation**: finding the correct memory is not sufficient on its own if it's surfaced
alongside a pile of irrelevant or contradictory material that a downstream generation step then
has to sort through — that noise is itself a source of errors traditional "was the answer
present" metrics don't capture.

**Why it matters**: traditional Recall@K and presence-in-top-k metrics are structurally blind
to this — they score "did the right answer show up" without penalizing what showed up alongside
it, so a system can score well on those metrics while still regularly getting confused by noise
in practice.

**What kinds of benchmark tasks evaluate it**: benchmarks that explicitly measure or penalize
retrieval pollution — e.g. scoring degrades as distractor density increases, or a metric checks
precision (how much of what's retrieved is actually relevant) alongside recall.

**What does not belong to this capability**: this is not Scalability ([Capability 13](13-scalability.md)) — scale is
about performance as *volume* grows in the abstract; this capability is specifically about
whether *irrelevant material adjacent to the correct answer* degrades the system's output, which
can be tested even at small scale with a handful of well-placed distractors.

**Typical failure modes**: correctly locating the right fact but also surfacing several wrong
or contradictory ones alongside it, letting a downstream generation step get confused by that
noise even when precision-of-location was fine; or being technically "correct" per a
presence-based metric while being useless in practice because of surrounding noise.

**Example benchmark questions**: a question whose correct answer is present in the stored
memory alongside several plausible-but-wrong distractor facts (e.g. near-duplicate entities,
similar attributes for different people) — the benchmark checks whether the final answer
reflects the correct fact specifically, not a blend or a wrong neighbor.

**Relationship to other capabilities**: closely tied to [Capability 1](01-direct-retrieval.md) (it's a robustness
qualifier on retrieval accuracy, not a separate retrieval task) and to [Capability 5](05-forgetting-and-memory-management.md) (unmanaged
accumulation from failing to forget is one way noise builds up in the first place, though the
two capabilities test different things — active memory management vs. resistance to whatever
noise is present).

**Mapping to cognitive science**: a weak analogue to interference resistance in human memory —
the ability to retrieve a target memory despite competing or similar memories, though the
sw-sys framing here (retrieval-pipeline noise) is a looser fit than most other capabilities'
cog-sci grounding.

**Notes about common ambiguities**: a benchmark reporting only recall/presence metrics provides
no capability-11 evidence, even at very large scale or distractor count — don't infer Full
coverage from "the dataset has lots of distractors" without checking that the *metric* actually
penalizes noise in the output, not just measures whether the answer was findable somewhere.

