# DS-06 · Unit Test Cases

**Agent:** Data Science · **Item:** FEAT-ZD-48600 · **Status:** ✅ done
**Approved by:** automated checks and the independent judge

**What this step does:** Write the unit test cases to cover the updated or newly written code as per the requirement documents with at least 90 percent of the code coverage
Access for the secrets in all the environments ²

**Expected outcome:** Unit testing completed

**Started by:** Code completion

## Input: what the agent received

| # | Input | What it is | Produced by | Open |
|---|---|---|---|---|
| 1 | `task.code` | Code changes on the working branch | Data Science agent · DS-05 | [input copy](https://github.com/arghya05/hobasa-data-science-agent/blob/main/inputs/FEAT-ZD-48600/DS-06/task.code.md) · [original](https://github.com/arghya05/hobasa-data-science-agent/blob/main/outputs/FEAT-ZD-48600/task.code.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-science-agent/main/docs/FEAT-ZD-48600/DS-05_task-code.html) · [PDF](https://github.com/arghya05/hobasa-data-science-agent/blob/main/docs/FEAT-ZD-48600/DS-05_task-code.pdf) |
| 2 | `feat.stories` | Epics, stories and tasks with numbered AC, frame links, assumptions and open items (latest revision) | Requirement agent · REQ-08 | [input copy](https://github.com/arghya05/hobasa-data-science-agent/blob/main/inputs/FEAT-ZD-48600/DS-06/feat.stories.md) · [original](https://github.com/arghya05/hobasa-requirement-agent/blob/main/outputs/FEAT-ZD-48600/feat.stories.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-requirement-agent/main/docs/FEAT-ZD-48600/REQ-08_feat-stories.html) · [PDF](https://github.com/arghya05/hobasa-requirement-agent/blob/main/docs/FEAT-ZD-48600/REQ-08_feat-stories.pdf) |

The full brief the agent was given, attempt by attempt: `brief_attempt<N>.md` in this folder.

## Output: what the agent produced

| # | Output | What it is | Open |
|---|---|---|---|
| 1 | `task.unit_tests` | Unit tests and coverage (≥90%) | [Markdown](https://github.com/arghya05/hobasa-data-science-agent/blob/main/outputs/FEAT-ZD-48600/task.unit_tests.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-science-agent/main/docs/FEAT-ZD-48600/DS-06_task-unit_tests.html) · [PDF](https://github.com/arghya05/hobasa-data-science-agent/blob/main/docs/FEAT-ZD-48600/DS-06_task-unit_tests.pdf) |

**Code:** repository [ap-forecast-sample](https://github.com/arghya05/ap-forecast-sample) · branch [feature/story](https://github.com/arghya05/ap-forecast-sample/tree/feature/story) · commit [d67e866](https://github.com/arghya05/ap-forecast-sample/commit/d67e866)

```
no file changes in this step
```

## Checks and approval

| Attempt | Session | Problems found by the checks | Unsupported statements (judge) | Judge scores |
|---|---|---|---|---|
| 1 | new | 1 | — | — |
| 2 | continued | 2 | 2 | {'correct': 4, 'complete': 4, 'grounded': 4, 'useful': 4} |
| 3 | continued | 0 | 0 | {'correct': 5, 'complete': 5, 'grounded': 4, 'useful': 5} |

Improvements the independent judge suggests (non-blocking):

- Section 1 cites 'tool:ci' for detail the spine did not record: compile true, 0 errors, static issues [], exit code 0, and the per-module table (5/11/5/11 statements, 32 in total). The spine's tool result only says '25 run, 0 failing; coverage 100.0%'. These figures agree with the code: coverage.py statement counts from the tool view come to 32, and no source line is longer than 140 characters. Still, the citation should point to the spine-recorded CI result, or the output should say the breakdown comes from the agent's own run of ci.py.
- The output says no source file changed and test_forecast.py is unchanged, citing 'git status'. That rests on the agent's own shell output, which is not among the recorded sources. Matching the tool view against the code in task.code supports the claim, but the spine could record the git status or diff so it can be checked.
- In test_no_config_read_or_llm_request, the patch on ap_forecast.explain.build_explanation_request does not affect the name re-exported at package level (ap_forecast.build_explanation_request). The test is still valid only because risk.py never imports explain, and test_module_imports_only_typing_and_predict checks that separately. A note on this pairing would help reviewers.
- AC-04.1 says any non-None value is appended. Section 4 openly leaves out values other than the three labels while TQ-4 is pending. A test with an arbitrary string would still document today's behaviour without fixing the pending meaning of 'given'. This is a design choice for the Developer.

## How the agent worked

- **Approach:** I followed the data-science-steps procedure for DS-06 and read both contracted inputs: task.code, which lists the DS-05 changes C-1 to C-3 and the planned tests C-4 and C-5, and feat.stories, which has AC-03.1 to AC-03.8 and AC-04.1 to AC-04.5. I called the CodeGraph MCP tool (codegraph_explore with the ap_forecast projectPath) to read the exact source of vendor_risk and build_explanation_request and to see which tests cover them. I read ci.py to see the coverage gate. feat.stories SA-6 allows new test files, so the tests are in two new files: tests/test_risk.py for STORY-03 and tests/test_explain_risk.py for STORY-04. tests/test_forecast.py is unchanged. Each acceptance criterion maps to at least one named test. The boundary days 30 and 10 are tested explicitly (lesson L-007), along with rounded-prediction cases, a generator input and the no-LLM, no-config isolation (patched load_config and an AST import check). I ran the full suite through `/usr/bin/python3 ci.py`, and it passed. I checked every statement about the tests against their source, including which files the tests read and which test histories have means ending in .5, and rewrote task.unit_tests.md in full. No source files were changed and nothing was committed.
- **Tools used:** Read, Write, mcp:codegraph (codegraph_explore), Bash:/usr/bin/python3 ci.py, Bash:git status
- **Helper subagents:** none
- **Connector calls:** none

## Files in this folder (audit trail)

- `brief_attempt<N>.md`: the exact brief the agent received
- `result_attempt<N>.json`: the agent's structured answer (approach, tools, claims with their sources, open questions)
- `checks_attempt<N>.json`: what the automated guardrail checks found
- `judge_attempt<N>.json`, `judge_brief_attempt<N>.md`: the independent judge
- `outputs/`: the outputs as approved
