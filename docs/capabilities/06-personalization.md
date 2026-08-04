# Capability 6 — Personalization

[← Back to capability index](../capabilities.md)

## At a glance

| | |
|---|---|
| **Core question** | Can the benchmark evaluate whether remembered user information influences future responses? |
| **Definition** | stored knowledge about a user — preferences, habits, goals, long-term projects, recurring constraints — is not just recallable but actively used to shape a *different*, non-recall response (a recommendation, a suggestion, a plan) than the system would have produced without it. |
| **Cognitive-science lens** | self-referential / autobiographical memory — knowledge that is organized around the self and used to guide self-relevant decisions and behavior. |

## Why this capability matters

personalization is the point where memory stops being a lookup feature and
starts changing what the system actually does — the differentiator between "the assistant
remembers I'm vegetarian" and "the assistant only recommends vegetarian restaurants because it
remembers."

a system can pass every recall-style test and still never *apply* what it
recalled; personalization is the capability that catches that gap, since it requires the memory
to visibly change downstream behavior, not just be repeatable on demand.

## What a benchmark must require

recommendation tasks that should be shaped by a
previously-stated preference, planning tasks that should respect a previously-stated
constraint, or any generation task whose "correct" output differs depending on a remembered
user trait.

## Boundaries and exclusions

this is not simple recall — a question that
directly asks "what did the user say their preference was?" is Direct Retrieval ([Capability 1](01-direct-retrieval.md)),
even if the content happens to be a preference. Personalization requires the memory to
influence a *different* downstream task, not just be repeated back.

### How to classify a test item

Use this quick check when evaluating a candidate item:

1. Does the item require the behavior described by this capability's core question?
2. Does the item avoid the adjacent behaviors excluded above?
3. Does the benchmark score that behavior rather than merely making it available in the data?

Read the actual task, released example, and metric before assigning a category. A task label alone is useful evidence, not proof.

## Common failure modes

correctly recalling the preference when asked directly, but failing
to apply it unprompted in a task where it should have mattered (e.g. still recommending a
meat dish); or over-applying a one-off statement as if it were a stable preference (bleeding
into [Capability 9](09-episodic-vs-semantic-understanding.md) territory).

## Illustrative benchmark item

> **Status:** Hypothetical example—not evidence that a specific benchmark measures this capability.
>
> **Memory:** “I avoid dairy.”
>
> **Task:** “Suggest a dessert for me.”
>
> **Expected behavior:** Suggest a dairy-free dessert without being prompted to restate the preference.
>
> **Why this capability:** The remembered preference must change a separate recommendation, rather than simply be recalled.

## Relationship to other capabilities

builds on [Capability 1](01-direct-retrieval.md) (retrieval of the underlying
preference fact) and overlaps with [Capability 9](09-episodic-vs-semantic-understanding.md) (whether that preference is a stable trait vs.
a one-off statement) — a benchmark can be Full on [Capability 1](01-direct-retrieval.md) (it can retrieve the stated
preference) while still being `None` on Capability 6 (it never checks whether the preference
actually changes any output).

## Common classification pitfalls

many benchmarks that appear to test personalization are
actually only testing [Capability 1](01-direct-retrieval.md) in disguise (asking the model to state the remembered
preference rather than checking whether a separate generation task respects it) — read the
actual task format carefully before scoring Full here.
