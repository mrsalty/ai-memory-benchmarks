# Capability 13 — Scalability



## At a glance

| | |
|---|---|
| **Core question** | Can the benchmark evaluate performance as memory size grows? |
| **Definition** | whether other memory capabilities' performance degrades as memory volume, number of sessions, number of entities, distractor count, or total stored knowledge increases. |
| **Cognitive-science lens** | no clean human-memory analogue — human memory doesn't degrade as a direct, measurable function of "store size" the way a retrieval index's recall does under growing load; this is a purely sw-sys engineering concern included because it materially affects whether a benchmark's other capability claims hold up at realistic memory sizes. |

## Why this capability matters

a memory system that works well on a small store but degrades sharply as it
grows is not actually solving the problem that motivates persistent memory in the first
place — most of the value of long-term memory only materializes after significant accumulation.

this is primarily an engineering stress-test dimension rather than a
correctness dimension in isolation — it asks not "is this capability present" but "does this
capability's presence survive realistic scale," which a benchmark can only answer if it
actually varies scale and measures the resulting degradation curve.

## What a benchmark must require

benchmarks that report performance as a function
of store size, session count, or distractor volume (rather than a single aggregate number at one
fixed scale), letting a degradation curve be observed rather than a single data point.

## Boundaries and exclusions

this is not Retrieval Robustness
([Capability 11](11-retrieval-robustness.md)) — that capability is about noise/distractor *interference* at a given point in
time, testable even at small scale; this capability is specifically about the *trend* as raw
volume grows, which requires multiple scale points to measure, not just one noisy scenario.

### How to classify a test item

Use this quick check when evaluating a candidate item:

1. Does the item require the behavior described by this capability's core question?
2. Does the item avoid the adjacent behaviors excluded above?
3. Does the benchmark score that behavior rather than merely making it available in the data?

Read the actual task, released example, and metric before assigning a category. A task label alone is useful evidence, not proof.

## Common failure modes

only reporting a single aggregate score at one fixed dataset size,
which provides no scalability evidence at all regardless of how large that fixed size is;
degrading gracefully on paper due to averaging effects that mask a sharp cliff at a specific
scale threshold.

## Example

a benchmark reporting accuracy separately at multiple context
lengths or distractor counts (e.g. accuracy at 1k, 10k, 100k tokens of accumulated memory),
letting a reader see whether and where performance drops off, rather than one number for the
whole dataset.

## Relationship to other capabilities

a cross-cutting stress dimension applied to whichever
other capability is being tested at increasing scale — conceptually it multiplies with
Capabilities 1–12 rather than sitting beside them as an independent behavior.

## Common classification pitfalls

a large dataset is not the same as evidence of scalability
testing — the benchmark must report results *at multiple scale points* (or otherwise isolate the
effect of scale) for this capability to be scored above `None`; a single large fixed-size
dataset with one aggregate score provides no scalability evidence on its own.
