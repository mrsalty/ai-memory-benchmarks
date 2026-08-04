# AI Memory Benchmarks

**A capability-first catalog of public benchmarks for AI memory systems.**

This project asks a question that benchmark leaderboards usually do not: **what does a high score
actually demonstrate—and what does it leave untested?**

A benchmark can accurately measure whether a system found a relevant memory while remaining silent
on whether it retrieved excessive noise, updated stale facts, preserved provenance, or worked at
scale. This catalog makes those boundaries explicit.

## Start here

| Resource | Use it to… |
|---|---|
| [Comparison matrix](matrix.md) | Compare every verified benchmark across all 16 capabilities. |
| [Capability taxonomy](docs/capabilities.md) | Understand the capabilities, scoring rubric, and scope. |
| [Benchmark analyses](docs/benchmarks/) | Inspect task formats, metrics, primary-source evidence, and blind spots. |

## What is covered

The catalog covers benchmarks for **AI/software memory systems**: persistent memory for LLM-based
agents and assistants. It is architecture-neutral, so it can assess RAG stores, vector databases,
knowledge graphs, structured memory layers, and other implementations by their observable
behavior.

It does **not** evaluate human cognition. Cognitive-science terminology is used only as a reference
point when naming and defining capabilities; it is not a claim that these benchmarks are human
psychometric tests.

## Current coverage

Four benchmarks have been verified against their primary sources and scored across all 16
capabilities:

- [BEAM](docs/benchmarks/beam.md)
- [LoCoMo](docs/benchmarks/locomo.md)
- [LongMemEval](docs/benchmarks/longmemeval.md)
- [MemoryAgentBench](docs/benchmarks/memoryagentbench.md)

The [comparison matrix](matrix.md) is the concise view. Each benchmark page contains the evidence
behind its row, including the task, metric, released-data examples, licenses, and limitations.

## How scoring works

Every benchmark–capability verdict is grounded in primary sources: the original paper and released
dataset and/or evaluation code. The process checks three proof legs in order:

1. **Authored targeting (Leg A):** Do the benchmark authors explicitly define a task or category
   for that capability’s core question? If not, the result is **❌ None**. Incidental examples are
   not enough.
2. **Concrete demonstration (Leg B):** Does released data contain a traceable example that truly
   requires the capability?
3. **Metric isolation (Leg C):** Does the metric score that capability specifically, rather than
   blend it into a broader measure?

| Verdict | Meaning |
|---|---|
| ✅ **Full** | Legs A, B, and C hold. |
| 🟡 **Partial** | Leg A holds, but Leg B or C falls short. |
| ❌ **None** | Leg A fails; the benchmark was not authored to test it. |
| ☐ **Pending** | Not yet evaluated. |

This standard deliberately favors an explicit pending result over an attractive but unsupported
claim. Benchmark and dataset licenses are recorded on their individual pages and remain separate
from this repository’s [MIT license](LICENSE).

## Repository layout

```text
.
├── README.md                  # Project overview and navigation
├── matrix.md                  # Benchmark × capability comparison
└── docs/
    ├── capabilities.md        # Taxonomy index and scoring rubric
    ├── capabilities/          # One benchmark-agnostic specification per capability
    └── benchmarks/            # One evidence-backed analysis per benchmark
```

`matrix.md` is at the root because it is the main comparison entry point. The `docs/` boundary is
intentional: it keeps the root concise and groups the two growing documentation collections without
mixing benchmark evidence into capability definitions.

## Contributing

To propose a benchmark, open a pull request that adds
`docs/benchmarks/<name>.md`, following the [LoCoMo page](docs/benchmarks/locomo.md) as the template.
Use official sources only: the paper, official code or dataset repository, and the project website.
Include the benchmark’s metadata, license, dataset statistics, task format, metric, and links to
the source material. Capability scoring is completed during review using the rubric above.
