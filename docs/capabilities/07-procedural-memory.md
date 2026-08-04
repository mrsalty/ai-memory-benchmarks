# Capability 7 — Procedural Memory



## At a glance

| | |
|---|---|
| **Core question** | Can the benchmark evaluate whether stored procedures are correctly reused? |
| **Definition** | previously stored workflows, instructions, or operating procedures are reused correctly in a later interaction — e.g. following a previously agreed-upon format, convention, or process without being re-told it. |
| **Cognitive-science lens** | non-declarative / skill / habit memory — but see the substrate-mismatch flag above; the AI-benchmark version of this capability is not a faithful analogue of the cog-sci concept it's named after. |

## Why this capability matters

recurring collaboration (a coding convention, a preferred meeting format, a
review process) depends on the system remembering *how* to do something, not just *what* was
said — nominally a distinct memory system in human cognition, included here because AI
benchmarks do make claims about testing it.

if a system has to be re-told the same workflow every session, it isn't
providing the continuity that makes long-term memory valuable for repeated collaborative tasks.

## What a benchmark must require

recurring workflows, preferred meeting formats,
coding conventions, review processes, or other previously-specified operating procedures being
applied again in a new interaction.

## Boundaries and exclusions

**substrate mismatch flag** — in human cognition,
procedural memory is *non-declarative* skill/habit memory (how to ride a bike), which can't be
verbalized the way a declarative fact can and is acquired through repetition rather than being
told once. AI benchmarks that claim to test "procedural memory" are almost always testing
stored *textual* how-to facts ("here's the process we agreed on for X") — a purely declarative
substitute for the actual cog-sci concept, and functionally closer to [Capability 1](01-direct-retrieval.md) (Direct
Retrieval) or [Capability 2](02-relational-integration.md) (Relational Integration) than to genuine skill memory. Flag this
every time a benchmark claims coverage here, and treat a "Full" verdict skeptically pending a
closer read of what's actually being retrieved and reused.

### How to classify a test item

Use this quick check when evaluating a candidate item:

1. Does the item require the behavior described by this capability's core question?
2. Does the item avoid the adjacent behaviors excluded above?
3. Does the benchmark score that behavior rather than merely making it available in the data?

Read the actual task, released example, and metric before assigning a category. A task label alone is useful evidence, not proof.

## Common failure modes

correctly recalling the stated procedure when asked to describe it,
but failing to actually *follow* it in a new task without being reminded — the procedural
equivalent of the [Capability 6](06-personalization.md) recall-vs-application gap.

## Example

a user specifies a preferred code review format in an early
session; a later session asks the system to review new code, and the benchmark checks whether
the review follows the previously-specified format without being told the format again.

## Relationship to other capabilities

given the substrate mismatch above, this capability is
functionally closer to [Capability 1](01-direct-retrieval.md)/2 (retrieval and application of a stored textual
description) than to a truly distinct memory system; it's also closely related to [Capability 6](06-personalization.md)
(both require applying a remembered fact to change behavior in a new task, rather than just
recalling it).

## Common classification pitfalls

because of the substrate mismatch, this is one of the
capabilities most likely to be over-claimed by benchmark authors — a "procedural memory" label
on a benchmark category should be treated as a hypothesis to check against actual task
mechanics, not a settled fact.
