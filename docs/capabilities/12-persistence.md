# Capability 12 — Persistence

[← Back to capability index](../capabilities.md)

## At a glance

| | |
|---|---|
| **Core question** | Can the benchmark determine whether memories survive across independent interactions? |
| **Definition** | memory formed in one session remains available and correctly used in a later, genuinely separate session or after an application restart — as opposed to being available only because it's still within one continuous context window. |
| **Cognitive-science lens** | a weak analogue to memory consolidation over time — the process by which memories become stable and durable rather than existing only transiently — but the sw-sys framing here (storage architecture surviving a process boundary) doesn't map cleanly onto biological consolidation. |

## Why this capability matters

"remembering across sessions" is the headline promise of persistent memory
systems, and it's a distinct engineering/behavioral claim from simply handling a long single
conversation well.

a system that performs well only within one uninterrupted context but loses
everything on session boundary or restart isn't providing persistent memory at all — it's
providing long-context handling, a different (and less demanding) capability.

## What a benchmark must require

tasks explicitly split across separate sessions,
application restarts, or otherwise structurally separated interactions, where the evaluation
specifically checks whether information from an earlier, closed session is available in a
later, independently-initiated one.

## Boundaries and exclusions

a long single context window is **not** persistent
memory, however long it is — if a benchmark's "sessions" are actually just concatenated into one
continuous prompt at eval time, it is testing long-context handling (related to [Capability 13](13-scalability.md)),
not this capability. This capability specifically requires a genuine boundary — a new session,
process, or context — between when the memory was formed and when it's used.

### How to classify a test item

Use this quick check when evaluating a candidate item:

1. Does the item require the behavior described by this capability's core question?
2. Does the item avoid the adjacent behaviors excluded above?
3. Does the benchmark score that behavior rather than merely making it available in the data?

Read the actual task, released example, and metric before assigning a category. A task label alone is useful evidence, not proof.

## Common failure modes

correctly using information within the session it was stated in, but
losing it once a new session starts; or requiring the entire prior conversation history to be
re-fed as context (rather than a persisted memory store) to "simulate" persistence, which
doesn't actually test cross-session storage.

## Illustrative benchmark item

> **Status:** Hypothetical example—not evidence that a specific benchmark measures this capability.
>
> **Memory:** In session one, the user states a preferred meeting time. The application then closes and starts a new session without replaying the old transcript.
>
> **Task:** In the new session, schedule a meeting for the user.
>
> **Expected behavior:** Use the earlier preference from persisted memory.
>
> **Why this capability:** The task requires a genuine session or process boundary; a single long prompt would not test persistence.

## Relationship to other capabilities

the necessary precondition for every other capability
to matter in a genuinely long-term-memory sense, rather than a long-context sense; closely
related to, but distinct from, [Capability 13](13-scalability.md) (scalability is about performance as *volume*
grows, independent of whether that volume crosses a session boundary).

## Common classification pitfalls

many benchmarks describe their data as "multi-session" while
their *evaluation protocol* still feeds the full conversation history as one prompt at inference
time — that's a genuine gap between what the dataset supports testing and what the benchmark's
own eval code actually tests. Verify the eval harness, not just the dataset structure.
