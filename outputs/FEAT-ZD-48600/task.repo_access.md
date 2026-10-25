# task.repo_access: FEAT-ZD-48600

This record covers repository availability and write access for the Data Science lane of FEAT-ZD-48600 ("Show which overdue invoices belong to vendors that usually pay late"). It is step DS-01, Checking the coding prerequisites. The step has no gate: automated checks, the independent judge and sampled human evaluation review it.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DS-01 Checking the coding prerequisites | record:brief |
| Inputs | feat.tasks (b0fd4d2), org.engineering_stack (76d03ce) | record:brief |
| Lane tasks | TSK-03 (STORY-03, `vendor_risk`), TSK-04 (STORY-04, `build_explanation_request(..., risk=None)`) | feat.tasks (section 1) |

## 1. Result

**Outcome: met.** The repository `ap_forecast` is available locally, and the agent identity has write access to it. The working branch is `feature/story`, created from `dev`. No repository needs to be created, and this lane uses no Databricks or other workspace.

## 2. Repository availability

| Check | Result | Source |
|---|---|---|
| Repository needed by the lane | `ap_forecast` (Data Science lane, TSK-03 and TSK-04) | feat.tasks (section 1) |
| Available locally | Yes | tool:repo-access |
| Repository creation needed | No. The repository already exists and has the branches `main` and `dev` | tool:repo-access; file:ap_forecast/.git/refs/heads/main; file:ap_forecast/.git/refs/heads/dev |
| Remote | `origin` points to GitHub repository `arghya05/ap-forecast-sample` | file:ap_forecast/.git/config |
| Language and runtime | Python 3.9, standard library; tests use unittest | tool:repo-access; org.engineering_stack; file:ap_forecast/CLAUDE.md |
| CI command | `/usr/bin/python3 ci.py`: compile, unit tests, line coverage of at least 90%, and static checks in place of SonarQube | org.engineering_stack; file:ap_forecast/ci.py; file:ap_forecast/README.md |
| Current package version | `0.9.0` (`src/ap_forecast/__init__.py`), which matches feat.tasks section 2 | file:ap_forecast/src/ap_forecast/__init__.py; feat.tasks (section 2) |
| Existing code the lane builds on | `src/ap_forecast/predict.py` defines `predict_delay_days` | file:ap_forecast/src/ap_forecast/predict.py |
| | `src/ap_forecast/explain.py` defines `build_explanation_request` and reads its provider and model from `models.json` | file:ap_forecast/src/ap_forecast/explain.py |
| | `tests/test_forecast.py` holds `test_predict`, `test_request_uses_config` and `test_request_with_explicit_config` | file:ap_forecast/tests/test_forecast.py |

## 3. Branch and write access

| Check | Result | Source |
|---|---|---|
| Checked-out branch | `feature/story` (HEAD) | file:ap_forecast/.git/HEAD; tool:repo-access |
| Branch base | Created from `dev` at `d67e866`. Today `dev` and `main` point to the same commit | tool:repo-access; file:ap_forecast/.git/logs/refs/heads/feature/story; file:ap_forecast/.git/refs/heads/dev; file:ap_forecast/.git/refs/heads/main |
| Working tree | Clean | tool:repo-access |
| Write access for the agent identity | Yes | tool:repo-access |
| Branch convention | `feature/<story>` branches. Commit messages start with the story ID (`STORY-03` or `STORY-04`), and the task ID may follow | feat.tasks (section 2); org.engineering_stack; record:memory |

## 4. Databricks and database

| Check | Result | Source |
|---|---|---|
| Databricks workspace | Not used by `ap_forecast`, so no access is needed | tool:repo-access; org.engineering_stack |
| Database | None. No repository uses a database | org.engineering_stack |

## 5. Readiness for the lane tasks

| Task | Files in budget | Repository ready | Source |
|---|---|---|---|
| TSK-03 (STORY-03) | New `src/ap_forecast/risk.py`; `src/ap_forecast/__init__.py` (import, `__all__`, version step); new `tests/test_risk.py` | Yes. `src/ap_forecast/__init__.py` exists, and so do the folders `src/ap_forecast/` and `tests/` that the new files go into | feat.tasks (TSK-03); file:ap_forecast/src/ap_forecast/__init__.py; file:ap_forecast/tests/test_forecast.py |
| TSK-04 (STORY-04) | `src/ap_forecast/explain.py`; `tests/test_forecast.py` | Yes. Both files exist | feat.tasks (TSK-04); file:ap_forecast/src/ap_forecast/explain.py; file:ap_forecast/tests/test_forecast.py |

## 6. Observations (non-blocking)

| ID | Observation | Interim handling | Source |
|---|---|---|---|
| RA-1 | The working branch is `feature/story`, not a story-specific name such as `feature/STORY-03`. TSK-03 and TSK-04 would share one branch | Use `feature/story`. The commit prefixes `STORY-03` and `STORY-04` keep each commit traceable | tool:repo-access; feat.tasks (section 2) |
| RA-2 | `ci.py` imports the third-party package `coverage`, while the library code uses only the standard library. CI was not run in this step, so it is not confirmed that `coverage` is installed for `/usr/bin/python3` | Confirm at the first CI run (DS-06 or DS-07) | file:ap_forecast/ci.py; file:ap_forecast/CLAUDE.md |

## 7. Open questions

None are blocking. RA-1 and RA-2 are listed for the Developer to confirm.


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_forecast] Branch feature/story at d67e866; write access confirmed
