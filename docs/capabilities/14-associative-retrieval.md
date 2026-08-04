# Capability 14 — Associative Retrieval



## At a glance

| | |
|---|---|
| **Core question** | Can the benchmark evaluate retrieval based on semantic association rather than explicit links? |
| **Definition** | related memories are discovered even when no explicit relationship was stated or stored between them — surfacing conceptually connected information (e.g. inferring possible hobbies from mentioned equipment) rather than combining explicitly stated facts into a new conclusion. |
| **Cognitive-science lens** | spreading activation in semantic network models — the classic account (Collins & Loftus) of how activating one concept partially activates related concepts in a semantic network, enabling retrieval of associated-but-not-identical information. |

## Why this capability matters

not everything worth surfacing from memory is connected by an explicit,
statable link — some of the most useful recall is associative, surfacing a conceptually related
memory that was never explicitly tied to the current context.

a system limited to exact-match or explicitly-linked retrieval will miss
conceptually relevant memories that a human conversational partner would naturally bring to
mind — this capability is what separates keyword/exact-fact lookup from something closer to
semantic recall.

## What a benchmark must require

tasks that require surfacing a conceptually
related memory without an explicit stated link — e.g. inferring possible hobbies from mentioned
equipment, travel interests from cities visited, or favorite topics from books read.

## Boundaries and exclusions

unlike Relational Integration ([Capability 2](02-relational-integration.md)), the
objective here is not to *combine* explicit facts into one derived conclusion required by the
question — it's to *discover* a conceptually related memory in the first place, where the
"answer" is closer to a relevance judgment than a synthesized fact. If the question requires
strictly combining two specific stated facts to derive a required answer, that's [Capability 2](02-relational-integration.md);
if it requires surfacing something merely conceptually adjacent without a required logical
combination, that's this capability.

### How to classify a test item

Use this quick check when evaluating a candidate item:

1. Does the item require the behavior described by this capability's core question?
2. Does the item avoid the adjacent behaviors excluded above?
3. Does the benchmark score that behavior rather than merely making it available in the data?

Read the actual task, released example, and metric before assigning a category. A task label alone is useful evidence, not proof.

## Common failure modes

missing an obviously conceptually-related memory because no explicit
keyword or stated link connects it to the query; or the opposite failure — over-associating,
surfacing loosely related but ultimately irrelevant memories (bleeding into [Capability 11](11-retrieval-robustness.md)
territory).

## Example

a user mentions owning camping equipment across several
mentions; a later question asks for gift ideas or activity suggestions, and the benchmark checks
whether outdoor/camping-adjacent options are surfaced without the user ever explicitly stating
"I like camping" or being asked to recall the equipment directly.

## Relationship to other capabilities

distinguished from [Capability 2](02-relational-integration.md) (explicit combination of
stated facts into a required answer) and from [Capability 8](08-knowledge-abstraction.md) (which forms a stable generalized
concept from repeated observations) — this capability can operate on a single memory with no
repetition at all, purely via conceptual/semantic proximity, and doesn't require the result to
be logically entailed by the stored facts the way [Capability 2](02-relational-integration.md) does. Becomes increasingly
important for semantic-memory and graph-based memory architectures specifically, though the
capability itself is defined independent of architecture per this taxonomy's design principles.

## Common classification pitfalls

the boundary with [Capability 2](02-relational-integration.md) is the most likely source of
scoring disagreement in this taxonomy — when in doubt, ask whether the question has one
logically-required combination of stated facts ([Capability 2](02-relational-integration.md)) or is satisfied by any
sufficiently-related surfaced memory (this capability, which tolerates more than one
reasonable answer).
