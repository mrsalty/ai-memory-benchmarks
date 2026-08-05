# BEAM (Beyond a Million Tokens)

## Metadata

| Field | Value |
|---|---|
| Paper | ["Beyond a Million Tokens: Benchmarking and Enhancing Long-Term Memory in LLMs"](https://arxiv.org/abs/2510.27246) — ICLR 2026, Mohammad Tavakoli, Alireza Salemi, Carrie Ye, Mohamed Abdalla, Hamed Zamani, J. Ross Mitchell |
| Project page | https://mohammadtavakoli78.github.io/beam-light/ |
| Code | https://github.com/mohammadtavakoli78/BEAM |
| Released data | [Hugging Face: `Mohammadta/BEAM`](https://huggingface.co/datasets/Mohammadta/BEAM) (100K, 500K, 1M) and [`Mohammadta/BEAM-10M`](https://huggingface.co/datasets/Mohammadta/BEAM-10M) |
| Eval code | [`run_evaluation.py`](https://github.com/mohammadtavakoli78/BEAM/blob/main/src/evaluation/run_evaluation.py), [`compute_metrics.py`](https://github.com/mohammadtavakoli78/BEAM/blob/main/src/evaluation/compute_metrics.py), [`report_results.py`](https://github.com/mohammadtavakoli78/BEAM/blob/main/src/evaluation/report_results.py) |
| License | **MIT** for code ([`LICENSE`](https://github.com/mohammadtavakoli78/BEAM/blob/main/LICENSE)); **CC BY-SA 4.0** for the benchmark/data, as stated by the official README and dataset cards. These are separate from this repository's MIT license. |
| Verification status | Verified against the [arXiv HTML paper](https://arxiv.org/html/2510.27246), official README, released repository data/evaluator files, and official Hugging Face dataset cards. Concrete questions below were read from [`chats/100K/1/probing_questions/probing_questions.json`](https://github.com/mohammadtavakoli78/BEAM/blob/main/chats/100K/1/probing_questions/probing_questions.json). |
| Last reviewed | 2026-08-03 |

## Citation

```bibtex
@misc{tavakoli2025milliontokensbenchmarkingenhancing,
      title={Beyond a Million Tokens: Benchmarking and Enhancing Long-Term Memory in LLMs},
      author={Mohammad Tavakoli and Alireza Salemi and Carrie Ye and Mohamed Abdalla and Hamed Zamani and J Ross Mitchell},
      year={2025}, eprint={2510.27246}, archivePrefix={arXiv}, primaryClass={cs.CL},
      url={https://arxiv.org/abs/2510.27246},
}
```

## Description

BEAM evaluates an LLM after a long, coherent multi-turn conversation using probing questions
explicitly designed for ten memory abilities. The authors describe its target as tasks requiring
“long-term memory and thus long-context reasoning,” and evaluate full-context LLMs, a conventional
RAG baseline, and their LIGHT method. LIGHT has an episodic-memory index, working memory, and a
scratchpad, so BEAM is usable for external-memory systems as well as raw long-context models.

This is also an important scope boundary: the runner can hand a complete compiled conversation to a
long-context model. BEAM measures answers to long-history probes; it does not require state to
survive an independently restarted application session.

## Dataset statistics and released versions

The paper reports **100 conversations** and **2,000 validated probing questions**:

| Nominal chat size | Conversations | Average user / assistant messages | Average turns |
|---|---:|---:|---:|
| 128K | 20 | 144 / 144 | 107 |
| 500K | 35 | 544 / 544 | 416 |
| 1M | 35 | 1,067 / 1,067 | 842 |
| 10M | 10 | 10,435 / 10,435 | 7,757 |

The standard Hugging Face release labels its smallest split `100K`, not `128K`, and has 20 `100K`,
35 `500K`, and 35 `1M` rows. The separate 10M release has 10 rows. Normal rows contain a seed,
narratives, profile, chronological plan, full `chat`, and serialized `probing_questions`; 10M rows
also contain ten plans.

## Task format

| Authored category | Requirement |
|---|---|
| Abstention | Withhold an answer when evidence is absent. |
| Contradiction Resolution | Detect and reconcile inconsistent separated statements. |
| Event Ordering | Reconstruct the order of evolving dialogue information. |
| Information Extraction | Recall entities and factual details in long histories. |
| Instruction Following | Sustain user-specified constraints over long contexts. |
| Knowledge Update | Revise stored facts when newer facts appear. |
| Multi-Session Reasoning | Integrate evidence across non-adjacent dialogue segments. |
| Preference Following | Adapt a response to evolving preferences. |
| Summarization | Abstract and compress dialogue content. |
| Temporal Reasoning | Reason about explicit and implicit time relations. |

“Multi-Session Reasoning” supports relational integration, but does not establish independent
application sessions: its released examples are separated portions of a supplied conversation.

## Metric

The evaluator reads each category separately from a system result JSON and its matching released
rubric. Nine categories use an LLM judge and average the rubric-item scores; `event_ordering` uses
normalized Kendall's tau (`tau_norm`). `report_results.py` exports one column per authored category,
not only a blended aggregate. This supplies category-level metric isolation, while leaving the
usual LLM-judge reproducibility caveat.

## Sample released data

The following are from [100K conversation 1](https://github.com/mohammadtavakoli78/BEAM/tree/main/chats/100K/1),
[`probing_questions/probing_questions.json`](https://github.com/mohammadtavakoli78/BEAM/blob/main/chats/100K/1/probing_questions/probing_questions.json),
reproduced under CC BY-SA 4.0.

| Category | Traceable released example |
|---|---|
| Information Extraction | “When does my first sprint end?”; rubric requires “March 29.” |
| Contradiction Resolution | “Have I worked with Flask routes and handled HTTP requests in this project?” References `chat_id: 58` and `chat_id: 24` and requires identifying the contradiction. |
| Knowledge Update | “What is the average response time of the dashboard API?”; rubric requires the later “250ms” value. |
| Multi-Session Reasoning | “How many new columns did I want to add … across my requests?”; rubric requires `category` and `notes`. |
| Preference Following | A request for Flask libraries; rubric requires lightweight choices and avoiding heavy frameworks. |
| Summarization | A project-progress summary requiring the timeline, features, security, and documentation. |
| Temporal Reasoning | A duration question connecting January 15 and March 15; the rubric requires “8 weeks.” |
| Abstention | A UI/UX-feedback question whose ideal response says the chat has no such information. |

## Capability scoring

| # | Capability | Verdict | Evidence |
|---:|---|---|---|
| 1 | [Direct Retrieval](../capabilities/01-direct-retrieval.md) | ✅ Full | **A:** Information Extraction explicitly measures recall of entities/facts. **B:** the first-sprint item requires “March 29.” **C:** `information_extraction` is separately evaluated and reported. |
| 2 | [Relational Integration](../capabilities/02-relational-integration.md) | ✅ Full | **A:** Multi-Session Reasoning explicitly integrates non-adjacent evidence. **B:** the columns item combines `category` and `notes` across requests. **C:** `multi_session_reasoning` is separately reported. |
| 3 | [Temporal Reasoning](../capabilities/03-temporal-reasoning.md) | ✅ Full | **A:** Temporal Reasoning explicitly targets time relations. **B:** the released duration item relates January 15 and March 15. **C:** `temporal_reasoning` is separately reported. |
| 4 | [Memory Updating](../capabilities/04-memory-updating.md) | ✅ Full | **A:** Knowledge Update explicitly revises facts as new facts appear; Contradiction Resolution is a second update category. **B:** the dashboard item requires the updated 250ms value. **C:** both categories have isolated scores. |
| 5 | [Directed Memory Deletion](../capabilities/05-directed-memory-deletion.md) | ❌ None | No authored task instructs deletion of a specifically identified memory and scores later non-retrieval/non-use while unrelated control memories remain available. |
| 6 | [Personalization](../capabilities/06-personalization.md) | ✅ Full | **A:** Preference Following explicitly targets personalized adaptation to evolving preferences. **B:** the released library item requires lightweight recommendations. **C:** `preference_following` is separately scored. |
| 7 | [Procedural Memory](../capabilities/07-procedural-memory.md) | ❌ None | Instruction Following concerns current supplied constraints, not later reuse of a previously learned procedure. No authored workflow-reuse task exists. |
| 8 | [Knowledge Abstraction](../capabilities/08-knowledge-abstraction.md) | ✅ Full | **A:** Summarization explicitly assesses abstraction/compression. **B:** the released project-progress item requires synthesis across features, schedule, security, and documentation. **C:** `summarization` is separately scored. |
| 9 | [Episodic vs Semantic Understanding](../capabilities/09-episodic-vs-semantic-understanding.md) | ❌ None | No task distinguishes a one-off episode from a standing trait or penalizes confusing them. |
| 10 | [Memory Calibration](../capabilities/10-memory-calibration.md) | ✅ Full | **A:** Abstention explicitly tests withholding an answer when evidence is missing. **B:** the released UI/UX-feedback item is unanswerable. **C:** `abstention` is separately scored. |
| 11 | [Retrieval Robustness](../capabilities/11-retrieval-robustness.md) | ❌ None | BEAM scores answer quality after full-context/RAG processing, not retrieval precision, irrelevant material, or distractor sensitivity. |
| 12 | [Persistence](../capabilities/12-persistence.md) | 🟡 Partial | **A:** authors explicitly target long-term memory. **B:** each released record is a long chronological history plus probes. **C fails:** the runner consumes compiled conversation context and imposes no restart/new-session survival test. |
| 13 | [Scalability](../capabilities/13-scalability.md) | ✅ Full | **A:** BEAM explicitly spans up to 10M tokens. **B:** releases cover 100K/500K/1M/10M. **C:** `chat_size` organizes results at each scale. |
| 14 | [Associative Retrieval](../capabilities/14-associative-retrieval.md) | ❌ None | No authored probe scores surfacing merely conceptually related memory without an entailed answer. |
| 15 | [Memory Formation and Write Fidelity](../capabilities/15-memory-formation-and-write-fidelity.md) | ❌ None | BEAM scores downstream answers, not what an evaluated system wrote, omitted, distorted, or inferred into memory. LIGHT's internal representations are a method, not a scored write-fidelity task. |
| 16 | [Memory Provenance and Source Attribution](../capabilities/16-memory-provenance-and-source-attribution.md) | ❌ None | Chat IDs and plan references are evaluator metadata; a system need not output/select source and no metric penalizes correct-content/wrong-source answers. |

## Open questions / follow-ups

- The paper calls the smallest setting 128K while the current standard dataset card calls it 100K; replications should identify the artifact/version used.
- Nine category scores use an LLM judge; reproductions should retain the judge model, prompt, and rubric.
- A chronological-ingestion plus restart-preservation harness could upgrade Capability 12 from Partial to Full.
- Summarization demonstrates answer-level abstraction, not inspection of an internal memory representation; it is not evidence for Capability 15.