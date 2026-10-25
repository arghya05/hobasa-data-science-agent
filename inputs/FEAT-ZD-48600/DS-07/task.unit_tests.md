<!-- input task.unit_tests for DS-07: from hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.unit_tests.md at commit 79527ef, produced by DS-06 -->
# task.unit_tests: FEAT-ZD-48600 (Data Science lane, ap_forecast)

DS-06 Unit Test Cases (attempt 3). Workbook action: write the unit test cases that cover the updated or new code, as the
requirement documents describe, with at least 90% code coverage. Also confirm secrets access in all environments.

**Result: CI passes. The tests planned as CMP-4 and CMP-5 in task.code are now in `tests/`. The CI run in this attempt
(`/usr/bin/python3 ci.py`, output in section 3) reports 11 tests run, 0 failures, 0 errors, 100.0% coverage, no static
issues and `"passed": true`. This closes task.code open question CQ-C1. Only files under `tests/` changed in this step.
Nothing is committed: the spine commits.**

**Refusal fixes in this attempt:** No test or code changed. Statements about the test files now cite the tool view
(tool:repo), which shows `tests/test_risk.py` and `tests/test_forecast.py` in full. They no longer cite `file:` paths.
The 90% gate cites the brief (record:brief), not `ci.py`. CI figures come from the CI run shown verbatim in section 3,
re-run in this attempt. The structured-result claims leave those figures out, because no citable context item holds
them.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DS-06 Unit Test Cases (attempt 3) | record:brief |
| Repository / branch | ap_forecast / `feature/story`, working tree only (not committed) | record:brief; task.code (header) |
| Stories | STORY-03 (`vendor_risk`), STORY-04 (`build_explanation_request` with `risk`) | task.code (section 1); feat.stories (STORY-03, STORY-04) |
| Write scope of this step | `tests/` | record:brief |
| Inputs | task.code (286af9a), feat.stories (0590ea4) | record:brief |
| Code and tests under test | `src/ap_forecast/*`, `tests/test_forecast.py`, `tests/test_risk.py` | tool:repo |

## 1. Changed test files

`git status` after this step: the files under `tests/` are new or modified here. The other entries (`README.md`,
`src/ap_forecast/__init__.py`, `src/ap_forecast/explain.py`, `docs/`, `src/ap_forecast/risk.py`) come from DS-05
(task.code section 1) and were not touched.

| File | Change | Component (task.code) | Story | Source |
|---|---|---|---|---|
| `tests/test_risk.py` | **New.** `VendorRiskTest` with six tests: `test_vendor_risk_levels`, `test_vendor_risk_boundaries`, `test_vendor_risk_empty_and_early`, `test_vendor_risk_accepts_any_iterable`, `test_vendor_risk_no_llm`, `test_vendor_risk_exported` | CMP-4 | STORY-03 | tool:repo; task.code (section 4) |
| `tests/test_forecast.py` | Adds `test_request_with_risk` and `test_request_without_risk_unchanged`. The existing tests (`test_predict`, `test_request_uses_config`, `test_request_with_explicit_config`) are unchanged | CMP-5 | STORY-04 | tool:repo; task.code (section 4) |

Test file split (CQ-DS-1): the `vendor_risk` tests go in a new `tests/test_risk.py`, as task.code section 4 plans for
CMP-4. feat.stories SA-6 lets a lane add new test files instead of using the existing one. The explanation tests stay in
`tests/test_forecast.py`, next to the existing `build_explanation_request` tests.

## 2. Test cases and acceptance criteria

### 2.1 STORY-03: `vendor_risk` (`tests/test_risk.py`)

Source for this table: tool:repo (`tests/test_risk.py`, `risk.py`, `predict.py`); feat.stories (STORY-03).

| Test | What it checks | AC |
|---|---|---|
| `test_vendor_risk_levels` | `[31]` and `[40, 50, 60]` give `'high'`; `[20]` and `[5, 15, 25]` give `'medium'`; `[3]` gives `'low'` | AC-03.1, AC-03.3 |
| `test_vendor_risk_boundaries` | `[30]` gives `'medium'`, `[31]` gives `'high'`, `[10]` gives `'low'`, `[11]` gives `'medium'`. The thresholds apply to the **rounded** prediction: `[30, 31]` predicts 30, so `'medium'`; `[10, 11]` predicts 10, so `'low'`; `[31, 32]` predicts 32, so `'high'`. `HIGH_ABOVE_DAYS` and `MEDIUM_ABOVE_DAYS` are 30 and 10 | AC-03.2, AC-03.4 (L-007 boundary day, record:memory) |
| `test_vendor_risk_empty_and_early` | `[]` gives `'low'`; `[-5, -20]` gives `'low'`; `[-100, 31]` gives `'medium'` (negative delays count as 0) | AC-03.5 [Pending PQ-1], AC-03.6 [Pending TQ-5] |
| `test_vendor_risk_accepts_any_iterable` | A generator and an empty iterator work, because `history` is read once, through `predict_delay_days` | `Iterable[int]` signature (task.code section 2) |
| `test_vendor_risk_no_llm` | With `explain.load_config` and `explain.build_explanation_request` patched, `vendor_risk` calls neither of them. The `risk` module has no `load_config` attribute and no `explain` attribute | AC-03.7 |
| `test_vendor_risk_exported` | `vendor_risk` is in `ap_forecast.__all__` and is the `risk` module's function. Over the delays -10 to 100 it returns exactly the set `{'high', 'medium', 'low'}` | AC-03.8 |

### 2.2 STORY-04: `build_explanation_request(..., risk=None)` (`tests/test_forecast.py`)

Source for this table: tool:repo (`tests/test_forecast.py`, `explain.py`, `models.json`); feat.stories (STORY-04).

| Test | What it checks | AC |
|---|---|---|
| `test_request_with_risk` | With an explicit config passed positionally and `risk="high"`, the message content is exactly today's prompt followed by `" Risk level: high."`. Provider, model and `max_tokens` come from the config. With `risk="medium"` as a keyword and no config, the prompt ends with `" Risk level: medium."`, and provider, model and `max_tokens` come from `models.json` (`acme-llm`, `acme-text-2`, 300) | AC-04.1, AC-04.4 |
| `test_request_without_risk_unchanged` | The whole returned dict equals today's dict when `risk` is left out, passed positionally as `None`, or passed as `risk=None` with `config=` as a keyword. The default-config request is the same with and without `risk=None`, and contains no `"Risk level"` | AC-04.2, AC-04.3, AC-04.5 [Pending TQ-4] |

The existing tests `test_predict`, `test_request_uses_config` and `test_request_with_explicit_config` pass unchanged
(tool:repo; CI run in section 3). They are the regression check for AC-04.3: the existing positional `config` call still
works.

Every new ap_forecast acceptance criterion, AC-03.1 to AC-03.8 and AC-04.1 to AC-04.5 (feat.stories), is covered by at
least one test in the tables above.

## 3. CI result (`/usr/bin/python3 ci.py`, re-run in this attempt)

Verbatim last line of the CI output from the run in this attempt:

```
{"compile": true, "tests_run": 11, "failures": 0, "errors": 0, "coverage": 100.0, "coverage_report": "Name                          Stmts   Miss  Cover   Missing\n-----------------------------------------------------------\nsrc/ap_forecast/__init__.py       5      0   100%\nsrc/ap_forecast/explain.py       11      0   100%\nsrc/ap_forecast/predict.py        5      0   100%\nsrc/ap_forecast/risk.py           9      0   100%\n-----------------------------------------------------------\nTOTAL                            30      0   100%\n", "test_output": "...........\n----------------------------------------------------------------------\nRan 11 tests in 0.002s\n\nOK\n", "static_issues": [], "passed": true}
```

| Check | Before (task.code, DS-05) | After (CI run above) |
|---|---|---|
| Compile | Pass | Pass |
| Tests run / failures / errors | 3 / 0 / 0 | 11 / 0 / 0 |
| Coverage, total (gate: at least 90%, record:brief) | 83.3, **fail** | 100.0, **pass** |
| `risk.py` coverage | 56%, lines 17-20 missed | 100% |
| `explain.py` coverage | 91%, line 21 missed | 100% |
| Static issues | none | none |
| Overall `passed` | false | **true** |

Sources: task.code (section 4) for "Before"; the CI run above for "After"; record:brief for the 90% gate.

## 4. Secrets, database and environments

| Item | Finding | Source |
|---|---|---|
| Secrets used by the tests | None. No credential, token or environment variable is read; the tests use an in-memory config dict or the unchanged `models.json` | tool:repo; task.code (section 5) |
| Network / LLM calls | None. `build_explanation_request` only builds a dict; `vendor_risk` is checked to make no config read | tool:repo; task.code (section 5) |
| Secrets in any environment (local/CI, `feature/<story>`, `dev`, `main`) | None needed for these tests, as for the code | task.code (section 5) |
| Database / Databricks | None | task.code (section 5) |

## 5. Open questions (non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| CQ-C1 (task.code) | Coverage gate after DS-05 | **Closed**: CI passes (section 3) | Developer |
| CQ-DS-1 | Test file split | New `tests/test_risk.py` for STORY-03; STORY-04 tests added to `tests/test_forecast.py` (section 1). Confirm at review | Developer |
| UT-1 | AC-03.5 (PQ-1), AC-03.6 (TQ-5) and AC-04.5 (TQ-4) are tested against interim answers (feat.stories). If an answer changes, the matching assertion changes too | Keep the interim answers | PM / Architect |
| Carried | CQ-C2 (version `0.10.0`), CQ-DS-3, RAQ-1, TQ-8, PQ-5 (task.code section 7) | Unchanged | As in task.code |

## 6. Validation check

| Check | Result | Source |
|---|---|---|
| Only `tests/` changed in this step | Yes (`git status`) | record:brief; task.code (section 1) |
| Every new or changed line covered | Yes: no missed statements | CI run (section 3) |
| Coverage of at least 90% | Yes | CI run (section 3); record:brief |
| All tests pass; existing tests unchanged | Yes: no failures or errors | CI run (section 3); tool:repo |
| Boundary day tested explicitly (L-007) | Yes: 30 and 10, and their rounded-mean versions | record:memory; tool:repo |
| Commit / push | Not done (the spine commits); commit messages to start with `STORY-03` / `STORY-04` | record:brief; record:memory |
| Blocking questions | None | — |


[Tool result] [ap_forecast] Unit tests: 11 run, 0 failing; coverage 100.0%