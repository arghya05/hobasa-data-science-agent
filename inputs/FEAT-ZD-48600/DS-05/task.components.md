<!-- input task.components for DS-05: from hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.components.md at commit f956a35, produced by DS-04 -->
# task.components: FEAT-ZD-48600 (Data Science lane, ap_forecast)

DS-04 Identify the components/layers to be changed (attempt 1). Workbook action: read the existing code, understand it, and list the components and layers to create or update with the minimum lines of code. Also confirm access to secrets in all environments.

**Result: the lane changes one repository, ap_forecast. It needs one new source module (`risk.py`), additive edits to two existing modules (`explain.py`, `__init__.py`), one new test file (`tests/test_risk.py`) and appended tests in `tests/test_forecast.py`. `predict.py`, `models.json` and `ci.py` stay unchanged. No database, Databricks or secret is needed in any environment (section 5).**

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DS-04 Identify the components/layers to be changed (attempt 1) | record:brief |
| Lane / repository | Data Science / ap_forecast | feat.tasks (section 1); task.repo_access (section 1) |
| Tasks in scope | TSK-03 (STORY-03), TSK-04 (STORY-04) | feat.tasks (sections 1, 3) |
| Inputs | feat.tasks (b0fd4d2), feat.stories (0590ea4), feat.prd (908b58d), feat.trd (3637e51), feat.design (722a98f), task.repo_access (aac590a) | record:brief |
| Background read | task.secrets_scope (DS-02, e779ba1), task.db_changes (DS-03, 27457ab) | journey:task.secrets_scope; journey:task.db_changes; mcp:hobasa-context (context_list revisions) |
| Code read | `src/ap_forecast/__init__.py`, `explain.py`, `predict.py`, `models.json`, `tests/test_forecast.py`; `ci.py` | tool:repo; file:ap_forecast/ci.py |
| Working branch | `feature/story`, created from `dev` at `d67e866`, clean | task.repo_access (section 2) |

## 1. What the code does today

| Layer | Module | Today | Source |
|---|---|---|---|
| Package API | `src/ap_forecast/__init__.py` | Imports `predict_delay_days` and `build_explanation_request`; `__version__ = "0.9.0"`; `__all__` lists those two names | tool:repo |
| Prediction (rule / model) | `src/ap_forecast/predict.py` | `predict_delay_days(history)`: rounded mean of past delays; 0 for an empty history; negative delays (early payments) count as 0 | tool:repo |
| LLM request | `src/ap_forecast/explain.py` | `build_explanation_request(vendor, predicted_days, config=None)` builds a one-sentence prompt and returns `provider`, `model`, `max_tokens` (from `config or load_config()`) and `messages`. It builds the dict; it does not send it | tool:repo |
| LLM configuration | `src/ap_forecast/models.json` | `provider` `acme-llm`, `model` `acme-text-2`, `max_tokens` 300; no key or endpoint | tool:repo |
| Tests | `tests/test_forecast.py` | `test_predict`, `test_request_uses_config`, `test_request_with_explicit_config` | tool:repo |
| CI | `ci.py`, run as `/usr/bin/python3 ci.py` | Compile, unittest discovery under `tests/`, coverage on `src/ap_forecast` with `COVERAGE_MIN = 90.0`, static checks (no `print` / `eval` / `exec`, no bare `except`, lines at most 140 characters) over every `src/ap_forecast/*.py` | file:ap_forecast/ci.py; task.repo_access (section 3) |

The package has no network, pipeline or storage layer of its own (tool:repo; journey:task.db_changes, section 2). Because `ci.py` discovers every test file under `tests/` and measures coverage over every module in `src/ap_forecast`, a new `risk.py` and a new `tests/test_risk.py` are picked up without any CI change (file:ap_forecast/ci.py).

## 2. Components and layers to create or update

| # | Layer | Component | Action | Task / story | What changes (minimum) | Source |
|---|---|---|---|---|---|---|
| CMP-1 | Prediction (rule) | `src/ap_forecast/risk.py`: `HIGH_ABOVE_DAYS = 30`, `MEDIUM_ABOVE_DAYS = 10`, `vendor_risk(history: Iterable[int]) -> str` | **Create** | TSK-03 / STORY-03 | New module. Imports only `predict_delay_days` (not `explain`). Calls it once and compares with strict `>`: `'high'` above 30, `'medium'` above 10, else `'low'` | feat.design (HC-4, B2.1, B2.2, DD-9); feat.trd (IF-3.1 to IF-3.3); feat.tasks (TSK-03); feat.prd (AC-7 to AC-10) |
| CMP-2 | Package API | `src/ap_forecast/__init__.py` | **Update** | TSK-03 / STORY-03 | Add `from .risk import vendor_risk`, add `"vendor_risk"` to `__all__`, and take the package's minor version step from `0.9.0` (TSK-03 owns it) | feat.design (B2.1, DD-11); feat.tasks (section 2, TSK-03); feat.stories (TK-03.1, TK-03.4, AC-03.8) |
| CMP-3 | LLM request | `src/ap_forecast/explain.py`: `build_explanation_request(vendor, predicted_days, config=None, risk=None)` | **Update (additive)** | TSK-04 / STORY-04 | Add `risk=None` as the last parameter. Append `f" Risk level: {risk}."` to the prompt only when `risk is not None`. The returned dict and `load_config` stay the same | feat.design (HC-5, B2.2); feat.trd (IF-4.1 to IF-4.5); feat.tasks (TSK-04); feat.prd (AC-11, AC-12) |
| CMP-4 | Tests | `tests/test_risk.py` | **Create** | TSK-03 / STORY-03 | `test_vendor_risk_levels`, `test_vendor_risk_boundaries`, `test_vendor_risk_empty_and_early`, `test_vendor_risk_no_llm`, `test_vendor_risk_exported` | feat.tasks (section 2 test files, TSK-03); feat.design (B2.5) |
| CMP-5 | Tests | `tests/test_forecast.py` | **Update (append)** | TSK-04 / STORY-04 | Add `test_request_with_risk` and `test_request_without_risk_unchanged`. Keep the existing three tests exactly as they are | feat.tasks (TSK-04); feat.design (B2.5); feat.stories (TK-04.2) |

### Left unchanged

| Component | Why | Source |
|---|---|---|
| `src/ap_forecast/predict.py` | `vendor_risk` reuses `predict_delay_days` as it is | feat.design (B2.1); feat.tasks (TSK-03 "Must not change") |
| `src/ap_forecast/models.json` | Provider, model and `max_tokens` are unchanged; `vendor_risk` must not read it | feat.trd (IF-3.2, IF-4.4); feat.tasks (TSK-03, TSK-04 "Must not change") |
| `load_config` and the `config or load_config()` source in `explain.py` | Provider, model and `max_tokens` still come from `config` or `models.json` | feat.stories (AC-04.4) |
| Existing signatures | "Keep every existing signature; add new functions." `risk` goes last so positional `config` calls still work | feat.design (Architecture rule row, NF-4); feat.prd (NFR-4); feat.stories (AC-04.3) |
| `ci.py`, `tests/__init__.py`, `README.md`, `CLAUDE.md` | Nothing in TSK-03 or TSK-04 needs them changed; CI already discovers new tests and modules | file:ap_forecast/ci.py; feat.tasks (TSK-03, TSK-04 "Files in budget") |
| Other repositories (ap_pipeline, ap_aging, ap_portal) | Other lanes' tasks. No lane imports another lane's package | feat.design (Part C "Lane dependencies"); task.repo_access (section 1) |

## 3. Minimum-code implementation sketch

The sketches follow the signatures and rules in feat.design B2.2. DS-05 writes the final code.

**CMP-1, new `risk.py`.** A single comparison chain with no validation of its own: `predict_delay_days` already applies `int(d)`, and calling it once lets a generator work (feat.tasks TSK-03 notes).

```python
"""Rule-based vendor payment-risk level from the predicted delay. No LLM, no models.json read."""
from typing import Iterable

from .predict import predict_delay_days

HIGH_ABOVE_DAYS = 30
MEDIUM_ABOVE_DAYS = 10


def vendor_risk(history: Iterable[int]) -> str:
    """'high' if the predicted delay is above HIGH_ABOVE_DAYS, 'medium' if above MEDIUM_ABOVE_DAYS, else 'low'."""
    days = predict_delay_days(history)
    if days > HIGH_ABOVE_DAYS:
        return "high"
    return "medium" if days > MEDIUM_ABOVE_DAYS else "low"
```

**CMP-2, `__init__.py`.** One import line, one name added to `__all__`, and the `__version__` value changed.

**CMP-3, `explain.py`.** Add the parameter and one guarded append. Everything else in the function stays the same.

```python
def build_explanation_request(vendor: str, predicted_days: int, config=None, risk=None) -> dict:
    cfg = config or load_config()
    prompt = f"Explain in one sentence why vendor {vendor} is predicted to be paid {predicted_days} days late."
    if risk is not None:
        prompt += f" Risk level: {risk}."
    return {...}  # unchanged
```

### Why this is the minimum

| Choice | Effect on code size | Source |
|---|---|---|
| Reuse `predict_delay_days` instead of a second averaging rule | No change to `predict.py` | feat.design (B2.1); feat.trd (IF-3.1) |
| No input validation in `vendor_risk`; call `predict_delay_days` exactly once | No extra branches to test | feat.tasks (TSK-03 notes) |
| `risk` appended without validation (any non-`None` value) | One `if` in `explain.py` | feat.tasks (TSK-04 notes); feat.trd (IF-4.5) |
| `risk` as the last parameter with default `None` | No caller or existing test changes | feat.trd (IF-4.1); feat.stories (AC-04.3) |
| Export only `vendor_risk` in `__all__`; the threshold constants stay module-level in `risk.py` (agent judgement) | No extra exports to maintain | feat.design (B2.1); feat.stories (TK-03.1, AC-03.8) |
| No CI or config change | `ci.py` already covers new modules and test files | file:ap_forecast/ci.py |

## 4. Tests and coverage

| Test | File | Asserts | AC | Source |
|---|---|---|---|---|
| `test_vendor_risk_levels` | `tests/test_risk.py` | Histories predicting 31 → `'high'`, 20 → `'medium'`, 5 → `'low'` | AC-03.1, AC-03.3 | feat.design (B2.5) |
| `test_vendor_risk_boundaries` | `tests/test_risk.py` | Single-value histories `[30]` → `'medium'`, `[31]` → `'high'`, `[10]` → `'low'`, `[11]` → `'medium'` | AC-03.2, AC-03.4 | feat.tasks (TSK-03); record:memory (L-007) |
| `test_vendor_risk_empty_and_early` | `tests/test_risk.py` | `[]` → `'low'`; only negative delays → `'low'` | AC-03.5, AC-03.6 | feat.design (B2.5) |
| `test_vendor_risk_no_llm` | `tests/test_risk.py` | Patch `ap_forecast.explain.load_config` to fail; `vendor_risk` still returns | AC-03.7 | feat.tasks (TSK-03) |
| `test_vendor_risk_exported` | `tests/test_risk.py` | `"vendor_risk" in ap_forecast.__all__` | AC-03.8 | feat.design (B2.5) |
| `test_request_with_risk` | `tests/test_forecast.py` | Prompt ends with `' Risk level: high.'` | AC-04.1 | feat.tasks (TSK-04) |
| `test_request_without_risk_unchanged` | `tests/test_forecast.py` | Same result with no `risk` and with explicit `risk=None` | AC-04.2, AC-04.5 | feat.tasks (TSK-04) |
| Existing three tests | `tests/test_forecast.py` | Pass unchanged (positional `config` still works) | AC-04.3 | feat.design (B2.5) |

Agent judgement from the sketches in section 3: every new line is reached by these tests, because each branch of `vendor_risk` is hit by the levels and boundaries tests and both sides of the `risk is not None` guard are hit by the two request tests, so the coverage bar of at least 90% (feat.tasks section 2 CI bar; file:ap_forecast/ci.py) is expected to hold. CI has not run in this lane yet, and whether the `coverage` package is installed on the runner is still open (task.repo_access RAQ-1); the first CI run in DS-05/DS-06 confirms both.

## 5. Secrets, database and environments

| Item | Finding | Source |
|---|---|---|
| Secrets added by this change | None. "No secrets, credentials or personal data are added" | feat.trd (S-6); feat.design (SE-4) |
| Credentials in ap_forecast code or config | None. `models.json` holds only `provider`, `model` and `max_tokens` | tool:repo |
| Outbound calls added | None. `vendor_risk` makes no LLM or network call; `build_explanation_request` only builds the request dict, with provider and model unchanged | feat.trd (IF-3.2, IF-4.4); tool:repo |
| Secrets per environment (CI, `feature/<story>`, `dev`, `main`) | None required and none granted in any environment; no access request needed | journey:task.secrets_scope (section 3) |
| Database / Databricks, all environments | None needed; nothing to provision | journey:task.db_changes (section 3); task.repo_access (section 2) |

The tests need no live provider: they check the built prompt text and patch `load_config` (feat.tasks TSK-03, TSK-04). Sending the risk label to `acme-llm` waits on Architect confirmation (TQ-8), which gates release, not coding (feat.tasks TSK-04; feat.design SE-3). Whichever component eventually sends the request will need a provider credential, outside this lane (journey:task.secrets_scope SSQ-1).

## 6. Dependencies and order

| Item | Effect on this lane | Source |
|---|---|---|
| Other lanes | No code dependency. TSK-03 and TSK-04 can be built in parallel with DE, BE and FE | feat.design (Part C); feat.tasks (section 1) |
| TSK-03 ↔ TSK-04 | Independent. They touch different files apart from the shared version step, which TSK-03 owns | feat.tasks (section 2) |
| PROV-ACME-2026-11 (PQ-5) | Also changes `build_explanation_request`. Whichever merges second rebases and re-runs T-7 | feat.tasks (TSK-04); feat.trd (R-3); feat.prd (PQ-5) |
| End-to-end harness (TSK-08) | Uses `vendor_risk` (CMP-1); does not call `build_explanation_request` | feat.tasks (TSK-08); feat.stories (SA-5) |

## 7. Open questions (non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| CQ-DS-1 | feat.stories TK-03.2 suggests adding the risk tests to `tests/test_forecast.py`, while feat.tasks (section 2 "Test files"; TA-4 in section 6) assigns a new `tests/test_risk.py` | Follow feat.tasks, which is the later task breakdown: new `tests/test_risk.py`; the split still awaits Developer confirmation (TKQ-2) | Developer |
| CQ-DS-2 | Exact next version number after `0.9.0` | A minor step, value confirmed in DS-05 (feat.tasks section 2: "The next version number is for the lane to confirm") | Developer |
| RAQ-1 (carried) | Is `coverage` installed for `/usr/bin/python3` on the runner? | Assume yes; first CI run confirms | Developer / infra |
| RAQ-2 (carried) | Both stories on the single `feature/story` branch, or per-story branches? | Shared `feature/story`, commits prefixed `STORY-03` / `STORY-04` | Developer |

Carried, unchanged: PQ-1 (AC-03.5), TQ-5 (AC-03.6), TQ-4 (AC-04.5), TQ-8 and PQ-5 (TSK-04 release) (feat.stories; feat.tasks); SSQ-1 (journey:task.secrets_scope).

## 8. Validation check

| Check | Result | Source |
|---|---|---|
| Every DS task maps to components | TSK-03 → CMP-1, CMP-2, CMP-4; TSK-04 → CMP-3, CMP-5 | feat.tasks (TSK-03, TSK-04) |
| Files stay inside each task's budget | Yes | feat.tasks (TSK-03, TSK-04 "Files in budget") |
| Existing signatures kept | Yes (`risk=None` is the last parameter) | feat.design (NF-4); feat.trd (IF-4.1) |
| No new technology | Python 3.9 standard library only | feat.design (DD-1) |
| Decisions and lessons | L-007 applied (boundary tests `[30]`, `[31]`, `[10]`, `[11]`). Consistent with D-2026-044 / L-011 (feat.tasks section 8 lists TSK-03 as consistent); the ap_forecast code handles delay days and labels, not monetary amounts (agent judgement from the code) | record:memory; feat.tasks (TSK-03, section 8); tool:repo |
| Conflict with an approved output | None; the test-file difference is listed as CQ-DS-1 | feat.stories (TK-03.2); feat.tasks (section 2) |
| Blocking questions | None | — |
