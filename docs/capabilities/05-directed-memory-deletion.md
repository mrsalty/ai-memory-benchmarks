# Capability 5 — Directed Memory Deletion

[← Back to capability index](../capabilities.md)

## At a glance

| | |
|---|---|
| **Core question** | Can the benchmark determine whether a system stops retrieving and using a specifically identified memory after an explicit deletion instruction? |
| **Definition** | after an authorized user or system instruction identifies a stored item for deletion, the system no longer retrieves, discloses, or uses that item in later behavior, while unrelated memories that were not named for deletion remain available. The observable target is selective non-retrieval and non-use; a benchmark may claim physical erasure only if it also inspects the memory state or a deletion audit. |
| **Cognitive-science lens** | no direct equivalent: deliberate deletion is a software-memory control operation, not ordinary human forgetting. |

## Why this capability matters

software memory systems may retain personal, obsolete, sensitive, or incorrect information
long after the user wants it removed. A memory system is not controllable if a deletion request
can be acknowledged while the targeted information remains available to retrieval or still shapes
later responses.

selectivity is essential: a test that only observes a missing target cannot distinguish successful
deletion from a broken memory system. Retained control memories establish that the system removed
the requested item rather than losing access to memory generally.

## What a benchmark must require

an explicit deletion instruction that unambiguously identifies a memory; a later direct probe for
that memory; a later task on which the deleted memory would have changed the output if it remained
available; and at least one unrelated retained control memory. The benchmark must score both
non-retrieval/non-use of the target and continued availability of the control.

## Boundaries and exclusions

this is not Memory Updating ([Capability 4](04-memory-updating.md)): a later contradictory fact
changes which fact is current, but does not direct removal of either record. It is not Memory
Calibration ([Capability 10](10-memory-calibration.md)): abstaining because no evidence is present
does not establish why evidence is absent. It is not Retrieval Robustness
([Capability 11](11-retrieval-robustness.md)): avoiding irrelevant distractors does not establish
that they were deleted. It is not Memory Formation and Write Fidelity
([Capability 15](15-memory-formation-and-write-fidelity.md)): information that was never retained
cannot have been deleted. Time-based expiry, passive decay, capacity eviction, and relevance
ranking are outside this capability unless an explicit deletion instruction is the evaluated event.

### How to classify a test item

Use this quick check when evaluating a candidate item:

1. Does the item require the behavior described by this capability's core question?
2. Does the item avoid the adjacent behaviors excluded above?
3. Does the benchmark score that behavior rather than merely making it available in the data?

Read the actual task, released example, and metric before assigning a category. A task label alone is useful evidence, not proof.

## Common failure modes

acknowledging a deletion request but later disclosing the deleted information; continuing to use it
in recommendations or decisions without naming it; deleting a similarly named but different
memory; or deleting unrelated control memories along with the target.

## Illustrative benchmark item

> **Status:** Hypothetical example—not evidence that a specific benchmark measures this capability.
>
> **Memory:** “My old home address was 14 Oak Street.” Separately: “I prefer vegetarian meals.”
>
> **Deletion instruction:** “Forget my old home address. Do not retain or use it again.”
>
> **Task:** Later, ask for the old address and ask for a meal recommendation.
>
> **Expected behavior:** Do not retrieve, disclose, or use the old address; retain and use the vegetarian preference.
>
> **Why this capability:** The instruction explicitly targets one existing memory for removal, and the retained preference distinguishes successful deletion from general memory loss.

## Relationship to other capabilities
depends on Direct Retrieval ([Capability 1](01-direct-retrieval.md)) to establish that the target
and control existed before deletion, but then requires the inverse outcome only for the named
target. It is adjacent to Memory Updating ([Capability 4](04-memory-updating.md)), Memory
Calibration ([Capability 10](10-memory-calibration.md)), Retrieval Robustness
([Capability 11](11-retrieval-robustness.md)), Persistence ([Capability 12](12-persistence.md)),
and Memory Formation and Write Fidelity ([Capability 15](15-memory-formation-and-write-fidelity.md));
the boundary rules above distinguish their different observable claims.

## Common classification pitfalls
a black-box "not found" response establishes only observable non-retrieval, not physical erasure;
claim actual deletion only with state inspection or an auditable deletion interface. A task that
asks the system to stop using a fact without first establishing that it was stored and without
retained controls cannot distinguish deletion from failed retention or retrieval. Do not infer
coverage from contradiction resolution, long histories, distractors, TTL-like timestamps, or a
generic privacy claim unless the benchmark explicitly evaluates a directed deletion operation.
