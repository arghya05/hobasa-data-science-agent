# task.repo_access: FEAT-ZD-48600

Coding prerequisites for the Data Science lane of FEAT-ZD-48600. This is DS-01, "Checking the coding prerequisites". The workbook action is to check that the repository is available, create it if needed, and confirm write access.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DS-01 Checking the coding prerequisites | record:brief |
| Owner / gate | Developer / automated checks, independent judge, sampled human evaluation | record:brief |
| Inputs | feat.tasks (b0fd4d2), org.engineering_stack (b6a392f) | record:brief |
| Lane tasks | TSK-03 (STORY-03, `vendor_risk`), TSK-04 (STORY-04, `build_explanation_request(..., risk=None)`) | feat.tasks (sections 1, 3) |

## 1. Repositories this lane needs

| Repository | Needed by | Language / test runner | CI command | Source |
|---|---|---|---|---|
| ap_forecast | TSK-03, TSK-04 | Python 3.9, unittest | `/usr/bin/python3 ci.py` | feat.tasks (section 1); org.engineering_stack |

Only ap_forecast is needed. The other tasks belong to other lanes: ap_pipeline (TSK-01, TSK-02, Data Engineering), ap_aging (TSK-05, TSK-06, Backend), ap_portal (TSK-07, Frontend) and the cross-repository harness (TSK-08, Testing Agent) (feat.tasks section 1). This lane does not change those repositories.

## 2. Availability and write access

| Check | Result | Source |
|---|---|---|
| Repository available | Yes. ap_forecast is checked out locally | tool:repo-access |
| Repository creation needed | No. The repository already exists; nothing is created | tool:repo-access |
| Remote | `origin` is configured on GitHub (`ap-forecast-sample`); `main` and `dev` track `origin` | file:ap_forecast/.git/config |
| Working branch | `feature/story`, created from `dev` at `d67e866`. The checkout's HEAD points to `refs/heads/feature/story` | tool:repo-access; file:ap_forecast/.git/HEAD |
| Branch tips | `main`, `dev` and `feature/story` all point to `d67e866…`, so the feature branch has no commits yet | file:ap_forecast/.git/refs/heads/main; file:ap_forecast/.git/refs/heads/dev; file:ap_forecast/.git/logs/refs/heads/feature/story |
| Branch roles | `main` (released), `dev` (integration), `feature/<story>` | file:ap_forecast/CLAUDE.md; org.engineering_stack |
| Write access | Yes. The agent identity has write access; git status is clean | tool:repo-access |
| Push | Done by the spine, not this agent. `feature/story` has no upstream configured yet; the spine sets it on first push | file:ap_forecast/.git/config |
| Databricks / workspace / database | Not used. No Databricks or other workspace is used by ap_forecast, and no repository uses Databricks or a database | tool:repo-access; org.engineering_stack |

## 3. Repository readiness for TSK-03 and TSK-04

| Item | Observed | Source |
|---|---|---|
| Code and test locations | Code in `src/ap_forecast/`, tests in `tests/` | file:ap_forecast/CLAUDE.md |
| Existing functions | `predict.py` has `predict_delay_days(history)`, which TSK-03 calls. `explain.py` has `build_explanation_request(vendor, predicted_days, config=None)` and `load_config()`, which TSK-04 extends with `risk=None`. `risk.py` does not exist yet; TSK-03 creates it | file:ap_forecast/src/ap_forecast/predict.py; file:ap_forecast/src/ap_forecast/explain.py; feat.tasks (TSK-03, TSK-04) |
| Existing tests | `tests/test_forecast.py` has one `ForecastTest` class with `test_predict`, `test_request_uses_config` and `test_request_with_explicit_config`. TSK-03 adds `tests/test_risk.py`; TSK-04 extends `tests/test_forecast.py` | file:ap_forecast/tests/test_forecast.py; feat.tasks (section 2) |
| Current version | `__version__ = "0.9.0"`; TSK-03 owns the version step | file:ap_forecast/src/ap_forecast/__init__.py; feat.tasks (section 2) |
| Current exports | `__all__ = ["predict_delay_days", "build_explanation_request"]` | file:ap_forecast/src/ap_forecast/__init__.py |
| CI gate | `ci.py` compiles, runs the unit tests, requires coverage of at least 90 (`COVERAGE_MIN = 90.0`) and runs static checks for `print`, `eval`, `exec`, bare `except` and lines longer than 140 characters | file:ap_forecast/ci.py; org.engineering_stack |
| CI dependency | `ci.py` imports the third-party `coverage` package. CI is not run in this step, so its presence for `/usr/bin/python3` is not confirmed here (RAQ-1) | file:ap_forecast/ci.py |
| Commit convention | Commit messages start with the story ID (`STORY-03`, `STORY-04`); the task ID may follow | org.engineering_stack; feat.tasks (section 2); record:memory |

## 4. Observations

1. **Branch name.** feat.tasks, org.engineering_stack and the repository's CLAUDE.md call for a feature branch per story (`feature/<story>`). The provided branch is the single `feature/story`, while this lane has two stories (STORY-03, STORY-04). This step uses `feature/story` for both tasks, as the tool view provides; story-ID commit prefixes keep each commit traceable to its story (non-blocking RAQ-2).
2. **How access was established.** DS-01 may not run shell commands under its tool policy. Availability, branch origin and write access come from the spine's repository-access tool view; branch tips, remote, layout and existing code and tests come from reading files in the checkout. No product code was changed and no git or CI command was run in this step.

## 5. Outcome

Outcome met: **ap_forecast is available with write access on `feature/story` (from `dev` at `d67e866`)**. No repository creation and no Databricks or database access are needed for TSK-03 or TSK-04 (tool:repo-access; org.engineering_stack).

## 6. Open questions (non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| RAQ-1 | Is the `coverage` package that `ci.py` imports installed for `/usr/bin/python3` on the runner? | Assume yes; the first CI run in the coding steps confirms it | Developer / infra |
| RAQ-2 | Should STORY-03 and STORY-04 use separate `feature/STORY-03` and `feature/STORY-04` branches instead of the single `feature/story`? | Use `feature/story` for both, with story-prefixed commits | Developer |


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_forecast] Branch feature/story at d67e866; write access confirmed
