# Capability 4 — Memory Updating

[← Back to capability index](../capabilities.md)

**Core question**: Can the benchmark determine whether memory correctly reflects changing
information?

**Definition**: when a user later states something that supersedes or contradicts an earlier
stored fact, the system answers using the current fact, not the stale one. This includes
distinguishing between storing both versions, retrieving only the latest version, and (where
relevant) explaining what changed and why.

**Motivation**: memory that never updates degrades into an append-only log of everything ever
said, which is actively harmful once facts change — a system needs a notion of "this
supersedes that," not just "this was said."

**Why it matters**: stale-fact retrieval is a uniquely bad failure mode because it looks
confident and sourced (the old fact really was said) while being wrong for the user's current
situation — e.g. recommending based on an address the user moved away from months ago.

**What kinds of benchmark tasks evaluate it**: changed addresses, changed preferences, explicit
corrections ("actually, I..."), contradictory statements across sessions, and revised plans.

**What does not belong to this capability**: this is not Forgetting/Memory Management
([Capability 5](05-forgetting-and-memory-management.md)) — updating is about *correctness in the face of an explicit contradiction*,
while forgetting is about deprioritizing information that was never contradicted, just no
longer relevant. It is also not Relational Integration ([Capability 2](02-relational-integration.md)): when two facts must be
combined *because* one supersedes the other, that combination is this capability, not
[Capability 2](02-relational-integration.md), which assumes the combined facts are compatible.

**Typical failure modes**: answering with the original fact because it appears earlier/more
often in context, blending both versions into a hedge instead of picking the current one, or
correctly retrieving the new fact but being unable to explain that (or why) it changed when
asked.

**Example benchmark questions**: a canonical format is: session N states "I live in Seattle,"
session N+k states "I just moved to Boston," and the eval question asks "Where does the user
currently live?" — testing whether the system answers "Boston," not "Seattle," and doesn't
merely retrieve whichever mention is more prominent.

**Relationship to other capabilities**: adjacent to [Capability 2](02-relational-integration.md) (both involve reasoning across
multiple stored facts) and [Capability 5](05-forgetting-and-memory-management.md) (both involve a stored fact losing priority over time),
but distinguished from each by the presence of an explicit contradiction requiring resolution,
as opposed to compatible facts ([Capability 2](02-relational-integration.md)) or simple staleness/irrelevance ([Capability 5](05-forgetting-and-memory-management.md)).

**Mapping to cognitive science**: memory updating and interference resolution — how new
learning modifies or competes with previously consolidated memory traces (retroactive
interference).

**Notes about common ambiguities**: a question can look like an update-handling test while
actually just testing recency ([Capability 3](03-temporal-reasoning.md)) if no true contradiction exists — verify the two
statements are genuinely incompatible, not just sequential.

