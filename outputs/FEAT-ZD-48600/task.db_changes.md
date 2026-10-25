# task.db_changes: FEAT-ZD-48600

Database changes check for the Data Science lane (ap_forecast) of FEAT-ZD-48600 ("Show which overdue invoices belong to vendors that usually pay late"). This is DS-03 Database changes (attempt 1).

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DS-03 Database changes (attempt 1) | record:brief |
| Owner / gate | Developer / automated checks, independent judge, sampled human evaluation | record:brief |
| Repository | ap_forecast (Data Science lane) | feat.tasks (section 1, TSK-03, TSK-04) |
| Tasks in scope | TSK-03 (STORY-03, `risk.vendor_risk`), TSK-04 (STORY-04, `explain.build_explanation_request(..., risk=None)`) | feat.tasks (section 1) |
| Inputs | feat.tasks (b0fd4d2), feat.design (722a98f) | record:brief |

## 1. Result

**No database change is required for the Data Science lane, and none is outstanding. The outcome "Required database changes are completed" is met: the set of required changes is empty.**

## 2. Evidence

| # | Check | Finding | Source |
|---|---|---|---|
| E-1 | Does the design add storage? | The HLD boundary puts "Storage, database, schema migration" out of this design. The data changes are one new record key `paid_date`, one new reject reason `'paid_date'`, an in-memory delay history, risk map and overdue rows, and the CT-1 JSON | feat.design (A1) |
| E-2 | Does the design allow new technology or storage? | DD-1: "No new repository, service, runtime, library or storage." Only the Python 3.9 standard library and Node.js 22 CommonJS | feat.design (A7, DD-1) |
| E-3 | Are the new functions pure? | NF-10: every new function is pure and in-memory, with no I/O, no network and no mutation of inputs | feat.design (A5, NF-10) |
| E-4 | Would storage reopen the platform design? | "any storage or network call from the libraries" is a DES-01 re-entry trigger; the design triggers none of them | feat.design (A8) |
| E-5 | What does TSK-03 change? | New `src/ap_forecast/risk.py`, `__init__.py` (import, `__all__`, version step) and new `tests/test_risk.py`. Rule only: no LLM, no network, no `models.json` read | feat.tasks (TSK-03); feat.design (B2.1, B2.2) |
| E-6 | What does TSK-04 change? | `src/ap_forecast/explain.py` and `tests/test_forecast.py`; `models.json` must not change | feat.tasks (TSK-04); feat.design (B2.1) |
| E-7 | Does ap_forecast use a database today? | No. The repository view states ap_forecast has no database and no migrations exist or are required. A search of the checkout for database, SQL, SQLite, SQLAlchemy, psycopg and migration terms found no match | tool:repo-access; file:ap_forecast/src/ap_forecast/predict.py; file:ap_forecast/src/ap_forecast/explain.py |
| E-8 | What persistent data does ap_forecast read? | Only `models.json`, a JSON configuration file read by `explain.load_config()` (provider, model, `max_tokens`). It is configuration, not a database, and it is unchanged by this feature | file:ap_forecast/src/ap_forecast/explain.py; feat.design (B2.1) |
| E-9 | Is `predict_delay_days` stateful? | No. It computes the rounded mean of the given history in memory, with no I/O | file:ap_forecast/src/ap_forecast/predict.py |
| E-10 | Is the `.codegraph/` database part of the product? | No. It is a local code-intelligence index; its `.gitignore` ignores everything except itself, so it is never committed and is not an application database | file:ap_forecast/.codegraph/.gitignore |

## 3. Database change list

| Change | Type | Status |
|---|---|---|
| None | — | Not required (sections 1 and 2) |

No schema, table, index, migration script, seed data, grant or DBA ticket is needed for TSK-03 or TSK-04. Nothing waits on infra or DBAs for this lane.

## 4. Impact on the next steps

- DS-04 (components) and DS-05 (coding) can proceed without a database prerequisite.
- If a later change to the design introduces storage for delay history or risk levels, that is a DES-01 re-entry (feat.design A8), and DS-03 must be re-run.

## 5. Out of scope for this lane

The other lanes (ap_pipeline, ap_aging, ap_portal) are not checked here. The design states the same position for the whole feature (no storage, database or schema migration; feat.design A1, DD-1), but each lane confirms its own repository in its own step.

## 6. Open questions

None. No blocking question.


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_forecast] No database in this repository's architecture; no migrations required
