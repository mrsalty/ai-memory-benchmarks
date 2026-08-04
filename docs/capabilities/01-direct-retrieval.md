# Capability 1 — Direct Retrieval

[← Back to capability index](../capabilities.md)

## At a glance

| | |
|---|---|
| **Core question** | Can the benchmark determine whether a system correctly retrieves one stored memory? |
| **Definition** | The answer is fully contained in one stored fact or turn. Retrieving and returning that fact—verbatim or paraphrased—is sufficient; no other stored fact or reasoning step is required. |
| **Cognitive-science lens** | Declarative memory retrieval: recalling a fact that was explicitly encoded. |

## Why this capability matters

Direct Retrieval is the most fundamental memory capability and the floor on which more complex capabilities build. A system that cannot reliably retrieve one relevant fact cannot be trusted on tasks that add synthesis, personalization, or updating on top of retrieval.

Nearly every practical use of conversational memory starts here: recalling a name, preference, or stated fact. If retrieval fails, downstream failures cannot be diagnosed reliably—it is unclear whether the system failed to reason over the memories or never found the right memory at all.

## What a benchmark must require

A Direct Retrieval item should require an answer that traces to **one identifiable source turn**. Typical task formats include:

- factual recall or attribute lookup;
- recalling a name, location, or explicitly stated preference; and
- any question whose answer is fully supported by one stored fact.

## Illustrative benchmark item

> **Status:** Hypothetical example—not evidence that a specific benchmark measures this capability.
>
> **Memory:** “I live in Porto.”
>
> **Task:** “Which city does the user live in?”
>
> **Expected behavior:** Return “Porto” by retrieving that one stored statement.
>
> **Why this capability:** One source turn fully answers the question. No fact combination, temporal interpretation, or contradiction resolution is required.

## Boundaries and exclusions

Direct Retrieval is deliberately narrow. An item does **not** belong here when:

- **It requires two or more non-redundant facts.** That is [Relational Integration](02-relational-integration.md) (Capability 2), even if a differently phrased question could be answered from one evidence pointer.
- **It requires temporal interpretation.** Recalling a timestamp verbatim may be Direct Retrieval; ordering events, resolving recency, or calculating a duration is [Temporal Reasoning](03-temporal-reasoning.md) (Capability 3).
- **It requires choosing a newer fact over a contradicted older fact.** That is [Memory Updating](04-memory-updating.md) (Capability 4), not ordinary retrieval.
- **The answer is available from the model's parametric world knowledge alone.** The benchmark must require consulting the stored fact; otherwise it is not testing memory.

### How to classify a test item

Use this quick check when evaluating a candidate item:

1. Can one stored turn fully answer the question?
2. Is that turn necessary—that is, can the answer *not* be supplied reliably from general world knowledge alone?
3. Does the question avoid temporal computation, contradiction resolution, and combining multiple facts?

If all three answers are yes, the item is a Direct Retrieval candidate. Read the actual cited turn before assigning the category; an annotation such as “single-hop” is useful evidence, not proof.

## Common failure modes

- Retrieving a similar but wrong fact, such as confusing attributes between entities.
- Returning a stale fact that was later updated; this can look like retrieval failure but is often a Capability-4 failure.
- Failing to retrieve a relevant fact buried deep in a long history.

## Relationship to other capabilities

Direct Retrieval is the baseline against which the other capabilities are defined. It supplies the individual facts that [Relational Integration](02-relational-integration.md) combines, but it does not itself test that combination. It is also distinct from temporal interpretation, update resolution, and every other capability that requires more than locating one correct stored fact.

## Common classification pitfalls

A benchmark category named “single-hop” is not automatically Direct Retrieval. Labels can be noisy: an item may be tagged as requiring one source turn while actually needing another, or vice versa. Verify the classification by reading the cited turns directly rather than trusting the label alone.
