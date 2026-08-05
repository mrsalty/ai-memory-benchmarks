# LongMemEval (Long-Term Interactive Memory)

## Metadata

| Field | Value |
|---|---|
| Paper | ["LongMemEval: Benchmarking Chat Assistants on Long-Term Interactive Memory"](https://arxiv.org/abs/2410.10813) — ICLR 2025, Di Wu, Hongwei Wang, Wenhao Yu, Yuwei Zhang, Kai-Wei Chang, Dong Yu |
| Project page | https://xiaowu0162.github.io/long-mem-eval/ |
| Code | https://github.com/xiaowu0162/LongMemEval |
| Released data | [Hugging Face: `xiaowu0162/longmemeval-cleaned`](https://huggingface.co/datasets/xiaowu0162/longmemeval-cleaned) — [`longmemeval_oracle.json`](https://huggingface.co/datasets/xiaowu0162/longmemeval-cleaned/resolve/main/longmemeval_oracle.json), `longmemeval_s_cleaned.json`, and `longmemeval_m_cleaned.json` |
| Eval code | [`src/evaluation/evaluate_qa.py`](https://github.com/xiaowu0162/LongMemEval/blob/main/src/evaluation/evaluate_qa.py), [`src/retrieval/eval_utils.py`](https://github.com/xiaowu0162/LongMemEval/blob/main/src/retrieval/eval_utils.py), and [`src/retrieval/run_retrieval.py`](https://github.com/xiaowu0162/LongMemEval/blob/main/src/retrieval/run_retrieval.py) |
| License | **MIT** for the official code repository ([`LICENSE`](https://github.com/xiaowu0162/LongMemEval/blob/main/LICENSE)) and the current Hugging Face dataset card. These govern the released artifacts and are separate from this repository's own MIT license. |
| Verification status | Verified against the paper's [HTML version](https://arxiv.org/html/2410.10813v2), official project page, official GitHub README and evaluator/retrieval code, the current official Hugging Face dataset card, and the released `longmemeval_oracle.json` (all 500 records were counted locally; task fields and concrete records below were inspected). The paper's original histories and the currently released cleaned histories are not identical; claims about the released data below refer to the cleaned release. |
| Last reviewed | 2026-08-03 |

## Description

LongMemEval evaluates chat assistants answering questions after a long, timestamped collection of
user–assistant sessions. The authors describe five core abilities: **information extraction**,
**multi-session reasoning**, **temporal reasoning**, **knowledge updates**, and **abstention**.
Their project page defines these as recalling user or assistant details; synthesizing information
across sessions; recognizing changes in personal information; reasoning over explicit time and
timestamp metadata; and declining questions whose information was never mentioned.

The paper's construction starts from an ontology of 164 user attributes in five categories. Human
experts filter and rewrite LLM-proposed questions, decompose answers into evidence statements, and
embed each statement indirectly in a task-oriented self-chat session. Those sessions are manually
screened for evidence inclusion, distribution, and coherence, then combined with distractor
sessions into a timestamped history. This is a memory-system benchmark, not merely a raw
long-context stress test: the authors explicitly frame the work around indexing, retrieval, and
reading stages of a chat assistant's long-term memory.

## Dataset statistics and versions

The current official `longmemeval-cleaned` release contains 500 question records in each history
variant. The released oracle file has the following `question_type` distribution:

| Released question type | Count | Authored ability / task role |
|---|---:|---|
| `single-session-user` | 70 | information extraction from a user statement |
| `single-session-assistant` | 56 | information extraction from an assistant statement |
| `single-session-preference` | 30 | use a remembered user preference in a personalized response |
| `multi-session` | 133 | multi-session reasoning |
| `temporal-reasoning` | 133 | temporal reasoning |
| `knowledge-update` | 78 | knowledge updates |
| **Total** | **500** | |

Separately, 30 of these 500 records have a question ID containing `_abs` and are the abstention
items. This marker overlaps the six `question_type` rows above (rather than defining a seventh,
disjoint split); the official retrieval evaluation excludes these items because they have no answer
location.

The project page describes two standard history scales: **LongMemEval S**, about 115k tokens and
30–40 sessions per question, and **LongMemEval M**, about 500 sessions / 1.5M tokens. The current
data package names its history files `longmemeval_s_cleaned.json` and
`longmemeval_m_cleaned.json`; `longmemeval_oracle.json` is a compact evidence-only/oracle variant.

### Important release change

The official README and dataset card say the September 2025 cleaned release **replaces the
original LongMemEval dataset** by removing noisy history sessions that could interfere with answer
correctness. Consequently, reported paper results and any reproduction on the cleaned files are
not automatically the same experimental artifact. This page uses the maintained cleaned release
for its record-level evidence and does not silently project its history contents back onto the
paper version.

## Task format

Each record contains `question_id`, `question_type`, `question`, `answer`, `question_date`,
`haystack_sessions`, `haystack_session_ids`, `haystack_dates`, and `answer_session_ids`. The final
field points to the session(s) containing the annotated supporting evidence. The evaluated system
returns a hypothesis for the question; it is not asked to return evidence IDs or citations.

The official retrieval runner loads one complete record at a time and builds its retrieval corpus
from that record's `haystack_sessions`. It supports a turn- or session-granularity index and
optional offline key expansions (summaries, keyphrases, user facts, or timestamped events). Thus,
although the benchmark's *intended deployment framing* is online memorization across interactions,
the supplied retrieval evaluation can operate over the already-compiled full history. It does not
require an application restart or inspect memory state written by the evaluated system.

## Metric

### Question answering

`evaluate_qa.py` uses a model judge (supported configurations include GPT-4o, GPT-4o mini, or a
locally served Llama 3.1 70B) at temperature 0. It reports overall accuracy and a separate
accuracy for every `question_type`. The judging instructions are task-specific:

- ordinary information and multi-session items accept semantically equivalent full answers;
- temporal items explicitly forgive off-by-one day errors for duration answers;
- update items accept previous information alongside the required updated answer;
- preference items require a response that recalls **and uses** personal information correctly;
- abstention items are judged on recognizing that the question is unanswerable.

This category-level reporting isolates Direct Retrieval, Relational Integration, Temporal
Reasoning, Memory Updating, Personalization, and Memory Calibration results. It does **not** score
the source of an answer or a system-produced memory representation.

### Retrieval

For annotated answer sessions/turns, the released retrieval code reports `recall_any`,
`recall_all`, and nDCG at configured cutoffs (the reporting script prints session-level @5/@10 and
turn-level @5/@10/@50 variants). `recall_all` is useful for multi-session evidence because every
annotated target must be retrieved. But neither this code nor the QA metric penalizes irrelevant
items retrieved alongside the targets; it is not a retrieval-precision/noise metric. Retrieval
evaluation skips the 30 abstention records and also skips records with no labeled user-side target.

## Sample released records

The following are compact, traceable facts from the official cleaned
[`longmemeval_oracle.json`](https://huggingface.co/datasets/xiaowu0162/longmemeval-cleaned/resolve/main/longmemeval_oracle.json).
They reproduce question metadata rather than conversation text.

| Question ID | Type | Question → gold answer | Evidence session IDs | Why it matters here |
|---|---|---|---|---|
| `e47becba` | `single-session-user` | “What degree did I graduate with?” → “Business Administration” | `answer_280352e9` | one user-information session supports direct retrieval |
| `0a995998` | `multi-session` | “How many items of clothing do I need to pick up or return from a store?” → `3` | `answer_afa9873b_2`, `_3`, `_1` | three distinct annotated sessions support multi-session synthesis |
| `gpt4_2655b836` | `temporal-reasoning` | “What was the first issue I had with my new car after its first service?” → “GPS system not functioning correctly” | `answer_4be1b6b4_2`, `_3`, `_1` | asks for temporal ordering across timestamped evidence |
| `6a1eabeb` | `knowledge-update` | “What was my personal best time in the charity 5K run?” → “25 minutes and 50 seconds (or 25:50)” | `answer_a25d4a91_1`, `_2` | paired evidence sessions, dated 2023-05-25 and 2023-05-27, support an updated fact |
| `8a2466db` | `single-session-preference` | request for video-editing resources → tailor suggestions to Adobe Premiere Pro and its advanced settings | `answer_edb03329` | a non-recall recommendation is judged against a personalized-response rubric |

## Capability scoring

Verdicts follow the repository's Leg-A-gated rubric. “Full” means an author-defined task targets
the exact capability, the released data supplies a traceable example, and the official evaluator
reports a category-specific score. “None” means no such authored targeting was found; incidental
metadata, a broad long-memory framing, or an implementation option is not enough.

| # | Capability | Verdict | Evidence and boundary |
|---:|---|---|---|
| 1 | [Direct Retrieval](../capabilities/01-direct-retrieval.md) | ✅ Full | **A:** the authors define Information Extraction as recalling specific user/assistant information from extensive histories. **B:** `e47becba` is a single-session user fact with one evidence session. **C:** QA accuracy is reported separately for `single-session-user` and `single-session-assistant`. |
| 2 | [Relational Integration](../capabilities/02-relational-integration.md) | ✅ Full | **A:** the project page defines Multi-Session Reasoning as synthesizing information across sessions for aggregation/comparison questions. **B:** `0a995998` has three required evidence sessions and asks for their combined count. **C:** `multi-session` has an isolated QA accuracy. |
| 3 | [Temporal Reasoning](../capabilities/03-temporal-reasoning.md) | ✅ Full | **A:** the authors explicitly target explicit time mentions and timestamp metadata. **B:** `gpt4_2655b836` asks for the *first* post-service issue across three dated sessions. **C:** `temporal-reasoning` is scored separately, with a temporal-specific judge prompt. |
| 4 | [Memory Updating](../capabilities/04-memory-updating.md) | ✅ Full | **A:** Knowledge Updates explicitly tests recognizing changed personal information and dynamically updating knowledge. **B:** `6a1eabeb` has two dated evidence sessions for the 5K personal-best update. **C:** `knowledge-update` is a separate QA category and its judge explicitly requires the updated answer. |
| 5 | [Directed Memory Deletion](../capabilities/05-directed-memory-deletion.md) | ❌ None | The benchmark makes histories difficult with distractors and tests updates, but no author-defined task instructs deletion of a specifically identified memory and then scores non-retrieval/non-use alongside retained controls. Update correctness is not directed deletion. |
| 6 | [Personalization](../capabilities/06-personalization.md) | ✅ Full | **A:** the released `single-session-preference` evaluator explicitly asks whether a response “recalls and utilizes” personal information correctly. **B:** `8a2466db` requires a recommendation shaped by the user's Premiere Pro preference, not merely restating it. **C:** this is its own question type and QA score. |
| 7 | [Procedural Memory](../capabilities/07-procedural-memory.md) | ❌ None | No authored task requires reusing a remembered workflow, convention, or operating procedure in a later task. Assistant-information recall does not establish procedural reuse. |
| 8 | [Knowledge Abstraction](../capabilities/08-knowledge-abstraction.md) | ❌ None | Multi-session synthesis combines annotated evidence for an answer; the sources do not define or score forming a durable higher-level generalization from repeated observations. |
| 9 | [Episodic vs Semantic Understanding](../capabilities/09-episodic-vs-semantic-understanding.md) | ❌ None | The benchmark includes events and personal information, but no authored category tests over-/under-generalizing a one-off event into a standing trait. |
| 10 | [Memory Calibration](../capabilities/10-memory-calibration.md) | ✅ Full | **A:** Abstention is defined as refraining when information was not mentioned. **B:** 30 `_abs` records are deliberately unanswerable and have no answer location. **C:** the QA evaluator uses a dedicated abstention prompt and reports that question type separately. |
| 11 | [Retrieval Robustness](../capabilities/11-retrieval-robustness.md) | ❌ None | Although the histories contain distractors, the retrieval metrics are Recall-any/Recall-all/nDCG and do not penalize irrelevant retrieved material; no authored noise-precision task or metric was found. |
| 12 | [Persistence](../capabilities/12-persistence.md) | 🟡 Partial | **A:** the benchmark explicitly frames questions after sustained, multi-session interactions. **B:** multi-session records such as `0a995998` cross independent chat sessions. **C falls short:** the supplied runner consumes each compiled full history as a corpus; it neither imposes a restart/new-context boundary nor checks that system state persisted across one. This tests cross-session *reasoning over supplied history*, not persistence in the taxonomy's stronger engineering sense. |
| 13 | [Scalability](../capabilities/13-scalability.md) | ✅ Full | **A:** the authors expressly design scalable histories and publish S (~115k tokens) and M (~1.5M tokens) settings. **B:** both released cleaned history variants retain the same question IDs, including the examples above. **C:** the paper/project page reports performance at these distinct history scales, enabling a degradation comparison rather than only one fixed-size aggregate. |
| 14 | [Associative Retrieval](../capabilities/14-associative-retrieval.md) | ❌ None | The benchmark targets locating annotated evidence for a question, not surfacing conceptually related memories where no explicit relation or uniquely entailed answer is given. |
| 15 | [Memory Formation and Write Fidelity](../capabilities/15-memory-formation-and-write-fidelity.md) | ❌ None | The authors discuss online memorization and provide optional summary/fact index expansions, but the benchmark scores neither which memories the evaluated system writes nor their factual fidelity separately from downstream retrieval/QA. Supplied sessions and oracle evidence are benchmark artifacts, not system-produced memories under evaluation. |
| 16 | [Memory Provenance and Source Attribution](../capabilities/16-memory-provenance-and-source-attribution.md) | ❌ None | `answer_session_ids` and timestamps are evaluator metadata. The system returns only a hypothesis; QA correctness does not require selecting or reporting the supporting speaker, session, turn, or inference source. |

## Open questions / follow-ups

- The maintained cleaned data card says it removed noisy sessions, but the linked change log has not
  been independently audited record by record. Any comparison with paper tables should identify
  which dataset version it uses.
- The official README says systems should parse interactions online, while the supplied retrieval
  runner loads a complete compiled history per record. A future evaluation harness could enforce
  chronological ingestion and an actual session/restart boundary to upgrade Capability 12 from
  Partial to a directly measured behavior.
- QA uses an LLM judge. Its fixed prompts and per-type reporting make task categories visible, but
  the benchmark does not publish a deterministic exact-match alternative for all free-form and
  preference answers in the checked evaluator.
- Retrieval nDCG discounts rank and Recall-all checks target completeness, but neither penalizes
  retrieval pollution. A precision or distractor-sensitivity metric would be needed before this
  benchmark could support Capability 11.