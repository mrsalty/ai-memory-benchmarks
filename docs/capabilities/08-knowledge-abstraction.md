# Capability 8 — Knowledge Abstraction

[← Back to capability index](../capabilities.md)

**Core question**: Can the benchmark evaluate whether repeated observations become generalized
knowledge?

**Definition**: the system forms a higher-level concept from multiple individual experiences or
observations — repeated observations becoming a stable preference, repeated interactions
becoming a recognized user habit, or repeated events becoming a semantic concept — rather than
just retrieving the individual observations that fed into it.

**Motivation**: some of the most valuable things a long-term memory system could know about a
user (their habits, their general tendencies) are never stated as a single fact; they only
become knowable by noticing a pattern across many separate mentions.

**Why it matters**: without this capability, a system can only ever know what was explicitly
told to it, never what can be *inferred* from a pattern of behavior — a meaningfully weaker
form of understanding than what long-term human relationships develop.

**What kinds of benchmark tasks evaluate it**: tasks that require noticing a pattern across
several separate, individually-unremarkable mentions and stating the generalized conclusion
(e.g., inferring "the user is training for a marathon" from several separate mentions of long
runs, never stated as such directly).

**What does not belong to this capability**: this is distinct from Relational Integration
([Capability 2](02-relational-integration.md)) — multi-hop combines a *fixed, small number* of existing explicit facts into one
answer; abstraction *creates a new semantic representation* from a pattern across many
observations, where no single combination of a couple of facts would produce the same
conclusion. It's also distinct from [Capability 9](09-episodic-vs-semantic-understanding.md) (episodic vs. semantic) — that capability is
about *not conflating* a one-off event with a generalization, whereas this capability is about
whether the generalization gets *formed* correctly from real repetition in the first place.

**Typical failure modes**: over-abstracting from a single instance (treating one mention as a
pattern — a [Capability 9](09-episodic-vs-semantic-understanding.md) boundary violation), never forming the abstraction at all even when
the pattern is clearly present across many mentions, or forming an abstraction but being unable
to cite the observations that support it.

**Example benchmark questions**: multiple separate mentions of running long distances on
weekends, in different sessions, followed by a question like "Is the user likely training for
something?" — correctly answering "probably a marathon" requires abstraction across all the
mentions, not retrieval of any single one.

**Relationship to other capabilities**: builds on [Capability 1](01-direct-retrieval.md) (the individual observations
must first be retrievable) and is bounded by [Capability 9](09-episodic-vs-semantic-understanding.md) (the abstraction must be
appropriately scoped, not over-generalized from too little evidence); distinct from [Capability 2](02-relational-integration.md)
in requiring pattern formation across many instances rather than combination of a couple of
discrete facts.

**Mapping to cognitive science**: schema formation and semantic memory consolidation — the
process by which repeated episodic experiences give rise to generalized semantic knowledge,
independent of any single originating episode.

**Notes about common ambiguities**: the line between "abstraction" and "well-annotated
multi-hop with unusually many evidence citations" can blur — the deciding factor is whether the
conclusion requires genuine pattern recognition across the observations (this capability) or
is fully determined by logically combining a small fixed set of them ([Capability 2](02-relational-integration.md)).

