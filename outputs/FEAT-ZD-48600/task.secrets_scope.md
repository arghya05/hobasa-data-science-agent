# task.secrets_scope: FEAT-ZD-48600 (Data Science lane, ap_forecast)

DS-02 Access to secrets (attempt 1). This step checks which secrets the Data Science tasks need in each environment, compares them with the permission table and asks for access where needed. Only secret **names** belong in this file. No values appear here.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DS-02 Access to secrets (attempt 1) | record:brief |
| Owner / gate | Developer / automated checks, independent judge, sampled human evaluation | record:brief |
| Inputs | feat.tasks (commit b0fd4d2, produced by DES-04); org.permission_table (restricted, by reference through tool:repo-access) | record:brief; feat.tasks |
| Repository | ap_forecast, branch `feature/story` | feat.tasks (section 1, section 2); file:ap_forecast/CLAUDE.md |

## 1. Data Science tasks in scope

| Task | Story | What the code does | External call at run time? | Source |
|---|---|---|---|---|
| TSK-03 | STORY-03 | New `risk.vendor_risk(history)`: rule-based level from `predict_delay_days`. No LLM, no network, no `models.json` read | No | feat.tasks (TSK-03) |
| TSK-04 | STORY-04 | Adds `risk=None` to `explain.build_explanation_request`. The function only **builds** a request dict (provider, model, `max_tokens`, messages) from `config or load_config()` | No. The function sends nothing. Nothing new calls it with `risk`, and it is not part of the end-to-end wiring | feat.tasks (TSK-04); file:ap_forecast/src/ap_forecast/explain.py |

Both tasks use only the Python 3.9 standard library, with no new technology (feat.tasks, preamble, DD-1). CI is the local command `/usr/bin/python3 ci.py`: compile, unit tests, `coverage` and AST static checks. It needs no credentials (file:ap_forecast/ci.py; file:ap_forecast/CLAUDE.md).

## 2. Code check: secret references in ap_forecast

I searched the whole checkout, excluding `.git`, case-insensitively for `environ`, `getenv`, `api_key`, `API_KEY`, `token`, `secret` and `password`.

| Finding | Meaning | Source |
|---|---|---|
| `explain.py` line 15 and `models.json` line 4 match `max_tokens` | A model setting, not a credential | file:ap_forecast/src/ap_forecast/explain.py; file:ap_forecast/src/ap_forecast/models.json |
| `tests/test_forecast.py` lines 18–19 match `max_tokens` | The test passes an explicit config; no credential | file:ap_forecast/tests/test_forecast.py |
| No environment-variable reads, API keys, tokens or passwords | The code has no secret dependency today | file:ap_forecast/src/ap_forecast/explain.py; file:ap_forecast/ci.py |

`models.json` names provider `acme-llm` and model `acme-text-2`. It holds no key, and the repository has no code that calls the provider (file:ap_forecast/src/ap_forecast/models.json; file:ap_forecast/src/ap_forecast/explain.py).

## 3. Secrets scope (names only)

| Environment | Secrets required by TSK-03 / TSK-04 | Granted (permission table) | Gap / access request |
|---|---|---|---|
| Local development and unit tests | None | None | None |
| CI (`ci.py`) | None | None | None |
| dev (integration branch) | None | None | None |
| main (released) | None | None | None |

The permission table grants no secrets for ap_forecast and says none are required (tool:repo-access). This matches sections 1 and 2, so the required set and the granted set are both empty. **No access request is raised.** No excess access needs revoking.

## 4. Outcome

Secrets are available with the right permissions: the tasks need none, and none are granted. TSK-03 and TSK-04 can go on to DS-03 and DS-04 without any secrets prerequisite (tool:repo-access; feat.tasks).

## 5. Open questions (non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| SSQ-1 | If an `acme-llm` request is ever sent at run time, which runtime owns the provider credential and its permission-table entry? TSK-04 only builds the request. Sending the risk label to `acme-llm` waits on Architect confirmation (TQ-8), which gates release, not coding | Out of scope for this task: ap_forecast does not send the request, so it holds no credential | Architect / Shared services |

## 6. Validation check

| Check | Result | Source |
|---|---|---|
| Every DS task in feat.tasks considered (TSK-03, TSK-04) | Pass | feat.tasks (section 1) |
| Required secrets compared with the permission table | Pass: both sets are empty | tool:repo-access |
| No secret values or personal data in this file | Pass: no names to list | — |
| Blocking questions | None | — |


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_forecast] Tool registry: no secrets required or granted (permission table)
