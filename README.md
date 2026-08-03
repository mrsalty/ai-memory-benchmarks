# ai-memory-benchmarks

A catalog of public benchmarks for AI/software memory systems (persistent memory for
LLM-based conversational agents and assistants), compared not just by what's available but by
**which memory capabilities each one actually measures — and which it doesn't**.

Most benchmark leaderboards answer "which system scores highest." This repo answers a
different question: for any given benchmark, what does a high score actually tell you, and
what does it stay silent on? A benchmark can score presence-in-top-k accuracy while being
structurally blind to whether the system also retrieved a pile of irrelevant noise alongside
the right answer — that gap is invisible unless someone maps it out.

## Contents

- [`docs/capabilities.md`](docs/capabilities.md) — index into a 16-capability taxonomy
  (Direct Retrieval, Relational Integration, Temporal Reasoning, Memory Updating, and so on),
  plus a Coverage rubric with a deterministic, Leg-A-gated decision rule (✅ Full / 🟡 Partial /
  ❌ None / ☐ Pending). Each capability's full specification — definition, boundary rules,
  failure modes, cognitive-science mapping — lives in its own page under
  [`docs/capabilities/`](docs/capabilities/). These pages describe only what a capability *is*;
  they name no specific benchmark.
- [`docs/benchmarks/`](docs/benchmarks/) — one page per benchmark: task format, metric, a
  capability-by-capability scoring with cited justification, and known blind spots. This is
  where benchmark-specific evidence lives.
- [`docs/matrix.md`](docs/matrix.md) — the benchmark × capability comparison matrix, using the
  same glyphs as the capability pages. Currently covers verified LoCoMo, LongMemEval, and
  MemoryAgentBench rows; grows one column-complete row at a time as more benchmarks are verified.

## Status

Rigor is applied one capability and one benchmark at a time — a ☐ `Pending` cell is more honest
than a guessed one. As it stands:

- **Capabilities**: 1 (Direct Retrieval) and 2 (Relational Integration) have finalized
  definitions, boundary rules, and verification criteria. Capabilities 3–16 have draft
  descriptions only.
- **Benchmarks**: [LoCoMo](docs/benchmarks/locomo.md),
  [LongMemEval](docs/benchmarks/longmemeval.md), and
  [MemoryAgentBench](docs/benchmarks/memoryagentbench.md) are verified against primary sources and
  fully scored across all 16 capabilities. Every verdict is checked against the paper, released
  dataset, and/or evaluator code rather than guessed.

## Scope

This catalogs benchmarks for **AI/software systems**, not human cognition. Cognitive
psychology's memory taxonomy (episodic/semantic/procedural, working vs. long-term, etc.) is
used only as a reference lens for naming and validating capabilities — see
[`docs/capabilities.md`](docs/capabilities.md) for where that mapping holds and where it breaks
down, and for the cog-sci phenomena no benchmark here tests at all.

## Methodology

Every capability verdict for a benchmark is decided by checking three proof legs, in order,
against **primary sources** (the actual paper and released dataset/code, not general knowledge
of the benchmark) — never a subjective impression:

- **Leg A — Authored targeting** (checked first, and gates everything else): can you *quote*
  the benchmark's authors defining a category or task that specifically targets this
  capability's core question? If not, the verdict is ❌ **None**, regardless of any incidental
  examples that might exist in the data — byproduct coverage the authors never designed for
  isn't a reliable signal.
- **Leg B — Concrete demonstration**: a real, traceable example from the released dataset
  showing the capability is genuinely required, verified by reading the example itself.
- **Leg C — Metric isolation**: whether the benchmark's metric produces a score reflecting this
  capability specifically, not blended into an aggregate or actually measuring a different
  property under a similar name.

If Leg A holds and both B and C hold, the verdict is ✅ **Full**; if Leg A holds but B or C falls
short, it's 🟡 **Partial**. Not yet checked is ☐ **Pending** — always more honest than a guess.

### Evaluation process

All benchmark evaluations in this catalog are conducted with support from **OpenAI GPT Terra** and
then reviewed for accuracy by the project maintainer (a human). GPT-assisted analysis does not
replace the primary-source evidence requirements above: every published verdict is human-reviewed
against the cited paper, released dataset, and/or evaluation code.

Benchmark and dataset licenses are noted per page and are separate from this repo's own MIT
license.

## Contributing

To propose a new benchmark, open a PR adding a page under `docs/benchmarks/<name>.md` with the
benchmark's official facts — paper, official code/data repo, license, dataset statistics, task
format, and metric — cited only to official sources (the paper, the dataset/code repo, or the
project's own website), following [`docs/benchmarks/locomo.md`](docs/benchmarks/locomo.md) as
the reference template. Capability scoring against [`docs/capabilities.md`](docs/capabilities.md)
is filled in during review, not required in the initial PR.
