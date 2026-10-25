# DS-06 Unit Test Cases (Data Science agent)

**Item:** FEAT-ZD-48600 · **status:** done
**Trigger:** Code completion  
**Action:** Write the unit test cases to cover the updated or newly written code as per the requirement documents with at least 90 percent of the code coverage
Access for the secrets in all the environments ²  
**Outcome expected:** Unit testing completed

## Inputs (what the agent was given)

| Item | Prepared input (this repository) | Read from (producer in GitHub) | Commit | Produced by |
|---|---|---|---|---|
| task.code | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-06/task.code.md | hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.code.md | 286af9a | DS-05 |
| feat.stories | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-06/feat.stories.md | hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.stories.md | 0590ea4 | REQ-08 |

Full brief per attempt: `brief_attempt<N>.md` (trigger event, owner answers, tool view, refusal reasons).

## Outputs (what the agent produced)

| Item | GitHub file |
|---|---|
| task.unit_tests | hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.unit_tests.md |

## Code

Repository `ap-forecast-sample`, branch `feature/story`, commit `d67e866`

```
no file changes in this step
```

## How the agent worked

- Approach: Attempt 3 fixes the G2 and G3 refusals by changing only the citations. No test or code changed. The checker does not count file: citations for the test files or ci.py, but the tool view (tool:repo) holds tests/test_risk.py and tests/test_forecast.py in full. Every statement about the tests now cites tool:repo. The 90% gate cites record:brief, which says "at least 90 percent". The CI figures are not in any citable item, so the claims leave them out. They stay in the output file as the verbatim CI output from this attempt's run. I listed the step's context items with context_list to confirm no CI tool view exists, re-ran `/usr/bin/python3 ci.py` (it passed), and checked git status: only files under tests/ changed in this step. Then I rewrote task.unit_tests.md in full with the corrected sources.
- Tools used: mcp__hobasa-context__context_list, Bash (/usr/bin/python3 ci.py, git status), Write
- Helper subagents: none
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 4 | — | — |
| 2 | resumed | 5 | — | — |
| 3 | resumed | 0 | 0 | {'correct': 5, 'complete': 5, 'grounded': 4, 'useful': 5} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- The verbatim CI JSON in section 3 includes 'Ran 11 tests in 0.002s' and the per-file coverage lines. The tool-view footer confirms only '11 run, 0 failing; coverage 100.0%'. The per-file 100% lines follow from a 100.0% total, and their statement counts (5/11/5/9/30) match task.code section 4. The 0.002s run time appears in no source I could see, and the judge's hook did not allow re-running ci.py. The figures are consistent but unverifiable, so consider dropping the timing or attaching the CI output as a tool result.
- test_vendor_risk_no_llm patches ap_forecast.explain.load_config and ap_forecast.explain.build_explanation_request, but risk.py never imports explain, so those mocks can never be called and prove little. The real guarantee comes from the hasattr checks. AC-03.7 also says 'no network call', and no test checks that directly (for example by patching socket or urllib). Say this in the output, or strengthen the test.
- The section 1 statement about `git status` (which files are new or modified, and that only tests/ changed) relies on the agent's own run. The judge could not confirm it with a diff. Citing the tool view for each file's change (new versus modified, and the existing tests unchanged) would make it checkable.
- test_vendor_risk_accepts_any_iterable is a sixth test beyond the five that task.code section 4 planned for CMP-4. That is fine, but stating explicitly that it is an addition beyond the plan would help reviewers.
- feat.stories TK-03.2 suggested putting the STORY-03 tests in tests/test_forecast.py (agent judgement), and SA-6 allows a new file instead. The output handles this correctly as CQ-DS-1. Consider also noting that TK-03.2's suggestion was not followed.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.
