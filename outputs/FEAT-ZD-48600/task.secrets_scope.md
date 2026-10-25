# task.secrets_scope: FEAT-ZD-48600 (Data Science lane, ap_forecast)

DS-02 Access to secrets (attempt 4). This file records which secrets the Data Science tasks need in each environment, and which ones are granted. It lists names only and holds no secret values. org.permission_table is restricted, so it is used only through the tool view (tool:repo-access) and is not copied here. The input feat.tasks comes from hobasa-design-agent at commit b0fd4d2 (record:brief).

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DS-02 Access to secrets (attempt 4) | record:brief |
| Lane / repository | Data Science / ap_forecast | feat.tasks (section 1) |
| Tasks in scope | TSK-03 (STORY-03), TSK-04 (STORY-04) | feat.tasks (section 1) |
| Permission source | Tool registry for ap_forecast: no secrets are required or granted for this repository | tool:repo-access |

## 1. Secrets needed, by task

| Task | What it does | External call or credential needed? | Secrets needed | Source |
|---|---|---|---|---|
| TSK-03 | New `vendor_risk(history)` in `risk.py`: a rule-based level (`'high'`, `'medium'`, `'low'`) from the existing `predict_delay_days` | No. The task states no LLM, no network and no `models.json` read, and `risk.py` imports only `predict_delay_days` | None | feat.tasks (TSK-03); tool:repo-access |
| TSK-04 | Adds `risk=None` to `build_explanation_request`, which appends `" Risk level: {risk}."` to the prompt | No new access. Provider, model and `max_tokens` keep coming from `config or load_config()`, and `models.json` does not change. Sending the label to `acme-llm` waits on Architect confirmation (TQ-8), which gates release, not coding | None | feat.tasks (TSK-04); tool:repo-access |

## 2. Secrets scope by environment (names only)

| Environment | Secrets required | Secrets granted | Access request raised | Source |
|---|---|---|---|---|
| Local / CI (task CI bar) | None | None | No | tool:repo-access; feat.tasks (section 2) |
| `feature/<story>` branch | None | None | No | tool:repo-access; feat.tasks (section 2) |
| `dev` (integration) | None | None | No | tool:repo-access; feat.tasks (section 2) |
| `main` (released) | None | None | No | tool:repo-access; feat.tasks (section 2) |

**Result:** the required access matches the granted access, and both are empty. No access request is needed. The outcome "Secrets are available with access permissions" is met trivially: the Data Science tasks need no secrets.

## 3. Boundaries and watch items

- Neither task adds an outbound call from ap_forecast. TSK-03 is explicitly no-network. TSK-04 changes only the prompt text, and nothing new calls it with `risk`; it is not in the end-to-end wiring (feat.tasks, TSK-03 and TSK-04).
- Whatever sends the explanation request to `acme-llm` needs a provider credential, and that is outside the scope of TSK-03 and TSK-04. If a later change makes ap_forecast call the provider directly, this step must be re-entered and a credential requested through the permission table. This is non-blocking question SSQ-1.
- The planned tests need no live provider: `test_vendor_risk_no_llm` patches `ap_forecast.explain.load_config` to fail, and the TSK-04 tests check the built prompt text (feat.tasks, TSK-03 and TSK-04 tests).
- No secret value, key name or personal data appears in this file.

## 4. Open questions (non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| SSQ-1 | Which component sends the `build_explanation_request` payload to `acme-llm`, and who owns its credential? | Outside the scope of TSK-03 and TSK-04. The permission table grants ap_forecast no secret, and none is needed | Architect / Shared services |


[Tool result] [ap_forecast] Tool registry: no secrets required or granted (permission table)