# Capability 16 — Memory Provenance and Source Attribution



## At a glance

| | |
|---|---|
| **Core question** | Can the benchmark determine whether the system correctly identifies where a recalled memory came from? |
| **Definition** | when using or reporting remembered information, the system correctly attributes it to its source — such as the speaker, interaction, session, turn, document, or an explicitly marked inference — and does not present a claim from one source as though it came from another. The source attribution itself must be required and scored, not merely available as hidden dataset metadata. |
| **Cognitive-science lens** | source monitoring — distinguishing the origin of remembered information, including whether it was perceived, communicated by a particular person, inferred, or imagined. |

## Why this capability matters

content-only correctness is insufficient when the provenance of a claim determines whether it should be trusted, corrected, or acted on. A system can repeat a true statement while misattributing who said it, when it was learned, or whether it was directly observed versus inferred.

source confusion creates high-impact failures in multi-speaker and multi-session memory: one user's preference can be assigned to another, an assistant inference can be presented as a user statement, or an outdated source can be treated as current evidence. Provenance also enables users and evaluators to audit, challenge, and correct memory-backed responses.

## What a benchmark must require

questions that require both a recalled claim and its source; attribution choices among plausible speakers, sessions, turns, or documents; and tasks that distinguish direct statements from system inferences. The metric must score correct provenance separately from, or jointly and unambiguously with, the content claim.

## Boundaries and exclusions

dataset evidence pointers used only by evaluators to validate an answer do not evaluate provenance unless the evaluated system must output or select them and is scored for doing so. Naming a speaker in a question is not enough if the gold answer requires only content. Memory Calibration ([Capability 10](10-memory-calibration.md)) asks whether the system knows that supporting memory is absent; this capability asks whether it identifies the correct source when support is present. Temporal Reasoning ([Capability 3](03-temporal-reasoning.md)) asks when an event occurred, not which source established it.

### How to classify a test item

Use this quick check when evaluating a candidate item:

1. Does the item require the behavior described by this capability's core question?
2. Does the item avoid the adjacent behaviors excluded above?
3. Does the benchmark score that behavior rather than merely making it available in the data?

Read the actual task, released example, and metric before assigning a category. A task label alone is useful evidence, not proof.

## Common failure modes

assigning one speaker's statement to another; confusing a direct user statement with an assistant-generated summary or inference; reporting the right fact with the wrong session or document citation; or attributing a memory to a plausible but unsupported source.

## Example

two speakers make similar but conflicting statements in separate sessions; a later query asks both for the current relevant fact and which speaker or turn established it. Another item presents an assistant inference derived from several turns and asks the system to distinguish that inference from a fact directly stated by the user.

## Relationship to other capabilities

Direct Retrieval ([Capability 1](01-direct-retrieval.md)) can establish that a system recovered the right content, but not that it knows the content's origin. Memory Updating ([Capability 4](04-memory-updating.md)) may require comparing sources to resolve a change, but does not require reporting provenance. This capability complements Memory Formation and Write Fidelity ([Capability 15](15-memory-formation-and-write-fidelity.md)): preserving source metadata at write time enables later attribution, while this capability evaluates the observable attribution outcome.

## Common classification pitfalls

a benchmark that stores speaker names, timestamps, dialogue IDs, or evidence spans but scores only answer text provides no provenance-coverage evidence. To score this capability above `None`, verify an authored task specifically requiring attribution and a metric that penalizes correct-content/wrong-source responses.
