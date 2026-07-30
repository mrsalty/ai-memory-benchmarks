# Capability 9 — Episodic vs Semantic Understanding

[← Back to capability index](../capabilities.md)

**Core question**: Can the benchmark determine whether specific experiences and generalized
knowledge are correctly distinguished?

**Definition**: the system distinguishes a specific one-off event ("I went to Paris last
March") from a generalized or standing fact ("I love traveling to Europe"), neither
over-generalizing from a single episode nor under-generalizing (failing to recognize an
established pattern as general).

**Motivation**: not everything worth remembering is the same *kind* of memory — a single
vacation is not the same as a travel preference, and a system needs to keep those categories
separate to reason about the user correctly.

**Why it matters**: conflating the two produces two different failure directions: assuming a
single event implies an ongoing preference (recommending Europe trips forever because of one
mentioned vacation), or failing to recognize a genuinely repeated pattern as a stable trait.

**What kinds of benchmark tasks evaluate it**: questions that require correctly classifying
whether a piece of information is a specific instance or a general pattern, or that penalize a
system for over-/under-generalizing between the two.

**What does not belong to this capability**: this is not about whether the abstraction gets
*formed* (that's [Capability 8](08-knowledge-abstraction.md)) — it's about whether, once formed or presented, episodic and
semantic information are kept correctly separated and not conflated. A single Direct Retrieval
([Capability 1](01-direct-retrieval.md)) of a one-off statement is not this capability unless the benchmark specifically
tests whether the system wrongly treats that one-off statement as a general trait (or vice
versa).

**Typical failure modes**: one purchase treated as an ongoing shopping habit, one complaint
treated as persistent dissatisfaction, or a genuinely repeated pattern dismissed as a
coincidence.

**Example benchmark questions**: a user mentions a single vacation to Italy once; a later
question probes whether the system incorrectly infers a general "loves Italy" travel preference
from that single mention, versus correctly treating it as a one-off episodic fact.

**Relationship to other capabilities**: the natural boundary check for [Capability 8](08-knowledge-abstraction.md) (knowledge
abstraction) — [Capability 8](08-knowledge-abstraction.md) asks whether a genuine pattern gets abstracted into general
knowledge; Capability 9 asks whether a *non*-pattern (a single episode) is correctly *not*
treated the same way, and vice versa.

**Mapping to cognitive science**: the classic episodic/semantic memory subdivision within
declarative memory — episodic memory for specific, contextually-situated experiences; semantic
memory for generalized, context-free knowledge.

**Notes about common ambiguities**: how many repetitions justify treating something as a
"pattern" rather than a coincidence is inherently a judgment call — a benchmark should state its
own threshold explicitly (e.g. "mentioned in ≥3 separate sessions") for this capability's
verdict to be scored objectively rather than subjectively.

