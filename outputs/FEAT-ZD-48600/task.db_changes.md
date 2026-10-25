# task.db_changes: FEAT-ZD-48600 (Data Science lane, ap_forecast)

DS-03 Database changes. This step checks whether the Data Science tasks need any database change and, if so, whether the change is complete.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DS-03 Database changes | record:brief |
| Owner / gate | Developer / automated checks, independent judge, sampled human evaluation | record:brief |
| Inputs | feat.tasks (b0fd4d2), feat.design (722a98f) | record:brief |
| Repository | ap_forecast (Python 3.9, standard library) | feat.tasks (TSK-03, TSK-04); file:ap_forecast/CLAUDE.md |
| Tasks in scope | TSK-03 (STORY-03, `risk.vendor_risk`), TSK-04 (STORY-04, `explain.build_explanation_request(..., risk=None)`) | feat.tasks (section 1) |

## 1. Result

**No database change is required for the Data Science tasks, and none is outstanding.** The step outcome ("Required database changes are completed") holds because there are no required changes to complete.

## 2. Evidence

| # | Check | Finding | Source |
|---|---|---|---|
| E-1 | Does the design add storage, a database or a schema migration? | No. The HLD boundary puts "Storage, database, schema migration" out of scope. The data in scope stays in memory (delay history, risk map, overdue rows) or travels as CT-1 JSON | feat.design (A1, "Data" row) |
| E-2 | Does the design add new technology or storage? | No. DD-1: "No new repository, service, runtime, library or storage" | feat.design (A7, DD-1) |
| E-3 | Are the new functions pure and in-memory? | Yes. NF-10: "Every new function is pure and in-memory: no I/O, no network, no mutation of inputs" | feat.design (A5, NF-10) |
| E-4 | What does TSK-03 touch? | New `src/ap_forecast/risk.py`, `src/ap_forecast/__init__.py`, new `tests/test_risk.py`. It makes no LLM call, makes no network call and does not read `models.json` | feat.tasks (TSK-03) |
| E-5 | What does TSK-04 touch? | `src/ap_forecast/explain.py` and `tests/test_forecast.py`. `models.json` must not change | feat.tasks (TSK-04) |
| E-6 | Would a storage change reopen the platform design? | Yes. "Any storage or network call from the libraries" re-triggers DES-01, and this design triggers none of the re-triggers | feat.design (A8) |
| E-7 | Does ap_forecast have a database or migrations today? | No database; no migrations exist or are required | tool:repo-access |
| E-8 | What do the package modules access today? | `predict.py` computes a rounded mean over an in-memory history using `statistics` and `typing` only. `explain.py` reads only the local `models.json` file next to the module and builds a request dict. `__init__.py` re-exports these two functions. None of them opens a database connection | file:ap_forecast/src/ap_forecast/predict.py; file:ap_forecast/src/ap_forecast/explain.py; file:ap_forecast/src/ap_forecast/__init__.py |
| E-9 | Is `models.json` a database? | No. It is the LLM provider configuration (`provider`, `model`, `max_tokens`) that `explain.load_config` reads. Neither task changes it | file:ap_forecast/src/ap_forecast/models.json; file:ap_forecast/src/ap_forecast/explain.py; feat.tasks (TSK-03, TSK-04) |

## 3. Database change list

| Change | Repository | Status |
|---|---|---|
| None required | ap_forecast | Not applicable |

## 4. Consequences for the next steps

- DS-04 (components) and DS-05 (coding) need no database prerequisite, migration script or DBA hand-off for TSK-03 and TSK-04 (feat.tasks section 3; feat.design A1, DD-1).
- If a later change adds storage to ap_forecast, that re-triggers DES-01 (feat.design A8), and this step runs again.

## 5. Open questions

None. No blocking question.


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_forecast] No database in this repository's architecture; no migrations required
