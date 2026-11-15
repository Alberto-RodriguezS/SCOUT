# SCOUT notebooks — what each one is and what it backs

Every notebook runs end to end in Google Colab from this repository. Each run writes a run ID (`run_utc`, UTC timestamp) and the SHA-256 of the TRACE export it used. All the runs below used `data/MIT TRACE Incidents - Sep 28 2026.csv` (SHA-256 `a553b3f7…3871ad9`).

| Notebook | Stage | Run of record | What it backs |
|---|---|---|---|
| `Avance1.Equipo44.ipynb` | EDA (structure, univariate, bi/multivariate, preprocessing) | — | Article §2 (data) |
| `Avance2.Equipo44.ipynb` | Feature engineering and RAG prototype (hybrid retriever, extractive template, verifier) | — | Article §3 (method) |
| `Avance3_Equipo44.ipynb` | Baselines and evaluation: consequences (Part 1), retrieval (Part 2), scenarios (Part 3), label-free evaluation of generated answers (Part 4), operations (Part 5) | `20261019T004547Z` (18 Oct 2026, 18:45 Mexico City) | Article §4, **Table 5**, Table 9, Figure 12 |
| `Avance5_Equipo44_WebIndexExport.ipynb` | Same pipeline, plus §5.6: export of the web app's retrieval index | `20261101T164752Z` (1 Nov 2026) | **`docs/trace_index.json`, `docs/item_vectors.json`** and their copies in `docs/SCOUTuser/` and `docs/SCOUTdev/`; article §4.5 (0.778 cut-off) and §5.1 |

## `Avance3_Equipo44.ipynb` — evaluation of record

* **Generator:** Claude Haiku 5.5 (`claude-haiku-5-5`) with `max_tokens=8000`.
  * Cell 0.0 says `USER_LLM_BACKEND = "hf"`, but the cell before §3.3 switches to Claude, so all generations in Parts 3–5 are Claude.
  * Answers that stop on `max_tokens` or a refusal are rejected rather than scored.
* **Judge (§4.2):** Claude Sonnet 5.5 (`claude-sonnet-5-5`) with `max_tokens=4000`; 80 of 80 answers graded with 0 failures.
* **Faithfulness:** HHEM-2.1-Open, run on CPU in this run (about 200 min). A GPU makes it much faster.
* **Headline results (C0–C8):**

  | | |
  |---|---|
  | Faithfulness | 0.592 |
  | Citation support | 0.528 |
  | Judge groundedness | 0.787 |
  | Context relevance gain over random | +0.283 |
  | Citations follow content (Jaccard) | 0.990 |
  | LLM − template, faithfulness | −0.408 |
  | LLM − template, answer relevance | +0.113 |

  Conclusion: the web app keeps the deterministic template.
* **Needs:** an `ANTHROPIC_API_KEY` Colab secret. A rerun calls the models again, so its Part 4 figures will differ slightly from Table 5. The saved outputs are the record.

## `Avance5_Equipo44_WebIndexExport.ipynb` — source of the web app files

* §5.6 (a) builds the index:
  * 784 incidents as int8 `bge-small-en-v1.5` vectors of the "SCOUT document".
  * The 39 question vectors, built from `docs/question_bank.json`, which this notebook reads as input.
  * The strong-match cut-off: the 25th percentile of the second-best score over 500 random questionnaires, giving **0.778**. This calibrates coverage (about 3 in 4 profiles get a written explanation), not relevance.
  * The sector boost of 0.03.
* §5.6 (b) pushes the two files to `docs/` through the GitHub Contents API (secret `GITHUB_TOKEN`).
  * **`USER_PUSH_TO_GITHUB` is `False` by default** so a rerun does not overwrite the published files.
  * The pages read the copies in `docs/SCOUTuser/` and `docs/SCOUTdev/`. A new export must be copied there too.
* Parts 3–4 of this notebook ran with Qwen2.5-3B and a judge that failed on 38 of 80 answers. They are **not** the evaluation of record, and their interpretation text describes an earlier run.

## Earlier runs (history only)

| Run | Generator | Status |
|---|---|---|
| 15 Oct 2026 | Qwen2.5-3B, no judge | Faithfulness 0.952; cited in the article as not comparable |
| 18 Oct 2026, `20261018T214410Z` | Claude Haiku 5.5, `max_tokens=900` | Answers truncated; superseded. Previous `Avance3_Equipo44.ipynb`, recoverable from commit `5df8a58` |

## Known gaps (where to continue)

1. The web app's in-browser retrieval has not been evaluated for relevance offline. §5.6 contains a Python copy of the page's search (`web_search`) that can be scored on the Part 2 probes (nDCG@10, P@5).
2. The human reviews of §3.4 and §5.4 are pending.
3. The page's JSON request to the future RAG service declares a sector boost of 0.15, while retrieval applies 0.03.
