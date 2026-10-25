# task.secrets_scope: FEAT-ZD-48600 (Data Science lane, ap_forecast)

This file is DS-02, Access to secrets. It records which secrets the Data Science tasks need in each environment and which secrets are granted for them. It gives names only and contains no secret values. org.permission_table is restricted, so this step uses it only through the tool view (tool:repo-access) and does not copy it here. The input feat.tasks comes from hobasa-design-agent at commit b0fd4d2 (record:brief).

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DS-02 Access to secrets | record:brief |
| Owner / gate | Developer / automated checks, independent judge, sampled human evaluation | record:brief |
| Lane / repository | Data Science / ap_forecast | feat.tasks (section 1) |
| Tasks in scope | TSK-03 (STORY-03), TSK-04 (STORY-04) | feat.tasks (section 1) |
| Permission source | Tool registry for ap_forecast: the permission table requires and grants no secrets for this repository | tool:repo-access |

## 1. Secrets needed, by task

| Task | What it does | External call or credential needed? | Secrets needed | Source |
|---|---|---|---|---|
| TSK-03 | Adds `vendor_risk(history)` in a new `risk.py`. It returns a rule-based level (`'high'`, `'medium'` or `'low'`) from the existing `predict_delay_days` | No. The task specifies no LLM, no network and no `models.json` read. `risk.py` imports only `predict_delay_days` | None | feat.tasks (TSK-03) |
| TSK-04 | Adds a `risk=None` parameter to `build_explanation_request`. When `risk` is given, the prompt ends with `" Risk level: {risk}."` | No new access. The provider, model and `max_tokens` still come from `config or load_config()`, and `models.json` stays unchanged. The function builds and returns a request dict and does not send it. Sending the label to `acme-llm` waits on Architect confirmation (TQ-8), which gates release, not coding | None | feat.tasks (TSK-04); file:ap_forecast/src/ap_forecast/explain.py |

## 2. Evidence from the repository

| File | What it does | Credential, environment variable or network use | Source |
|---|---|---|---|
| `predict.py` | Imports `statistics.mean` and `typing.Iterable` and returns the rounded mean of past delays | None | file:ap_forecast/src/ap_forecast/predict.py |
| `explain.py` | Imports `json` and `pathlib`, reads `models.json` from the package directory, and returns a dict with `provider`, `model`, `max_tokens` and `messages` | None. It sends no request | file:ap_forecast/src/ap_forecast/explain.py |
| `models.json` | Names the provider `acme-llm` and the model `acme-text-2` | None. It contains no key and no endpoint | file:ap_forecast/src/ap_forecast/models.json |
| `__init__.py` | Re-exports `predict_delay_days` and `build_explanation_request` | None | file:ap_forecast/src/ap_forecast/__init__.py |
| `tests/test_forecast.py` | Tests `predict_delay_days` and checks the built request, using the packaged config or an explicit config dict | None. No live provider is called | file:ap_forecast/tests/test_forecast.py |
| `ci.py` | Compiles the code, runs the unit tests with coverage, and runs static checks | None. It reads no credential | file:ap_forecast/ci.py |
| `CLAUDE.md` | States the language (Python 3.9, standard library), the CI command `/usr/bin/python3 ci.py` and the branches `main` (released), `dev` (integration) and `feature/<story>` | None | file:ap_forecast/CLAUDE.md |

## 3. Secrets scope by environment (names only)

| Environment | Secrets required | Secrets granted | Access request raised | Source |
|---|---|---|---|---|
| CI (`/usr/bin/python3 ci.py`) | None | None | No | tool:repo-access; file:ap_forecast/CLAUDE.md |
| `feature/<story>` | None | None | No | tool:repo-access; feat.tasks (section 2); file:ap_forecast/CLAUDE.md |
| `dev` (integration) | None | None | No | tool:repo-access; feat.tasks (section 2); file:ap_forecast/CLAUDE.md |
| `main` (released) | None | None | No | tool:repo-access; feat.tasks (section 2); file:ap_forecast/CLAUDE.md |

**Result:** the required secrets and the granted secrets match, and both lists are empty. No access request is needed. The outcome "Secrets are available with access permissions" holds because the Data Science tasks need no secrets (tool:repo-access; feat.tasks).

## 4. Boundaries and watch items

- Neither task adds an outbound call from ap_forecast. TSK-03 explicitly uses no network. Nothing new calls TSK-04's `build_explanation_request` with `risk`, and it is not part of the end-to-end wiring (feat.tasks, TSK-03 and TSK-04).
- Whatever component sends the explanation request to `acme-llm` will need a provider credential, and that component is outside TSK-03 and TSK-04. If a later change makes ap_forecast call the provider directly, this step must be re-entered and a credential requested through the permission table (non-blocking question SSQ-1).
- The planned tests need no live provider. `test_vendor_risk_no_llm` patches `ap_forecast.explain.load_config` to fail, and the TSK-04 tests check the text of the built prompt (feat.tasks, TSK-03 and TSK-04 "Tests to add").
- This file contains no secret value, credential name or personal data.

## 5. Open questions (non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| SSQ-1 | Which component sends the `build_explanation_request` payload to `acme-llm`, and who owns its credential? | Outside TSK-03 and TSK-04. The permission table grants ap_forecast no secrets, and this lane needs none | Architect / Shared services |


[Tool result] [ap_forecast] Tool registry: no secrets required or granted (permission table)