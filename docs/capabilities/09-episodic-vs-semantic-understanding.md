# Capability 9 — Episodic vs Semantic Understanding

[← Back to capability index](../capabilities.md)

## At a glance

| | |
|---|---|
| **Core question** | Can the benchmark determine whether specific experiences and generalized knowledge are correctly distinguished? |
| **Definition** | the system distinguishes a specific one-off event ("I went to Paris last March") from a generalized or standing fact ("I love traveling to Europe"), neither over-generalizing from a single episode nor under-generalizing (failing to recognize an established pattern as general). |
| **Cognitive-science lens** | the classic episodic/semantic memory subdivision within declarative memory — episodic memory for specific, contextually-situated experiences; semantic memory for generalized, context-free knowledge. |

## Why this capability matters

not everything worth remembering is the same *kind* of memory — a single
vacation is not the same as a travel preference, and a system needs to keep those categories
separate to reason about the user correctly.

conflating the two produces two different failure directions: assuming a
single event implies an ongoing preference (recommending Europe trips forever because of one
mentioned vacation), or failing to recognize a genuinely repeated pattern as a stable trait.

## What a benchmark must require

questions that require correctly classifying
whether a piece of information is a specific instance or a general pattern, or that penalize a
system for over-/under-generalizing between the two.

## Boundaries and exclusions

this is not about whether the abstraction gets
*formed* (that's [Capability 8](08-knowledge-abstraction.md)) — it's about whether, once formed or presented, episodic and
semantic information are kept correctly separated and not conflated. A single Direct Retrieval
([Capability 1](01-direct-retrieval.md)) of a one-off statement is not this capability unless the benchmark specifically
tests whether the system wrongly treats that one-off statement as a general trait (or vice
versa).

### How to classify a test item

Use this quick check when evaluating a candidate item:

1. Does the item require the behavior described by this capability's core question?
2. Does the item avoid the adjacent behaviors excluded above?
3. Does the benchmark score that behavior rather than merely making it available in the data?

Read the actual task, released example, and metric before assigning a category. A task label alone is useful evidence, not proof.

## Common failure modes

one purchase treated as an ongoing shopping habit, one complaint
treated as persistent dissatisfaction, or a genuinely repeated pattern dismissed as a
coincidence.

## Illustrative benchmark item

> **Status:** Hypothetical example—not evidence that a specific benchmark measures this capability.
>
> **Memory:** “I visited Italy once on a school trip.”
>
> **Task:** “Should the assistant treat Italy as a standing travel preference?”
>
> **Expected behavior:** No; identify the statement as a one-off event rather than a durable preference.
>
> **Why this capability:** The task tests whether an episode is kept distinct from generalized knowledge.

## Relationship to other capabilities

the natural boundary check for [Capability 8](08-knowledge-abstraction.md) (knowledge
abstraction) — [Capability 8](08-knowledge-abstraction.md) asks whether a genuine pattern gets abstracted into general
knowledge; Capability 9 asks whether a *non*-pattern (a single episode) is correctly *not*
treated the same way, and vice versa.

## Common classification pitfalls

how many repetitions justify treating something as a
"pattern" rather than a coincidence is inherently a judgment call — a benchmark should state its
own threshold explicitly (e.g. "mentioned in ≥3 separate sessions") for this capability's
verdict to be scored objectively rather than subjectively.
