# Capability 5 — Forgetting and Memory Management

[← Back to capability index](../capabilities.md)

**Core question**: Can the benchmark evaluate whether memory appropriately removes, suppresses,
or deprioritizes information?

**Definition**: irrelevant, superseded, or low-salience information stops influencing future
behavior — via decay, pruning, budgeting, suppression, or interference reduction — rather than
accumulating indefinitely and diluting retrieval quality.

**Motivation**: memory systems cannot retain everything forever at full salience; a system that
treats every stored fact as equally retrievable forever will eventually drown useful
information in noise as the store grows.

**Why it matters**: unmanaged accumulation is a scaling failure mode distinct from a raw
retrieval failure — the *right* fact may still technically be present, but buried under so much
irrelevant accumulated history that it's effectively unreachable or gets outweighed by noise.

**What kinds of benchmark tasks evaluate it**: measuring whether stale or irrelevant facts stop
being surfaced or stop influencing answers over time, as distinct from measuring whether a
*specific* contradiction is resolved correctly.

**What does not belong to this capability**: this is fundamentally different from Memory
Updating ([Capability 4](04-memory-updating.md)) — [Capability 4](04-memory-updating.md) is about correctly answering when a specific new fact
explicitly contradicts an old one; this capability is about *selective forgetting* of
information that was never contradicted, just no longer relevant, salient, or worth the storage
budget. A benchmark testing this must explicitly evaluate deprioritization, not merely reward
retrieving the newer of two facts.

**Typical failure modes**: never deprioritizing anything (unbounded accumulation degrading
retrieval precision at scale — bleeding into [Capability 11](11-retrieval-robustness.md)/13 territory), or over-aggressively
forgetting facts that are still relevant (dropping true long-term preferences because they
weren't restated recently).

**Example benchmark questions**: a benchmark item that never restates a persona detail after
early sessions, then asks (many sessions later, with substantial irrelevant content in
between) a question that only works if seldom-repeated but still-relevant information wasn't
pruned — designed to probe the boundary between healthy pruning and harmful over-forgetting.

**Relationship to other capabilities**: distinct from [Capability 4](04-memory-updating.md) (contradiction-driven
correctness) and overlaps at the edges with [Capability 11](11-retrieval-robustness.md) (retrieval robustness against noise)
and [Capability 13](13-scalability.md) (scalability) — but this capability is specifically about the system's own
active memory-management policy, not just its robustness to externally-injected distractors.

**Mapping to cognitive science**: decay and forgetting-curve theory, plus interference-based
forgetting models — the idea that unused or superseded memories fade or get suppressed rather
than persisting at constant strength.

**Notes about common ambiguities**: a benchmark that never rewards forgetting (because its
metric only measures recall of the full history) provides no capability-5 evidence either
way — the correct verdict there is `None`, not an assumed `Full`, even if the underlying
conversations are long. Don't infer coverage of this capability from conversation length alone.

