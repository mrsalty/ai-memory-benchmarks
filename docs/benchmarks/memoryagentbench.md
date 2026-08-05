# MemoryAgentBench

## Metadata

| Field | Value |
|---|---|
| Paper | ["Evaluating Memory in LLM Agents via Incremental Multi-Turn Interactions"](https://arxiv.org/abs/2507.05257) — ICLR 2026, Yuanzhe Hu, Yu Wang, Julian McAuley |
| Paper HTML | https://arxiv.org/html/2507.05257 |
| Code | https://github.com/HUST-AI-HYZ/MemoryAgentBench |
| Released data | [Hugging Face: `ai-hyz/MemoryAgentBench`](https://huggingface.co/datasets/ai-hyz/MemoryAgentBench) — `Accurate_Retrieval`, `Test_Time_Learning`, `Long_Range_Understanding`, and `Conflict_Resolution` Parquet splits |
| Eval code | [`main.py`](https://github.com/HUST-AI-HYZ/MemoryAgentBench/blob/main/main.py), [`conversation_creator.py`](https://github.com/HUST-AI-HYZ/MemoryAgentBench/blob/main/conversation_creator.py), and [`utils/eval_other_utils.py`](https://github.com/HUST-AI-HYZ/MemoryAgentBench/blob/main/utils/eval_other_utils.py) |
| License | **MIT** for the official code repository and Hugging Face dataset card; separate from this repository's MIT license. |
| Verification status | Verified against the ICLR 2026 paper's arXiv HTML/source (§3 and Appendix B), official GitHub README/configurations/harness, official dataset card and four released Parquet artifacts, and public dataset records including `Conflict_Resolution` row 0. |
| Last reviewed | 2026-08-03 |

## Description

MemoryAgentBench evaluates LLM memory agents through incremental multi-turn interaction. A context
is split into chunks, each chunk is sent with a memorization instruction, and questions are asked
after ingestion. The paper says agents take chunks “one by one,” absorb and incrementally update
memory, then answer related questions (§3.3). The released harness initializes an agent per
context, ingests that context's chunks, then runs associated questions; it does not impose an
independent-session or restart boundary.

The paper's competencies are **Accurate Retrieval (AR)**, **Test-Time Learning (TTL)**,
**Long-Range Understanding (LRU)**, and **Selective Forgetting (SF)**. SF is a taxonomy boundary
case: `FactConsolidation` contains explicitly contradictory fact-edit pairs and instructs agents
to use the later fact. It tests this repository's **Memory Updating**, not non-contradictory
forgetting/memory management.

## Dataset statistics and versions

| Split | Context records | Paper task groups | Average context lengths |
|---|---:|---|---|
| `Accurate_Retrieval` | 22 | SH/MH-Doc QA, LongMemEval (S*), EventQA | 197K, 421K, ~355K, 534K tokens |
| `Test_Time_Learning` | 6 | Five classifiers and movie recommendation | 103K; 1.44M recommendation |
| `Long_Range_Understanding` | 110 | ∞Bench summarization; Detective QA | 172K; 124K |
| `Conflict_Resolution` | 8 | FactConsolidation-SH/-MH | 6K, 32K, 64K, 262K |

Records have `context`, `questions`, `answers`, and `metadata`. Context counts are not question
counts: the paper states five LongMemEval (S*) histories contain 300 questions.

## Task format

| Authored competency | Task(s) | Requirement |
|---|---|---|
| Accurate Retrieval | SH/MH-Doc QA, LongMemEval (S*), EventQA | Locate one or more facts; EventQA selects a next event after up to five prior events. |
| Test-Time Learning | Five classifiers; movie recommendation | Learn label mappings from examples, or recommend movies from thousands of dialogue turns. |
| Long-Range Understanding | ∞Bench summarization; Detective QA | Summarize a novel or reason over a detective narrative. |
| “Selective Forgetting” | FactConsolidation-SH/-MH | Resolve explicit ordered contradictions using the newer/final fact. |

## Metric

| Task family | Released task identifiers | Metric |
|---|---|---|
| AR document/Event QA | `ruler_qa1`, `ruler_qa2`, `event_qa` | substring exact-match accuracy |
| FactConsolidation | `fact_mh`, `fact_sh` | substring exact-match accuracy |
| Detective QA and TTL classification | `detectiveQA`; `ICL_*` | exact-match accuracy |
| Recommendation | `recsys` | Recall@5 |
| LongMemEval (S*) | `longmemeval` | GPT-4o LLM-as-judge |
| ∞Bench summarization | `infbench` subset | LLM-as-judge F1 following HELMET |

Exact/sub-string match isolates answer, classification, and update tasks, but not what an agent
wrote to memory. Recall@5 does not penalize irrelevant items. No metric scores retrieval precision,
system-produced memory records, attribution, abstention, or restart survival.

## Sample data

Released `Conflict_Resolution` row 0 begins a numbered factual stream including `Thomas Kyd was
born in the city of London`, `Victoria Beckham is married to David Beckham`, and `the chief
executive officer of Apple Inc. is Tim Cook`; the complete record contains associated questions
and answers. It is a traceable FactConsolidation-format instance.

Appendix B says EventQA uses five ∞Bench books, extracts 101 character events, and turns each into
a six-way continuation question with five distractors. The agent receives up to five previous events
and selects the continuation by Accuracy.

## Capability scoring

Verdicts follow the Leg-A-gated rubric. Full requires authored targeting, a concrete data/task
demonstration, and a capability-isolating metric. None means no authored task targets the exact
taxonomy capability.

| # | Capability | Verdict | Primary-source evidence (Leg A → B → C) |
|---:|---|:---:|---|
| 1 | [Direct Retrieval](../capabilities/01-direct-retrieval.md) | ✅ Full | **A:** AR identifies/retrieves information in long dialogue; SH-Doc QA is single-hop. **B:** released AR records contain SH-Doc QA. **C:** SH-Doc/Event QA use substring exact-match Accuracy. |
| 2 | [Relational Integration](../capabilities/02-relational-integration.md) | ✅ Full | **A:** MH-Doc QA and FactConsolidation-MH require multiple facts/inference. **B:** the paper's MH example asks for a death location through a spouse relation. **C:** MH tasks have separate exact/sub-string-match Accuracy. |
| 3 | [Temporal Reasoning](../capabilities/03-temporal-reasoning.md) | ✅ Full | **A:** EventQA targets temporal sequences. **B:** it selects a continuation after up to five events. **C:** it reports mean multiple-choice Accuracy per book and across five books. |
| 4 | [Memory Updating](../capabilities/04-memory-updating.md) | ✅ Full | **A:** FactConsolidation orders contradictory old/new fact pairs and directs use of final state. **B:** released `Conflict_Resolution` row 0 was inspected. **C:** SH/MH use separate substring exact-match Accuracy. |
| 5 | [Directed Memory Deletion](../capabilities/05-directed-memory-deletion.md) | ❌ None | FactConsolidation is contradiction-driven later-fact replacement: Capability 4 here. No authored task instructs deletion of a specifically identified memory and scores later non-retrieval/non-use while unrelated controls remain available. |
| 6 | [Personalization](../capabilities/06-personalization.md) | ❌ None | Movie recommendation is TTL from dialogue examples, not a task where remembered user preference changes a separately scored response. Recall@5 has no personalized-preference isolation. |
| 7 | [Procedural Memory](../capabilities/07-procedural-memory.md) | ❌ None | TTL learns label mappings, not a previously specified workflow reused later. No procedure-reuse task or metric was found. |
| 8 | [Knowledge Abstraction](../capabilities/08-knowledge-abstraction.md) | ❌ None | No task authors or scores a generalized concept formed from repeated observations. Summarization is not a selective abstraction-of-memory test. |
| 9 | [Episodic vs Semantic Understanding](../capabilities/09-episodic-vs-semantic-understanding.md) | ❌ None | No competency tests or penalizes confusing a one-off episode with a standing trait. |
| 10 | [Memory Calibration](../capabilities/10-memory-calibration.md) | ❌ None | Tasks have gold answers/labels; no authored unanswerable/abstention task or metric was found. |
| 11 | [Retrieval Robustness](../capabilities/11-retrieval-robustness.md) | ❌ None | Accuracy, Recall@5, F1, and LLM judging do not score retrieval precision or penalize irrelevant retrieved material. |
| 12 | [Persistence](../capabilities/12-persistence.md) | 🟡 Partial | **A:** authors target incremental multi-turn memory. **B:** released records provide chunkable contexts and post-ingestion queries. **C fails:** no independent session/process or restart-survival test. |
| 13 | [Scalability](../capabilities/13-scalability.md) | ✅ Full | **A:** contexts span 6K–1.44M and the paper reports length analysis. **B:** released configs include FactConsolidation 6K/32K/64K/262K and EventQA variants. **C:** results are reported at multiple lengths. |
| 14 | [Associative Retrieval](../capabilities/14-associative-retrieval.md) | ❌ None | Tasks demand gold answers, learned mappings, or global interpretation; none scores merely conceptually associated memory absent an entailed answer. |
| 15 | [Memory Formation and Write Fidelity](../capabilities/15-memory-formation-and-write-fidelity.md) | ❌ None | Ingestion is instructed, but only downstream outputs are scored; the benchmark does not separately score what an agent wrote, omitted, distorted, or inferred. |
| 16 | [Memory Provenance and Source Attribution](../capabilities/16-memory-provenance-and-source-attribution.md) | ❌ None | Source/session/ID metadata is evaluator-visible, but systems need not output/select provenance and no attribution metric exists. |

## Open questions / follow-ups

- The public card/README say “Conflict Resolution,” while the ICLR paper says “Selective Forgetting”; this page scores task mechanics rather than labels.
- Public release counts are contexts while the paper also describes questions within shared contexts; report both when reproducing.
- A restart-preserving extension could upgrade Capability 12 from Partial to Full.
- FactConsolidation should not be cited as evidence of non-contradictory pruning, retention budgets, or harmful over-forgetting.
