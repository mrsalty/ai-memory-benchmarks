# Capability 3 — Temporal Reasoning



## At a glance

| | |
|---|---|
| **Core question** | Can the benchmark evaluate whether memories are interpreted correctly with respect to time? |
| **Definition** | the system reasons over temporal properties of stored memories — ordering, recency, durations, historical/"as of" state, or before/after relationships — using time-related cues in the conversation (explicit dates, relative expressions, or session ordering), rather than simply reciting a stored timestamp. |
| **Cognitive-science lens** | temporal source memory — the aspect of episodic memory that encodes *when* an experience occurred, distinct from encoding *what* occurred. |

## Why this capability matters

conversational memory accumulates over time, so questions about *when* things
happened, or what was true *at a given point*, are a distinct reasoning demand from questions
about *what* happened — a system can get the facts right and still get the timeline wrong.

users routinely ask time-relative questions ("what did I say before my
trip," "is this still current"), and a system that can't place facts on a timeline will give
technically-sourced but contextually wrong answers.

## What a benchmark must require

event ordering, recency judgments, duration
computation, "as of time T" queries, and before/after relationship questions.

## Boundaries and exclusions

if the temporal placement is incidental to a
question whose real demand is combining two unrelated facts into a new relational conclusion,
that's Relational Integration ([Capability 2](02-relational-integration.md)), not Temporal Reasoning — this capability requires
that reasoning about *time itself* (not just using time-stamped facts) be the point of the
question. Simply recalling an explicitly-stated timestamp verbatim, with no ordering/duration/
recency computation involved, is Direct Retrieval ([Capability 1](01-direct-retrieval.md)).

### How to classify a test item

Use this quick check when evaluating a candidate item:

1. Does the item require the behavior described by this capability's core question?
2. Does the item avoid the adjacent behaviors excluded above?
3. Does the benchmark score that behavior rather than merely making it available in the data?

Read the actual task, released example, and metric before assigning a category. A task label alone is useful evidence, not proof.

## Common failure modes

conflating "recently mentioned" with "recently true," miscomputing
relative dates ("the week before X"), or answering with the wrong session's timeframe when
multiple candidate events could match.

## Example

*"When did the user start their new job?"* → *"the Monday
before their birthday"* — answerable only by resolving a relative time expression against
another stated date, not by reciting a verbatim timestamp.

## Relationship to other capabilities

distinct from Relational Integration ([Capability 2](02-relational-integration.md)) in
requiring a temporal computation rather than a new non-temporal relational fact; distinct from
Memory Updating ([Capability 4](04-memory-updating.md)) in that no contradiction/supersession is involved — the facts
being ordered are all still true, just at different points on a timeline.

## Common classification pitfalls

date-arithmetic questions that also require picking the
*right* fact out of several candidates blur into [Capability 2](02-relational-integration.md) territory — the dividing line is
whether the extra work is temporal computation (this capability) or a non-temporal relational
inference ([Capability 2](02-relational-integration.md)).
