# Capability 11 — Retrieval Robustness

[← Back to capability index](../capabilities.md)

## At a glance

| | |
|---|---|
| **Core question** | Can the benchmark distinguish accurate retrieval from noisy retrieval? |
| **Definition** | the system's answer is correct *and* the retrieval that produced it wasn't buried in, or contaminated by, irrelevant material — resistance to distractors, irrelevant memories, semantic interference, duplicated information, or conflicting context surfaced alongside the right answer. |
| **Cognitive-science lens** | a weak analogue to interference resistance in human memory — the ability to retrieve a target memory despite competing or similar memories, though the sw-sys framing here (retrieval-pipeline noise) is a looser fit than most other capabilities' cog-sci grounding. |

## Why this capability matters

finding the correct memory is not sufficient on its own if it's surfaced
alongside a pile of irrelevant or contradictory material that a downstream generation step then
has to sort through — that noise is itself a source of errors traditional "was the answer
present" metrics don't capture.

traditional Recall@K and presence-in-top-k metrics are structurally blind
to this — they score "did the right answer show up" without penalizing what showed up alongside
it, so a system can score well on those metrics while still regularly getting confused by noise
in practice.

## What a benchmark must require

benchmarks that explicitly measure or penalize
retrieval pollution — e.g. scoring degrades as distractor density increases, or a metric checks
precision (how much of what's retrieved is actually relevant) alongside recall.

## Boundaries and exclusions

this is not Scalability ([Capability 13](13-scalability.md)) — scale is
about performance as *volume* grows in the abstract; this capability is specifically about
whether *irrelevant material adjacent to the correct answer* degrades the system's output, which
can be tested even at small scale with a handful of well-placed distractors.

### How to classify a test item

Use this quick check when evaluating a candidate item:

1. Does the item require the behavior described by this capability's core question?
2. Does the item avoid the adjacent behaviors excluded above?
3. Does the benchmark score that behavior rather than merely making it available in the data?

Read the actual task, released example, and metric before assigning a category. A task label alone is useful evidence, not proof.

## Common failure modes

correctly locating the right fact but also surfacing several wrong
or contradictory ones alongside it, letting a downstream generation step get confused by that
noise even when precision-of-location was fine; or being technically "correct" per a
presence-based metric while being useless in practice because of surrounding noise.

## Illustrative benchmark item

> **Status:** Hypothetical example—not evidence that a specific benchmark measures this capability.
>
> **Memory:** The target fact says “Mina prefers decaf”; nearby distractors state that several other people prefer regular coffee.
>
> **Task:** “What coffee does Mina prefer?” The evaluation records both the answer and irrelevant memories retrieved alongside it.
>
> **Expected behavior:** Return “decaf” without contaminating the answer or retrieval set with the distractors.
>
> **Why this capability:** The benchmark must penalize irrelevant retrieval, not merely reward finding the target fact.

## Relationship to other capabilities

closely tied to [Capability 1](01-direct-retrieval.md) (it's a robustness
qualifier on retrieval accuracy, not a separate retrieval task) and to [Capability 5](05-forgetting-and-memory-management.md) (unmanaged
accumulation from failing to forget is one way noise builds up in the first place, though the
two capabilities test different things — active memory management vs. resistance to whatever
noise is present).

## Common classification pitfalls

a benchmark reporting only recall/presence metrics provides
no capability-11 evidence, even at very large scale or distractor count — don't infer Full
coverage from "the dataset has lots of distractors" without checking that the *metric* actually
penalizes noise in the output, not just measures whether the answer was findable somewhere.
