# Capability 10 — Memory Calibration

[← Back to capability index](../capabilities.md)

**Core question**: Can the benchmark evaluate whether the system understands the limits of its
own memory?

**Definition**: when the answer isn't present in memory, the system says so (abstains,
expresses uncertainty, or asks for clarification) rather than producing a plausible-sounding
but unsupported answer.

**Motivation**: a memory system's usefulness depends not just on what it can retrieve, but on
whether it can be trusted to flag the cases where it has nothing to retrieve — the difference
between a system that's *usually* right and one that's *reliably* right about what it knows.

**Why it matters**: unflagged hallucination on missing memories is arguably worse than simply
failing to retrieve, because it's indistinguishable from a correct answer to the user without
independent verification — this capability is what catches that specific failure mode.

**What kinds of benchmark tasks evaluate it**: questions deliberately constructed so the answer
is *not* present in the stored memory, checking whether the system abstains/expresses
uncertainty rather than confabulating; adversarial or "unanswerable" question categories.

**What does not belong to this capability**: this is not a retrieval-accuracy capability — a
system can score Full on Capabilities 1–9 (always correct when the answer is present) and still
score `None` here if it has no mechanism for recognizing when the answer is *absent*. Getting an
answerable question right is not evidence for or against this capability.

**Typical failure modes**: hallucinating a specific, confident-sounding but unsupported answer
when memory has nothing relevant; or the opposite failure — over-abstaining on questions that
were actually answerable, which trades hallucination risk for uselessness.

**Example benchmark questions**: a question constructed so that memory contains no supporting
fact at all (e.g. asking for a preference the user never stated), designed to trick the system
into providing a plausible-sounding wrong answer, with the expectation that it instead
recognizes the question as unanswerable from memory.

**Relationship to other capabilities**: cuts across every other capability rather than sitting
next to one specific neighbor — any capability's "Full" scoring on an answerable item says
nothing about whether the same system would correctly abstain on an unanswerable one; the two
need to be tested and scored separately.

**Mapping to cognitive science**: metamemory, specifically the feeling-of-knowing
phenomenon — the (often accurate) sense of whether one knows something, prior to or instead of
actually retrieving it.

**Notes about common ambiguities**: distinguish "the system says it doesn't know" (calibration
working correctly) from "the system gives a hedged but still wrong answer" (calibration failure
dressed up as caution) — a benchmark's metric needs to actually check for the former, not just
detect the absence of a confident wrong answer.

