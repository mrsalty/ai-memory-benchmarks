# Capability 8 — Knowledge Abstraction

[← Back to capability index](../capabilities.md)

## At a glance

| | |
|---|---|
| **Core question** | Can the benchmark evaluate whether repeated observations become generalized knowledge? |
| **Definition** | the system forms a higher-level concept from multiple individual experiences or observations — repeated observations becoming a stable preference, repeated interactions becoming a recognized user habit, or repeated events becoming a semantic concept — rather than just retrieving the individual observations that fed into it. |
| **Cognitive-science lens** | schema formation and semantic memory consolidation — the process by which repeated episodic experiences give rise to generalized semantic knowledge, independent of any single originating episode. |

## Why this capability matters

some of the most valuable things a long-term memory system could know about a
user (their habits, their general tendencies) are never stated as a single fact; they only
become knowable by noticing a pattern across many separate mentions.

without this capability, a system can only ever know what was explicitly
told to it, never what can be *inferred* from a pattern of behavior — a meaningfully weaker
form of understanding than what long-term human relationships develop.

## What a benchmark must require

tasks that require noticing a pattern across
several separate, individually-unremarkable mentions and stating the generalized conclusion
(e.g., inferring "the user is training for a marathon" from several separate mentions of long
runs, never stated as such directly).

## Boundaries and exclusions

this is distinct from Relational Integration
([Capability 2](02-relational-integration.md)) — multi-hop combines a *fixed, small number* of existing explicit facts into one
answer; abstraction *creates a new semantic representation* from a pattern across many
observations, where no single combination of a couple of facts would produce the same
conclusion. It's also distinct from [Capability 9](09-episodic-vs-semantic-understanding.md) (episodic vs. semantic) — that capability is
about *not conflating* a one-off event with a generalization, whereas this capability is about
whether the generalization gets *formed* correctly from real repetition in the first place.

### How to classify a test item

Use this quick check when evaluating a candidate item:

1. Does the item require the behavior described by this capability's core question?
2. Does the item avoid the adjacent behaviors excluded above?
3. Does the benchmark score that behavior rather than merely making it available in the data?

Read the actual task, released example, and metric before assigning a category. A task label alone is useful evidence, not proof.

## Common failure modes

over-abstracting from a single instance (treating one mention as a
pattern — a [Capability 9](09-episodic-vs-semantic-understanding.md) boundary violation), never forming the abstraction at all even when
the pattern is clearly present across many mentions, or forming an abstraction but being unable
to cite the observations that support it.

## Illustrative benchmark item

> **Status:** Hypothetical example—not evidence that a specific benchmark measures this capability.
>
> **Memory:** Across several sessions, the user describes long weekend runs, a training schedule, and an upcoming race.
>
> **Task:** “What long-term goal is the user likely pursuing?”
>
> **Expected behavior:** Infer a supported high-level goal such as training for a marathon, while expressing appropriate uncertainty.
>
> **Why this capability:** The conclusion is a pattern across repeated observations, not a fact stated in any one turn.

## Relationship to other capabilities

builds on [Capability 1](01-direct-retrieval.md) (the individual observations
must first be retrievable) and is bounded by [Capability 9](09-episodic-vs-semantic-understanding.md) (the abstraction must be
appropriately scoped, not over-generalized from too little evidence); distinct from [Capability 2](02-relational-integration.md)
in requiring pattern formation across many instances rather than combination of a couple of
discrete facts.

## Common classification pitfalls

the line between "abstraction" and "well-annotated
multi-hop with unusually many evidence citations" can blur — the deciding factor is whether the
conclusion requires genuine pattern recognition across the observations (this capability) or
is fully determined by logically combining a small fixed set of them ([Capability 2](02-relational-integration.md)).
