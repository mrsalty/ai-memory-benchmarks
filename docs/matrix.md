# Benchmark comparison matrix

Columns refer to the 16 capabilities indexed in [`capabilities.md`](capabilities.md) (numbers
match that page's Capability index and each capability's own page under
[`capabilities/`](capabilities/)). Each cell is scored with the Coverage rubric's
Leg-A-gated decision rule — Leg A (authored targeting) is checked first: if the benchmark's
authors never quotably designed an evaluation for a capability's exact core question, the verdict
is ❌ **None**, full stop, regardless of any incidental examples in the data. If Leg A holds,
Legs B (concrete example) and C (metric isolation) decide between ✅ **Full** and 🟡 **Partial**.
See each benchmark's own page in [`benchmarks/`](benchmarks/) for the full per-capability
evidence — this table is a summary view, not a substitute for it.

| Verdict | Glyph | Meaning |
|---|---|---|
| Full | ✅ | Leg A holds; Legs B and C both hold |
| Partial | 🟡 | Leg A holds; Leg B or C falls short |
| None | ❌ | Leg A fails — no authored targeting found |
| Pending | ☐ | not yet checked against this benchmark |

| Capability / Benchmark                   | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
|------------------------------------------|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| [LoCoMo](benchmarks/locomo.md)           | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ |
| [LongMemEval](benchmarks/longmemeval.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ | 🟡 | ✅ | ❌ | ❌ | ❌ |
| [MemoryAgentBench](benchmarks/memoryagentbench.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | 🟡 | ✅ | ❌ | ❌ | ❌ |
| [BEAM](benchmarks/beam.md)               | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ | ❌ | ✅ | ❌ | 🟡 | ✅ | ❌ | ❌ | ❌ |

**Capability key** (full names, linked to each capability's own page):

[1. Direct Retrieval](capabilities/01-direct-retrieval.md) ·

[2. Relational Integration](capabilities/02-relational-integration.md) ·

[3. Temporal Reasoning](capabilities/03-temporal-reasoning.md) ·

[4. Memory Updating](capabilities/04-memory-updating.md) ·

[5. Forgetting and Memory Management](capabilities/05-forgetting-and-memory-management.md) ·

[6. Personalization](capabilities/06-personalization.md) ·

[7. Procedural Memory](capabilities/07-procedural-memory.md) ·

[8. Knowledge Abstraction](capabilities/08-knowledge-abstraction.md) ·

[9. Episodic vs Semantic Understanding](capabilities/09-episodic-vs-semantic-understanding.md) ·

[10. Memory Calibration](capabilities/10-memory-calibration.md) ·

[11. Retrieval Robustness](capabilities/11-retrieval-robustness.md) ·

[12. Persistence](capabilities/12-persistence.md) ·

[13. Scalability](capabilities/13-scalability.md) ·

[14. Associative Retrieval](capabilities/14-associative-retrieval.md) ·

[15. Memory Formation and Write Fidelity](capabilities/15-memory-formation-and-write-fidelity.md) ·

[16. Memory Provenance and Source Attribution](capabilities/16-memory-provenance-and-source-attribution.md)

This matrix deliberately contains only the grid, legend, and capability key. For metric detail,
blind spots, and primary-source reasoning, see each benchmark's own page under
[`benchmarks/`](benchmarks/).

## Adding a benchmark

1. Add a page under `benchmarks/<name>.md` following [`benchmarks/locomo.md`](benchmarks/locomo.md)
   as the reference template: metadata, description, dataset statistics, task format, metric,
   sample data, capability scoring (all 16 capabilities, Leg-A-gated), open questions.
2. Add a row to the table above using the same glyphs, sourced directly from that page's
   Capability scoring table — don't re-derive verdicts here, copy them.
3. Once ≥2 benchmarks are scored, add real cross-benchmark headline findings back to this page,
   each citing the specific benchmark pages it's drawn from.
