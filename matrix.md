# Benchmark × Capability Matrix

## How to read this matrix

Each row is a verified benchmark; each numbered column is one of the 16 capabilities in the [capability taxonomy](docs/capabilities.md). A cell summarizes the benchmark's evidence for that capability—not the quality of any system evaluated on the benchmark. Use the [benchmark analysis pages](docs/benchmarks/) for the source-backed reasoning behind each verdict.

| Verdict | Meaning |
|---|---|
| ✅ **Full** | The benchmark explicitly targets the capability, includes a traceable example, and scores it in isolation. |
| 🟡 **Partial** | The benchmark explicitly targets the capability, but its evidence or metric falls short. |
| ❌ **None** | The benchmark authors did not explicitly design an evaluation for the capability. |
| ☐ **Pending** | The capability has not yet been evaluated for this benchmark. |

> **Scoring rule:** authored targeting is required. If the authors do not explicitly target a capability, the verdict is ❌ **None**—even if an incidental dataset example appears related. When authored targeting holds, a concrete released-data example and metric isolation distinguish ✅ **Full** from 🟡 **Partial**.

## Coverage

| Benchmark ↓ / Capability → | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
|---|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| [LoCoMo](docs/benchmarks/locomo.md) | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ |
| [LongMemEval](docs/benchmarks/longmemeval.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ | 🟡 | ✅ | ❌ | ❌ | ❌ |
| [MemoryAgentBench](docs/benchmarks/memoryagentbench.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | 🟡 | ✅ | ❌ | ❌ | ❌ |
| [BEAM](docs/benchmarks/beam.md) | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ | ❌ | ✅ | ❌ | 🟡 | ✅ | ❌ | ❌ | ❌ |

## Capability lookup

| # | Capability | # | Capability |
|:-:|---|:-:|---|
| 1 | [Direct Retrieval](docs/capabilities/01-direct-retrieval.md) | 9 | [Episodic vs Semantic Understanding](docs/capabilities/09-episodic-vs-semantic-understanding.md) |
| 2 | [Relational Integration (Multi-hop)](docs/capabilities/02-relational-integration.md) | 10 | [Memory Calibration](docs/capabilities/10-memory-calibration.md) |
| 3 | [Temporal Reasoning](docs/capabilities/03-temporal-reasoning.md) | 11 | [Retrieval Robustness](docs/capabilities/11-retrieval-robustness.md) |
| 4 | [Memory Updating](docs/capabilities/04-memory-updating.md) | 12 | [Persistence](docs/capabilities/12-persistence.md) |
| 5 | [Directed Memory Deletion](docs/capabilities/05-directed-memory-deletion.md) | 13 | [Scalability](docs/capabilities/13-scalability.md) |
| 6 | [Personalization](docs/capabilities/06-personalization.md) | 14 | [Associative Retrieval](docs/capabilities/14-associative-retrieval.md) |
| 7 | [Procedural Memory](docs/capabilities/07-procedural-memory.md) | 15 | [Memory Formation and Write Fidelity](docs/capabilities/15-memory-formation-and-write-fidelity.md) |
| 8 | [Knowledge Abstraction](docs/capabilities/08-knowledge-abstraction.md) | 16 | [Memory Provenance and Source Attribution](docs/capabilities/16-memory-provenance-and-source-attribution.md) |

## Adding a benchmark

1. Create `docs/benchmarks/<name>.md`, using [LoCoMo](docs/benchmarks/locomo.md) as the reference template. Include official metadata, license, dataset statistics, task format, metric, sample data, capability scoring, and open questions.
2. Verify every capability verdict against primary sources using the scoring rule above.
3. Add a row here by copying the verified verdicts from the benchmark page; do not re-derive them in the matrix.
