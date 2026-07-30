# Capability 1 — Direct Retrieval

[← Back to capability index](../capabilities.md)

**Core question**: Can the benchmark determine whether the system correctly retrieves a single
stored memory?

**Definition**: the answer is fully contained in a single stored fact or turn; retrieving and
returning that one fact (verbatim or paraphrased) is sufficient, with no combination with any
other stored fact required and no reasoning beyond locating the correct memory.

**Motivation**: this is the most fundamental memory capability, and the floor every other
capability builds on — a system that can't reliably do this can't be trusted on anything more
complex layered on top of it.

**Why it matters**: nearly every practical use of conversational memory (recalling a name, a
preference, a stated fact) reduces to this at the lowest level. If a system fails here,
downstream capabilities that assume correct retrieval (synthesis, personalization, updating)
are unverifiable — you can't tell whether a multi-hop failure is a synthesis failure or a
retrieval failure underneath it.

**What kinds of benchmark tasks evaluate it**: factual recall, attribute lookup, remembering
names or locations, and recalling explicitly-stated user preferences — any QA item whose
answer traces to one identifiable source turn.

**What does not belong to this capability**: if answering requires combining ≥2 non-redundant
stored facts, it's Relational Integration ([Capability 2](02-relational-integration.md)), not Direct Retrieval — even if a
single evidence pointer could superficially answer a differently-phrased version of the
question. A **contamination check** applies: the gold answer must not be derivable from the
model's parametric world knowledge alone, without consulting the stored fact, or the benchmark
isn't testing memory at all.

**Typical failure modes**: retrieving a similar-but-wrong fact (name/attribute confusion
between entities), retrieving a stale version of a fact that was later updated (a Capability-4
failure masquerading as a retrieval failure), or failing outright when the fact is buried deep
in a long context.

**Example benchmark questions**: *"What city does the user live in?"* → answerable from one
turn where the user stated their city once; *"What is the user's job?"* → answerable from a
single stated fact, with no other turn needed to derive it.

**Relationship to other capabilities**: the baseline every other capability is defined
*against* — Capabilities 2–14 all specify, in their own boundary rules, how they differ from
this one.

**Mapping to cognitive science**: declarative memory retrieval — recalling a fact that was
explicitly encoded, the most basic operation in the declarative memory system.

**Notes about common ambiguities**: a category labeled "single-hop" or similar by a benchmark's
authors is not automatically Direct Retrieval — always spot-check concrete examples, since
annotation label noise is common: an item can be labeled as requiring only one source turn while
actually needing another, or vice versa. Verify by reading the cited turns directly rather than
trusting the label.

