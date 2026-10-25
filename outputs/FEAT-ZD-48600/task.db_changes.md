# task.db_changes: FEAT-ZD-48600 (Data Science lane, ap_forecast)

DS-03 Database changes (attempt 1). Workbook action: check the required database changes are completed.

**Result: the Data Science lane needs no database change, so nothing is outstanding. The step outcome ("Required database changes are completed") is met because there are no changes to make.**

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DS-03 Database changes (attempt 1) | record:brief |
| Lane / repository | Data Science / ap_forecast | feat.tasks (section 1, TSK-03, TSK-04) |
| Tasks in scope | TSK-03 (STORY-03, `risk.vendor_risk`), TSK-04 (STORY-04, `build_explanation_request(..., risk=None)`) | feat.tasks (sections 1, 3) |
| Inputs | feat.tasks (b0fd4d2), feat.design (722a98f) | record:brief |

## 1. What the design says about storage

| Check | Finding | Source |
|---|---|---|
| Data boundary of the feature | "Storage, database, schema migration" is listed as **out of this design**. The in-scope data is one new record key `paid_date`, one new reject reason `'paid_date'`, an in-memory delay history, risk map and overdue rows, and the CT-1 JSON | feat.design (A1, Data row) |
| Technology | No new repository, service, runtime, library or storage; only the Python 3.9 standard library and Node.js 22 CommonJS (DD-1) | feat.design (A7, DD-1); feat.tasks (intro) |
| Purity | Every new function is pure and in-memory: no I/O, no network, no mutation of inputs (NF-10) | feat.design (A5, NF-10) |
| Platform trigger | "any storage or network call from the libraries" would reopen DES-01; the design triggers none | feat.design (A8) |
| ap_forecast modules | New `src/ap_forecast/risk.py`; additive `risk=None` in `explain.py`; `__init__.py` export and version step; `predict.py` and `models.json` unchanged | feat.design (B2.1) |
| ap_forecast data structures | `history` (iterable of `int`), risk level (`str`), explanation request (`dict`), all held in memory | feat.design (B2.3) |
| TSK-03 constraints | No LLM, no network, no `models.json` read | feat.tasks (TSK-03 Goal) |
| TSK-04 constraints | Provider, model and `max_tokens` still come from `config or load_config()`; `models.json` must not change | feat.tasks (TSK-04 Must not change); feat.design (B2.2) |
| Package version today | ap_forecast `0.9.0`; the version step is owned by TSK-03. This is a package version, not a schema version | feat.design (B2 heading); feat.tasks (section 2) |

## 2. What the repository holds today

| Check | Finding | Source |
|---|---|---|
| Database or migrations | ap_forecast has no database; no migrations exist or are required | tool:repo-access |
| Tracked source files | `src/ap_forecast/__init__.py`, `explain.py`, `predict.py`, `models.json`; no migration folder, schema or SQL file | file:ap_forecast/src/ap_forecast (directory listing) |
| Database usage in code | A case-insensitive search of the repository for `sqlite`, `sqlalchemy`, `psycopg`, `migrat`, `database` and `CREATE TABLE` found no match | file:ap_forecast (search) |
| `predict_delay_days` | Pure computation over the given history (rounded mean, negatives count as 0); no storage | file:ap_forecast/src/ap_forecast/predict.py |
| `build_explanation_request` | Reads only the packaged `models.json` configuration file through `load_config()`; it does not touch a database | file:ap_forecast/src/ap_forecast/explain.py |

## 3. Required database changes

| # | Change | Status |
|---|---|---|
| — | None required by TSK-03 or TSK-04 | Complete (nothing to do) |

The Data Science lane needs nothing from DBAs or infrastructure. This step adds no migration, schema, seed data or connection secret (feat.design A1, DD-1; tool:repo-access).

## 4. Re-entry triggers

Re-run this check if any of the following appears. Each would bring storage into ap_forecast and also reopen DES-01 (feat.design A8):

- `vendor_risk` results or delay histories are to be persisted instead of computed in memory.
- The production wiring of the stages (open question DQ-1 in feat.design D3) introduces a store or service.
- A provider or model change (PQ-5, TQ-8) moves configuration out of `models.json` into a store.

## 5. Open questions

None new. The carried design questions (DQ-1 to DQ-5, TQ-1 to TQ-11) are unchanged, and none of them concerns a database for this lane.


[Tool result] [ap_forecast] No database in this repository's architecture; no migrations required