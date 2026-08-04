# Capability 2 — Relational Integration (Multi-hop)



## At a glance

| | |
|---|---|
| **Core question** | Can the benchmark determine whether multiple stored memories are correctly combined? |
| **Definition** | the answer is not stated anywhere as a single fact; producing it requires combining ≥2 distinct, non-redundant stored facts introduced at different (typically non-adjacent) points in the conversation. Both facts must be individually necessary — the answer must not be recoverable from either one alone. |
| **Cognitive-science lens** | associative/relational integration — binding separately encoded facts into a new inferential judgment, akin to relational memory binding in hippocampal-dependent memory research. |

## Why this capability matters

real conversational memory is rarely used by restating a single fact back —
useful recall usually means connecting something said in one session to something said in
another. This capability isolates whether a system can do that connecting step at all.

a system that only aces Direct Retrieval but fails here can answer "what
did I say" but not "what follows from what I said" — which is most of what makes long-term
memory valuable over a simple lookup table.

## What a benchmark must require

combining facts across sessions, connecting
related events, synthesizing distributed information, graph traversal over stored facts, or any
multi-hop QA format.

## Boundaries and exclusions

(1) a question with an evidence pointer spanning
multiple turns is *not* automatically multi-hop — if one cited turn alone fully answers the
question and the other is merely corroborating, it's Direct Retrieval with over-annotated
evidence, not genuine synthesis; (2) if combining the facts is purely about ordering, recency,
or date arithmetic with no new relational conclusion beyond sequencing, it's Temporal Reasoning
([Capability 3](03-temporal-reasoning.md)); (3) if the reason two facts must be combined is that the later one
supersedes/negates the earlier one, it's Memory Updating ([Capability 4](04-memory-updating.md)) — Relational
Integration assumes the cited facts are compatible and must be *jointly* used, not that one
overrides the other. The same **contamination check** as [Capability 1](01-direct-retrieval.md) applies, plus: the answer
must not be derivable from either cited fact in isolation (that would collapse it back to
Direct Retrieval).

### How to classify a test item

Use this quick check when evaluating a candidate item:

1. Does the item require the behavior described by this capability's core question?
2. Does the item avoid the adjacent behaviors excluded above?
3. Does the benchmark score that behavior rather than merely making it available in the data?

Read the actual task, released example, and metric before assigning a category. A task label alone is useful evidence, not proof.

## Common failure modes

retrieving only one of the two required facts and guessing/
hallucinating the rest, retrieving both facts but failing to combine them correctly, or
"solving" the question via world knowledge instead of the stored facts (a contamination
failure).

## Example

*"Where did the user move from before their current city?"*,
where one turn states how long ago the move happened and a separate, non-adjacent turn names
the origin location — neither turn alone answers the question, only their combination does.

## Relationship to other capabilities

sits directly downstream of [Capability 1](01-direct-retrieval.md) (it presumes
correct retrieval of each individual fact) and adjacent to [Capability 3](03-temporal-reasoning.md) (temporal combination)
and [Capability 4](04-memory-updating.md) (contradiction combination) — see "What does not belong" above for the exact
dividing lines. Also related to [Capability 14](14-associative-retrieval.md) (Associative Retrieval): that capability finds
conceptually related memories without an explicit combination requirement, while this one
requires the combination itself to produce the answer.

## Common classification pitfalls

this is the capability most vulnerable to annotation-count
illusions — an evidence pointer with 2+ citations looks like multi-hop at a glance but isn't,
unless each citation is read and confirmed to be individually insufficient. Always verify by
reading the actual cited turns, not by counting evidence-pointer length.
