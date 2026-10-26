<!-- input task.unit_tests for DS-07: from hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.unit_tests.md at commit 3777aa5, produced by DS-06 -->
# task.unit_tests: FEAT-ZD-48600

Unit tests for the Data Science lane of FEAT-ZD-48600. This is step DS-06, Unit Test Cases. The workbook outcome is "Unit testing completed". The tests cover the code that DS-05 changed in `ap_forecast` on branch `feature/story`: STORY-03 (`vendor_risk`) and STORY-04 (`risk=None` on `build_explanation_request`). The tests are not committed yet. The spine commits them after evaluation.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DS-06 Unit Test Cases | record:brief |
| Owner / gate | Developer / no gate: automated checks, the independent judge and sampled human evaluation | record:brief |
| Inputs | task.code (7b73e9c), feat.stories (0590ea4) | record:brief |
| Repository / branch | `ap_forecast`, `feature/story` | record:brief; task.code |
| Allowed paths in this step | `tests/` | record:brief |
| CI command | `/usr/bin/python3 ci.py` (coverage gate `COVERAGE_MIN = 90.0`) | record:brief; file:ap_forecast/ci.py |

## 1. Result

**CI passes: every test is green and line coverage of `src/ap_forecast` is 100.0%, above the 90.0 gate.**

| CI check | Result | Source |
|---|---|---|
| Compile `src` | `true` | tool:ci (`/usr/bin/python3 ci.py`) |
| Tests run / failures / errors | 25 / 0 / 0 | tool:ci |
| Coverage of `src/ap_forecast` | 100.0 (32 statements, 0 missed) | tool:ci |
| Static issues | none (`[]`) | tool:ci |
| `passed` | `true`, exit code 0 | tool:ci |

Per-module coverage from the CI report:

| Module | Statements | Missed | Cover |
|---|---|---|---|
| `src/ap_forecast/__init__.py` | 5 | 0 | 100% |
| `src/ap_forecast/explain.py` | 11 | 0 | 100% |
| `src/ap_forecast/predict.py` | 5 | 0 | 100% |
| `src/ap_forecast/risk.py` | 11 | 0 | 100% |

The coverage comes from the tests alone. There are no exclusion pragmas, `ci.py` is not edited, and no source file is changed in this step (tool:repo; `git status`).

## 2. Files changed

| File | Change | Story |
|---|---|---|
| `tests/test_risk.py` | New: 15 tests in `VendorRiskThresholdTest`, `VendorRiskIsolationTest` and `VendorRiskExportTest` | STORY-03 |
| `tests/test_explain_risk.py` | New: 7 tests in `ExplanationRiskTest` | STORY-04 |
| `tests/test_forecast.py` | Unchanged. Its 3 existing tests still pass, so the existing call forms still work (AC-04.3, NFR-4) | — |

feat.stories SA-6 lets a lane add new test files instead of extending the existing one. Using new files leaves the existing tests exactly as they were.

Blast radius from CodeGraph (mcp:codegraph): `tests/test_risk.py` covers `vendor_risk`. `tests/test_forecast.py` and `tests/test_explain_risk.py` cover `build_explanation_request`.

## 3. Acceptance criteria to tests

### 3.1 STORY-03: `vendor_risk(history)` (`tests/test_risk.py`)

| AC | Test | Input → expected |
|---|---|---|
| AC-03.1 | `test_above_30_is_high` | `[31]` → `'high'`; `[45, 60]` → `'high'` |
| AC-03.2 | `test_exactly_30_is_medium` | `[30]` → `'medium'` (boundary day, L-007) |
| AC-03.3 | `test_between_10_and_30_is_medium` | `[11]`, `[20, 25]` → `'medium'` |
| AC-03.4 | `test_exactly_10_is_low_and_11_is_medium` | `[10]` → `'low'`; `[11]` → `'medium'` (boundary day, L-007) |
| AC-03.5 [Pending PQ-1] | `test_empty_history_is_low` | `[]` → `'low'` |
| AC-03.6 [Pending TQ-5] | `test_early_payments_only_are_low` | `[-5, -20]` → `'low'` |
| AC-03.6 / predict rule | `test_negative_delays_count_as_zero_not_netted` | `[-100, 62]` predicts 31, so `'high'`: a negative delay counts as 0 and is not netted against late payments |
| AC-03 preamble (rounded prediction) | `test_threshold_applies_to_rounded_prediction` | `[30, 30, 31]` → `'medium'`; `[10, 10, 11]` → `'low'`; `[31, 31, 30]` → `'high'` |
| C-1 "predict once" (task.code) | `test_generator_history` | a generator of `[31, 31]` → `'high'` |
| C-1 named constants (task.code) | `test_constants` | `HIGH_ABOVE_DAYS == 30`, `MEDIUM_ABOVE_DAYS == 10` |
| AC-03.7 | `test_no_config_read_or_llm_request` | `explain.load_config` and `explain.build_explanation_request` are patched to raise; `vendor_risk` runs on five histories and calls neither |
| AC-03.7 | `test_module_imports_only_typing_and_predict` | the AST of `risk.py` imports exactly `typing` and `.predict`, with no `explain`, network or config import |
| AC-03.8 | `test_exported_in_all` | `'vendor_risk'` is in `ap_forecast.__all__`, and the package attribute is `risk.vendor_risk` |
| NFR-4 | `test_existing_exports_kept` | `predict_delay_days` and `build_explanation_request` stay in `__all__` |
| AC-03.8 | `test_returns_only_known_levels` | single-value histories from -5 to 60 give exactly `{'high', 'medium', 'low'}` |

### 3.2 STORY-04: `build_explanation_request(..., risk=None)` (`tests/test_explain_risk.py`)

| AC | Test | Input → expected |
|---|---|---|
| AC-04.1 | `test_risk_appended_to_prompt` | for `'high'`, `'medium'` and `'low'`, the prompt is today's prompt followed by `" Risk level: <risk>."` |
| AC-04.1 | `test_risk_positional_fourth_argument` | `risk` passed as the 4th positional argument gives the same result |
| AC-04.2 | `test_without_risk_request_is_unchanged` | with no `risk`, the whole returned dict equals today's dict |
| AC-04.5 [Pending TQ-4] | `test_explicit_none_is_not_given` | `risk=None`, by keyword or by position, gives the same request as no `risk`, with no "Risk level" text |
| AC-04.3 | `test_existing_positional_config_call` | `config` passed positionally still binds to `config` |
| AC-04.4 | `test_provider_model_tokens_from_config_with_risk` | with `risk`, provider, model and `max_tokens` come from the explicit config, and there is one user message |
| AC-04.4 | `test_provider_model_tokens_from_models_json_with_risk` | with no config, provider, model and `max_tokens` equal `load_config()` (`models.json`) |

The test data uses only the synthetic vendor name "Synthetic Vendor A". The tests contain no real vendor, no secret and no personal data.

## 4. Not tested, on purpose

| Item | Why | Source |
|---|---|---|
| `__version__` value `0.10.0` | It is still open as DSQ-1 (Developer). A test would pin an unconfirmed value | task.code (section 7) |
| A `risk` value other than the three labels, such as `""` or `'unknown'` | AC-04.1 appends any value that is not None, but what counts as "given" is still pending TQ-4. The tests use only the three labels `vendor_risk` returns | feat.stories (AC-04.5, TQ-4) |
| Sending the request to `acme-llm` | The code only builds the request and does not send it, so there is no network behaviour to test. Whether the label may go to the provider is TQ-8 | task.code (sections 2.2, 6); feat.stories (STORY-04 open items) |

## 5. Secrets and environments

The tests need no secret and no network. They read only files inside the repository: the packaged `models.json` (through `load_config`, in `tests/test_explain_risk.py` and `tests/test_forecast.py`) and the source of `src/ap_forecast/risk.py` (through `Path(risk_module.__file__).read_text()` in `test_module_imports_only_typing_and_predict`). Per-environment confirmation of secrets access is still the non-blocking SSQ-2 carried by task.code. Nothing in this step depends on it.

## 6. Notes

- The full test suite runs through the CI, so the tests that cover each changed symbol (section 2) all run.
- Python's `round` rounds halves to even, so a history whose mean is exactly 30.5 predicts 30 and gives `'medium'`. This is how the existing `predict_delay_days` behaves, and this step does not change it.
- Two test histories have means that end in exactly .5: `[45, 60]` (mean 52.5, predicts 52) and `[20, 25]` (mean 22.5, predicts 22). Both are far from the thresholds, so rounding up or down gives the same level (`'high'` and `'medium'`). The cases that test rounding at a threshold, in `test_threshold_applies_to_rounded_prediction`, use means of about 30.33, 10.33 and 30.67, none of which ends in .5.

## 7. Open questions (carried, none blocking)

| ID | Question | Owner |
|---|---|---|
| DSQ-1 | Confirm `0.10.0` as the minor version step from `0.9.0` | Developer |
| DSQ-2 | The coverage gate is met in DS-06 (100.0 ≥ 90.0); please confirm this closes DSQ-2 | Developer |
| PQ-1, TQ-4, TQ-5 | The tests for AC-03.5, AC-03.6 and AC-04.5 follow the interim design. If an answer changes that design, only those tests change | PM / Architect |
| TQ-8, PQ-5 | Whether the risk label may be sent to `acme-llm`; the order of this change relative to PROV-ACME-2026-11 | Architect / PM |


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_forecast] Unit tests: 25 run, 0 failing; coverage 100.0%
