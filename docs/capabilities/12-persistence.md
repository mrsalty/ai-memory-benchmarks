# Capability 12 — Persistence

[← Back to capability index](../capabilities.md)

**Core question**: Can the benchmark determine whether memories survive across independent
interactions?

**Definition**: memory formed in one session remains available and correctly used in a later,
genuinely separate session or after an application restart — as opposed to being available only
because it's still within one continuous context window.

**Motivation**: "remembering across sessions" is the headline promise of persistent memory
systems, and it's a distinct engineering/behavioral claim from simply handling a long single
conversation well.

**Why it matters**: a system that performs well only within one uninterrupted context but loses
everything on session boundary or restart isn't providing persistent memory at all — it's
providing long-context handling, a different (and less demanding) capability.

**What kinds of benchmark tasks evaluate it**: tasks explicitly split across separate sessions,
application restarts, or otherwise structurally separated interactions, where the evaluation
specifically checks whether information from an earlier, closed session is available in a
later, independently-initiated one.

**What does not belong to this capability**: a long single context window is **not** persistent
memory, however long it is — if a benchmark's "sessions" are actually just concatenated into one
continuous prompt at eval time, it is testing long-context handling (related to [Capability 13](13-scalability.md)),
not this capability. This capability specifically requires a genuine boundary — a new session,
process, or context — between when the memory was formed and when it's used.

**Typical failure modes**: correctly using information within the session it was stated in, but
losing it once a new session starts; or requiring the entire prior conversation history to be
re-fed as context (rather than a persisted memory store) to "simulate" persistence, which
doesn't actually test cross-session storage.

**Example benchmark questions**: a fact is stated in one session; a separate, independently
initiated session (not the same context, no replayed history) later asks a question that can
only be answered correctly if that earlier fact was actually persisted and retrieved, rather
than re-supplied as part of the prompt.

**Relationship to other capabilities**: the necessary precondition for every other capability
to matter in a genuinely long-term-memory sense, rather than a long-context sense; closely
related to, but distinct from, [Capability 13](13-scalability.md) (scalability is about performance as *volume*
grows, independent of whether that volume crosses a session boundary).

**Mapping to cognitive science**: a weak analogue to memory consolidation over time — the
process by which memories become stable and durable rather than existing only transiently — but
the sw-sys framing here (storage architecture surviving a process boundary) doesn't map cleanly
onto biological consolidation.

**Notes about common ambiguities**: many benchmarks describe their data as "multi-session" while
their *evaluation protocol* still feeds the full conversation history as one prompt at inference
time — that's a genuine gap between what the dataset supports testing and what the benchmark's
own eval code actually tests. Verify the eval harness, not just the dataset structure.

