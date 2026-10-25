# task.commit: FEAT-ZD-48600 (Data Science lane, ap_forecast)

DS-07 Commit & Push (attempt 4). Workbook action: commit the code with a message that traces back to the user story,
review and resolve any merge conflicts, push to the central repository, and confirm secrets access in all environments.

**Result: the STORY-03 and STORY-04 code is committed on `feature/story` (commit `2e6fb82`), and CI passes on it.
The commit message is `STORY # task.commit: FEAT-ZD-48600 (Data Science lane, ap_forecast)`. It does not start with
a story ID, so it breaks the convention and does not trace to STORY-03 or STORY-04. Section 2 gives the corrected
message, which the spine should apply before the push; this step may run no shell commands. The push to the central
repository is the spine's job and follows this step's approval. No merge conflict needed manual resolution.**

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DS-07 Commit & Push (attempt 4) | record:brief |
| Repository / branch | ap_forecast / `feature/story` | tool:repo; task.code (header) |
| Stories | STORY-03 (`vendor_risk`), STORY-04 (`build_explanation_request` with `risk`) | feat.stories (STORY-03, STORY-04); task.code (section 1) |
| Inputs | task.code (286af9a), task.unit_tests (79527ef), feat.stories (0590ea4) | record:brief |
| Shell in this step | Blocked by hook G9: DS-07 may run no shell commands, and git push, merge and reset are run only by the spine | tool:bash (hook response) |

## 1. The commit

| Item | Value | Source |
|---|---|---|
| Commit | `2e6fb82` on `feature/story`, made by the spine | tool:repo |
| Message (as committed) | `STORY # task.commit: FEAT-ZD-48600 (Data Science lane, ap_forecast)` | tool:repo |
| CI on the commit | Passed: tests 11, coverage 100.0%, static issues 0 | tool:repo |
| Push to the central repository | Done by the spine after this step's checks and evaluation pass, not by this agent | record:brief; tool:bash (hook response) |

### 1.1 Files in the change

| File | Change | Produced by | Story | Source |
|---|---|---|---|---|
| `src/ap_forecast/risk.py` | New: `HIGH_ABOVE_DAYS = 30`, `MEDIUM_ABOVE_DAYS = 10`, `vendor_risk(history: Iterable[int]) -> str`, strict `>` | DS-05 | STORY-03 | task.code (sections 1, 2) |
| `src/ap_forecast/__init__.py` | Exports `vendor_risk` in `__all__`; `__version__` goes from `"0.9.0"` to `"0.10.0"` | DS-05 | STORY-03 | task.code (sections 1, 2) |
| `tests/test_risk.py` | New: `VendorRiskTest` with six tests (levels, boundaries, empty and early, any iterable, no LLM, exported) | DS-06 | STORY-03 | task.unit_tests (sections 1, 2.1) |
| `src/ap_forecast/explain.py` | `build_explanation_request(..., risk=None)`; appends `" Risk level: {risk}."` only when `risk is not None` | DS-05 | STORY-04 | task.code (sections 1, 2) |
| `tests/test_forecast.py` | Adds `test_request_with_risk` and `test_request_without_risk_unchanged`; `test_predict`, `test_request_uses_config` and `test_request_with_explicit_config` are unchanged | DS-06 | STORY-04 | task.unit_tests (sections 1, 2.2) |
| `README.md` | Public API table, example, CI command | DS-05 | STORY-03, STORY-04 | task.code (section 1) |
| `docs/ap_forecast.md` | New module documentation and changelog | DS-05 | STORY-03, STORY-04 | task.code (section 1) |

`src/ap_forecast/predict.py` and `models.json` are unchanged (task.code section 1).

## 2. Commit message: fix before the push

The convention is "Commit messages start with the story ID" (record:memory). The committed message starts with
`STORY #`, which is not a story ID. It names neither STORY-03 nor STORY-04, and the rest of it is this output file's
heading, not a description of the change. In that form, `git log` cannot be traced back to the stories.

**Requested action (spine, before the push):** reword the message of the commit on `feature/story`. Only the message
changes; the tree stays the same. Use this message:

```
STORY-03, STORY-04: add vendor_risk and risk-aware explanation request

FEAT-ZD-48600 / EP-1 / FT-2 / TK-03.1, TK-03.2, TK-04.1, TK-04.2

STORY-03 (AC-03.1 to AC-03.8)
- New src/ap_forecast/risk.py: vendor_risk(history) returns 'high' when the
  predicted delay is > 30 days, 'medium' when > 10, else 'low' (strict >, on
  the rounded prediction from predict_delay_days). No LLM call and no
  models.json read.
- Export vendor_risk in __all__; version 0.9.0 -> 0.10.0.
- New tests/test_risk.py: levels, boundaries 30/10 (L-007), empty and early
  histories, any iterable, no LLM, export.

STORY-04 (AC-04.1 to AC-04.5)
- build_explanation_request(vendor, predicted_days, config=None, risk=None):
  when risk is not None the prompt ends with " Risk level: <risk>."; without
  risk (or risk=None) the request is unchanged. Provider, model and max_tokens
  still come from config or models.json.
- tests/test_forecast.py: test_request_with_risk and
  test_request_without_risk_unchanged; existing tests unchanged.

Docs: README.md and new docs/ap_forecast.md.
```

The one commit holds both stories, so the subject names both. Splitting it into one commit per story would need a
reset, and only the spine may run that (tool:bash hook response). A reworded single commit is enough for
traceability.

## 3. CI result

| Check | Result | Source |
|---|---|---|
| Compile | Pass | task.unit_tests (section 3) |
| Tests | 11 run, 0 failures, 0 errors | task.unit_tests (section 3); tool:repo |
| Coverage (gate: at least 90%) | 100.0, pass | task.unit_tests (section 3); tool:repo; record:brief |
| Static issues | None | task.unit_tests (section 3); tool:repo |
| Overall | Passed | task.unit_tests (section 3); tool:repo |

The DS-06 run on the working tree and the tool view's CI on the commit report the same figures. Rewording the
message changes no file, so the result still holds after the reword.

## 4. Merge conflicts

The spine's commit on `feature/story` succeeded, and CI passed on it (tool:repo). No conflict was reported and nothing
needed manual review or resolution. The change touches only ap_forecast files that this lane owns (task.code
section 1). This step cannot fetch the remote (tool:bash hook response). If the remote branch has moved by the time
the spine pushes, the push will be rejected, and the conflict goes to the Developer for manual review (open question
CP-2).

## 5. Push target

| Item | Value | Source |
|---|---|---|
| Push | `feature/story` to the central ap_forecast repository, after the reword in section 2 | tool:repo; record:brief |
| Not pushed | `main` and `dev`. This agent may not push to protected main or merge | record:brief (guardrails) |
| PR | Not raised in this step (open question CP-3) | record:brief |

## 6. Secrets, database and environments

| Item | Finding | Source |
|---|---|---|
| Secrets in the commit | None. A search of the checkout for `api_key`, `secret`, `token` and `password` found only the `max_tokens` config field | tool:grep; task.code (section 5) |
| Secrets needed by the code or the tests in any environment (local/CI, `feature/<story>`, `dev`, `main`) | None | task.code (section 5); task.unit_tests (section 4) |
| Secrets needed for the push | Write credentials for the central repository. The spine runs the push; this agent does not use them | tool:bash (hook response); record:brief |
| Database / Databricks | None | task.code (section 5) |

## 7. Story traceability

| Story | Acceptance criteria | Code | Tests | Commit |
|---|---|---|---|---|
| STORY-03 | AC-03.1 to AC-03.8 | `risk.py`, `__init__.py` | `tests/test_risk.py` | Spine commit on `feature/story` (message to be reworded) |
| STORY-04 | AC-04.1 to AC-04.5 | `explain.py` | `tests/test_forecast.py` | Spine commit on `feature/story` (message to be reworded) |

Sources: feat.stories (STORY-03, STORY-04); task.code (section 3); task.unit_tests (section 2); tool:repo.

## 8. Open questions (non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| CP-0 | The spine's commit has the message `STORY # task.commit: …`, which does not start with a story ID. Will the spine reword it to the section 2 message before the push? | Reword the message only, then push; do not push the current message | Developer / spine owner |
| CP-1 | The workbook says "push the code to central/main repository", but agents may not push to protected main. Is pushing `feature/story` the intended reading? | Push `feature/story` only; `dev` and `main` get the change through the PR and the deployment merge | Developer |
| CP-2 | This step cannot fetch the remote to confirm that `feature/story` has not moved. Will the spine fetch before the push and send any conflict to the Developer? | Fetch before the push; on rejection, the Developer resolves manually | Developer / spine owner |
| CP-3 | The workbook outcome says "PR is being raised". Is the PR expected from this step, or from the later PR step? | The later PR step raises it, after the push | Developer |
| Carried | CQ-C2 (version `0.10.0`), CQ-DS-1 (test file split), UT-1 (AC-03.5, AC-03.6, AC-04.5 rest on interim answers), CQ-DS-3, RAQ-1, TQ-8, PQ-5 | Unchanged | As in task.code and task.unit_tests |

## 9. Validation check

| Check | Result | Source |
|---|---|---|
| Code committed on the story branch | Yes, on `feature/story` | tool:repo |
| Commit message traces to the story | **No, as committed.** The corrected message is in section 2 (CP-0) | record:memory; tool:repo |
| CI green on the commit (compile, tests, coverage of at least 90%, static) | Yes (section 3) | task.unit_tests (section 3); tool:repo; record:brief |
| Merge conflicts | None reported (section 4) | tool:repo |
| Pushed | Left to the spine, after approval | record:brief; tool:bash (hook response) |
| No secrets or personal data in the commit | Yes (section 6) | tool:grep |
| No push to `main`, no merge, no self-approval by this agent | Yes | record:brief; tool:bash (hook response) |
| Blocking questions | None | — |


[Tool result] [ap_forecast] Commit 2e6fb82 on feature/story: 'STORY # task.commit: FEAT-ZD-48600 (Data Science lane, ap_forecast)'. CI PASSED; tests 11; coverage 100.0%; static issues 0