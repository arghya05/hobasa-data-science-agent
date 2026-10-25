# task.secrets_scope: FEAT-ZD-48600

Secrets check for the Data Science lane of FEAT-ZD-48600 ("Show which overdue invoices belong to vendors that usually pay late"). This is DS-02 Access to secrets. The workbook action is to check access to the secrets the tasks need and to ask for access where needed. This file lists secret names only. It holds no secret values.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DS-02 Access to secrets | record:brief |
| Owner / gate | Developer / no gate: automated checks, the independent judge and sampled human evaluation | record:brief |
| Inputs | feat.tasks (b0fd4d2); org.permission_table (restricted, passed by reference only; read through the tool view) | record:brief |
| Lane tasks | TSK-03 (STORY-03, `risk.vendor_risk`) and TSK-04 (STORY-04, `explain.build_explanation_request(..., risk=None)`), both in `ap_forecast` | feat.tasks (sections 1, 3) |

## 1. Result

**The Data Science tasks need no secrets, and none are granted. No access request is needed. The secrets scope is empty.**

| Check | Result | Source |
|---|---|---|
| Secrets granted for `ap_forecast` | None. The permission table grants no secrets for this repository | tool:repo-access |
| Secrets required by `ap_forecast` | None. The permission table requires no secrets for this repository | tool:repo-access |
| TSK-03 needs a secret | No. `vendor_risk` uses no LLM, makes no network call and does not read `models.json`. It calls only `predict_delay_days`, which computes from the history in memory | feat.tasks (TSK-03); journey:feat.clarifications (CL-DS4); file:ap_forecast/src/ap_forecast/predict.py |
| TSK-04 needs a secret | No. `build_explanation_request` builds and returns a request dict with `provider`, `model`, `max_tokens` and `messages`. It does not send the request, so it needs no provider credential. The provider and model come from `models.json` (`acme-llm`, `acme-text-2`) and stay unchanged | feat.tasks (TSK-04); journey:feat.clarifications (CL-DS8); file:ap_forecast/src/ap_forecast/explain.py; file:ap_forecast/src/ap_forecast/models.json |
| Secrets read by today's code | None. A search of the repository for secret, key, token, credential, password and environment-variable access matched only the `max_tokens` setting and the tests that check it | file:ap_forecast/src/ap_forecast/explain.py; file:ap_forecast/src/ap_forecast/models.json; file:ap_forecast/tests/test_forecast.py |
| CI command | `/usr/bin/python3 ci.py`, run in the repository | file:ap_forecast/README.md; file:ap_forecast/CLAUDE.md |
| Secrets for CI | None. `ci.py` compiles the sources, runs the unit tests with a coverage gate and runs static checks. It reads no secret, credential or environment variable | file:ap_forecast/ci.py |
| Secrets for the existing tests | None. The tests build requests from `models.json` or from an explicit config dict, with no credential | file:ap_forecast/tests/test_forecast.py |
| Test data | Synthetic only, with no secrets or personal data | journey:feat.clarifications (CL-X9) |
| Secrets added by the design | None added. Tests and examples use synthetic data | journey:feat.design (SE-4) |
| External data flow | With `risk`, the explanation prompt also carries the vendor's risk label to the configured provider (`acme-llm`). The Architect must confirm this (TQ-8). That changes what the prompt says, not which secrets the lane needs | journey:feat.design (SE-3); journey:feat.clarifications (CL-DS8); feat.tasks (TSK-04) |

## 2. Secrets scope (names only)

| Secret name | Environment | Needed by | Granted | Access request |
|---|---|---|---|---|
| — (none) | — | — | — | Not needed |

Source: tool:repo-access; feat.tasks (TSK-03, TSK-04).

## 3. Open questions (none blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| SSQ-1 | `explain.py` only builds the request for `acme-llm`. Whatever sends it would need the provider credential, and no input names that sender. Please confirm that credential is outside this lane's scope | Neither TSK-03 nor TSK-04 sends a request, and the design adds no secrets. Sending the risk label to `acme-llm` stays with the Architect (TQ-8) and gates release, not coding | Architect |
| SSQ-2 | The permission table is restricted. This step read it only through the tool view, which answers per repository, not per environment. Please confirm that no environment grants or requires a secret for `ap_forecast` | Use the tool view's statement as given: no secrets are required or granted for this repository | Developer / shared services |

## 4. Validation check

| Check | Result | Source |
|---|---|---|
| Every lane task checked for secret needs | Pass (TSK-03, TSK-04) | feat.tasks (section 1) |
| Required secrets available with access | Pass. None are required | tool:repo-access |
| Access requests raised | None needed | tool:repo-access |
| Secret values or personal data in this file | None | — |
| Blocking questions | None | — |


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_forecast] Tool registry: no secrets required or granted (permission table)
