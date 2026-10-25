<!-- input task.repo_access for DS-04: from hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.repo_access.md at commit daae080, produced by DS-01 -->
# task.repo_access: FEAT-ZD-48600

Repository and workspace check for the Data Science lane of FEAT-ZD-48600 ("Show which overdue invoices belong to vendors that usually pay late"). This is DS-01 Checking the coding prerequisites. The workbook outcome is that the repository is available with write access.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DS-01 Checking the coding prerequisites | record:brief |
| Owner / gate | Developer / no gate: automated checks, the independent judge and sampled human evaluation | record:brief |
| Inputs | feat.tasks (b0fd4d2), org.engineering_stack (e8afce8) | record:brief |
| Lane tasks | TSK-03 (STORY-03, `risk.vendor_risk`) and TSK-04 (STORY-04, `explain.build_explanation_request(..., risk=None)`) | feat.tasks (sections 1, 3) |

## 1. Result

**The repository is available with write access. No repository creation is needed. No Databricks or other workspace is required.**

| Check | Result | Source |
|---|---|---|
| Repository needed by the lane | `ap_forecast`, the only repository for TSK-03 and TSK-04 | feat.tasks (section 1) |
| Repository exists | Yes, available locally. Existing package `src/ap_forecast/` (`__init__.py`, `predict.py`, `explain.py`, `models.json`), tests in `tests/test_forecast.py`, and `ci.py` | tool:repo-access; file:ap_forecast/src/ap_forecast/__init__.py; file:ap_forecast/tests/test_forecast.py; file:ap_forecast/ci.py |
| Repository creation needed | No. The repository exists. TSK-03 adds new files (`risk.py`, `tests/test_risk.py`) inside it | tool:repo-access; feat.tasks (TSK-03) |
| Remote | `origin` is configured, and local `main` and `dev` track it | file:ap_forecast/.git/config |
| Branches | `main` and `dev` exist, plus the working branch `feature/story`. HEAD is on `feature/story` | file:ap_forecast/.git/HEAD; tool:repo-access |
| Working branch base | `feature/story` was created from `dev` at `d67e866`. Locally `main` and `dev` point to the same commit | tool:repo-access; file:ap_forecast/.git/refs/heads/dev; file:ap_forecast/.git/refs/heads/main |
| Write access | The agent identity has write access, and `git status` is clean | tool:repo-access |
| Language / runtime | Python 3.9 (standard library) | tool:repo-access; org.engineering_stack |
| CI command | `/usr/bin/python3 ci.py`: compile, unit tests, line coverage >= 90% and static checks that stand in for SonarQube | org.engineering_stack; file:ap_forecast/ci.py |
| Package version today | `0.9.0` (TSK-03 owns the version step) | file:ap_forecast/src/ap_forecast/__init__.py; feat.tasks (section 2) |
| Databricks / database | Not used. No repository uses Databricks or a database | org.engineering_stack; tool:repo-access |

## 2. Repository conventions for the lane

| Item | Value | Source |
|---|---|---|
| Branch convention | `main` (released), `dev` (integration), `feature/<story>` | org.engineering_stack; feat.tasks (section 2) |
| Commit message | Starts with the story ID (for example `STORY-03`). The task ID may follow it | org.engineering_stack; feat.tasks (section 2); record:memory |
| Signatures | Keep the existing public signatures and add new functions for new behaviour | file:ap_forecast/CLAUDE.md; feat.tasks (section 2) |
| Static checks in CI | No `print`, `eval` or `exec` calls and no bare `except` in `src/ap_forecast/*.py`. Lines are at most 140 characters | file:ap_forecast/ci.py |

## 3. Open questions (none blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| RAQ-1 | The working branch is literally named `feature/story`. The convention is `feature/<story>`, and this lane covers STORY-03 and STORY-04 in one repository. Should the branch keep this name, or be split or renamed per story? | Use the existing `feature/story` branch. Keep the story ID as the commit prefix for traceability | Developer |
| RAQ-2 | `ci.py` imports the `coverage` package, which is not part of the standard library. This step ran no shell commands, so it did not check that `coverage` is installed for `/usr/bin/python3` | The first CI run in DS-05 or DS-06 confirms it | Developer / infra |
| RAQ-3 | `.git/config` has no upstream entry for `feature/story`. Push rights on `origin` rest on the tool view's write-access statement | Accept the tool view's write-access statement | Developer |

## 4. Validation check

| Check | Result | Source |
|---|---|---|
| Repository available | Pass | tool:repo-access |
| Write access | Pass | tool:repo-access |
| Databricks or other workspace needed | Not applicable | org.engineering_stack; tool:repo-access |
| Blocking questions | None | — |


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_forecast] Branch feature/story at d67e866; write access confirmed
