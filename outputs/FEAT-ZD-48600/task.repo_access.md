# task.repo_access: FEAT-ZD-48600

Coding prerequisites for the Data Science lane of FEAT-ZD-48600 (DS-01 Checking the coding prerequisites, attempt 3). This file checks that the repository for this lane's tasks exists, has the expected branches and that the agent identity can write to it. Nothing in the repository was changed in this step. Every statement rests on the step's contracted inputs (feat.tasks, org.engineering_stack), the step's tool view (tool:repo-access), the brief and shared memory.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DS-01 Checking the coding prerequisites (attempt 3) | record:brief |
| Trigger | Task creation (feat.tasks from DES-04) | record:brief; feat.tasks |
| Owner / gate | Developer / automated checks, independent judge, sampled human evaluation | record:brief |
| Inputs | feat.tasks (b0fd4d2), org.engineering_stack (a6b3e2a) | record:brief |

## 1. Repositories this lane needs

| Task | Story | Repository | Source |
|---|---|---|---|
| TSK-03 Rule-based vendor risk level (new `risk.vendor_risk`) | STORY-03 | ap_forecast | feat.tasks (section 1, TSK-03) |
| TSK-04 State the risk level in the explanation prompt (`build_explanation_request(..., risk=None)`) | STORY-04 | ap_forecast | feat.tasks (section 1, TSK-04) |

The Data Science lane needs only one repository, ap_forecast. It already exists, so no repository has to be created (feat.tasks section 1; tool:repo-access).

## 2. Availability and write access

| Check | Result | Source |
|---|---|---|
| Repository available locally | Yes: ap_forecast | tool:repo-access |
| Language | Python 3.9 (standard library) | tool:repo-access; org.engineering_stack |
| Long-lived branches | `main` (released) and `dev` (integration) | org.engineering_stack; feat.tasks (section 2) |
| Working branch | `feature/story`, created from `dev` at `d67e866` | tool:repo-access |
| Working tree | Clean (git status clean) | tool:repo-access |
| Write access for the agent identity | Yes | tool:repo-access |
| Databricks / database / other workspace | Not used by this repository; nothing to provision | tool:repo-access; org.engineering_stack |

**Outcome: the repository is available and the agent identity has write access.** This meets the step outcome.

## 3. What the lane's tasks need from the repository

| Item | Value | Source |
|---|---|---|
| Test framework and CI command | unittest; `/usr/bin/python3 ci.py` | org.engineering_stack |
| CI bar | Compile, unit tests, line coverage >= 90%, static checks standing in for SonarQube; Python 3.9 syntax only, no `print`, `eval`, `exec` or bare `except`, lines at most 140 characters | org.engineering_stack; feat.tasks (section 2) |
| Package version | ap_forecast `0.9.0` today; TSK-03 owns the version step (shared with TSK-04) | feat.tasks (section 2, TSK-03) |
| Existing function TSK-03 builds on | `predict_delay_days`, used unchanged | feat.tasks (TSK-03) |
| Existing function TSK-04 extends | `build_explanation_request`, with an additive `risk=None` parameter | feat.tasks (TSK-04) |
| Files in TSK-03 budget | New `src/ap_forecast/risk.py`; `src/ap_forecast/__init__.py` (import, `__all__`, version step); new `tests/test_risk.py` | feat.tasks (TSK-03) |
| Files in TSK-04 budget | `src/ap_forecast/explain.py`; `tests/test_forecast.py` | feat.tasks (TSK-04) |
| Must not change | `predict.py`, `models.json`; existing signatures (add new functions or additive parameters only) | feat.tasks (section 2, TSK-03, TSK-04) |

## 4. Branch and commit conventions to apply

- Feature branches are per story (`feature/<story>`) (org.engineering_stack; feat.tasks section 2).
- Commit messages start with the story ID (`STORY-03`, `STORY-04`); the task ID may follow (feat.tasks section 2; org.engineering_stack; record:memory conventions).
- This agent does not merge, deploy or push; the spine commits and pushes after evaluation and approval (record:brief).

## 5. Open questions (non-blocking)

| ID | Question | Interim assumption | Owner (proposed) |
|---|---|---|---|
| RAQ-1 | feat.tasks and org.engineering_stack call for a feature branch per story (`feature/<story>`), but the tool view reports one branch literally named `feature/story`. Should TSK-03 (STORY-03) and TSK-04 (STORY-04) share it, or be split into per-story branches? | Both Data Science tasks are built on `feature/story`, with commits prefixed by their own story ID for traceability | Developer |
| RAQ-2 | This step may run no shell commands, so whether the CI command runs in this environment (interpreter and any coverage tooling it needs) was not checked | The first CI run in DS-05/DS-06 confirms it | Developer / infra |
| RAQ-3 | Push permission to the remote was not tested in this step (pushes are spine-only); write access is taken from the tool view | Write access as stated by tool:repo-access | Spine / Developer |


[Tool result] [ap_forecast] Branch feature/story at d67e866; write access confirmed