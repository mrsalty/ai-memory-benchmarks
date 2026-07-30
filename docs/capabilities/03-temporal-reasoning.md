# Capability 3 — Temporal Reasoning

[← Back to capability index](../capabilities.md)

**Core question**: Can the benchmark evaluate whether memories are interpreted correctly with
respect to time?

**Definition**: the system reasons over temporal properties of stored memories — ordering,
recency, durations, historical/"as of" state, or before/after relationships — using
time-related cues in the conversation (explicit dates, relative expressions, or session
ordering), rather than simply reciting a stored timestamp.

**Motivation**: conversational memory accumulates over time, so questions about *when* things
happened, or what was true *at a given point*, are a distinct reasoning demand from questions
about *what* happened — a system can get the facts right and still get the timeline wrong.

**Why it matters**: users routinely ask time-relative questions ("what did I say before my
trip," "is this still current"), and a system that can't place facts on a timeline will give
technically-sourced but contextually wrong answers.

**What kinds of benchmark tasks evaluate it**: event ordering, recency judgments, duration
computation, "as of time T" queries, and before/after relationship questions.

**What does not belong to this capability**: if the temporal placement is incidental to a
question whose real demand is combining two unrelated facts into a new relational conclusion,
that's Relational Integration ([Capability 2](02-relational-integration.md)), not Temporal Reasoning — this capability requires
that reasoning about *time itself* (not just using time-stamped facts) be the point of the
question. Simply recalling an explicitly-stated timestamp verbatim, with no ordering/duration/
recency computation involved, is Direct Retrieval ([Capability 1](01-direct-retrieval.md)).

**Typical failure modes**: conflating "recently mentioned" with "recently true," miscomputing
relative dates ("the week before X"), or answering with the wrong session's timeframe when
multiple candidate events could match.

**Example benchmark questions**: *"When did the user start their new job?"* → *"the Monday
before their birthday"* — answerable only by resolving a relative time expression against
another stated date, not by reciting a verbatim timestamp.

**Relationship to other capabilities**: distinct from Relational Integration ([Capability 2](02-relational-integration.md)) in
requiring a temporal computation rather than a new non-temporal relational fact; distinct from
Memory Updating ([Capability 4](04-memory-updating.md)) in that no contradiction/supersession is involved — the facts
being ordered are all still true, just at different points on a timeline.

**Mapping to cognitive science**: temporal source memory — the aspect of episodic memory that
encodes *when* an experience occurred, distinct from encoding *what* occurred.

**Notes about common ambiguities**: date-arithmetic questions that also require picking the
*right* fact out of several candidates blur into [Capability 2](02-relational-integration.md) territory — the dividing line is
whether the extra work is temporal computation (this capability) or a non-temporal relational
inference ([Capability 2](02-relational-integration.md)).

