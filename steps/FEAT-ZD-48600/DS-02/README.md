# DS-02 · Access to secrets

**Agent:** Data Science · **Item:** FEAT-ZD-48600 · **Status:** ✅ done
**Approved by:** automated checks and the independent judge

**What this step does:** Check the access for required secrets and ask for access as needed

**Expected outcome:** Secrets are available with access permissions

**Started by:** Task creation, or any re-entry trigger ¹

## Input: what the agent received

| # | Input | What it is | Produced by | Open |
|---|---|---|---|---|
| 1 | `feat.tasks` | Engineering tasks with goal, AC, dependencies and size budget | Design agent · DES-04 | [input copy](https://github.com/arghya05/hobasa-data-science-agent/blob/main/inputs/FEAT-ZD-48600/DS-02/feat.tasks.md) · [original](https://github.com/arghya05/hobasa-design-agent/blob/main/outputs/FEAT-ZD-48600/feat.tasks.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-design-agent/main/docs/FEAT-ZD-48600/DES-04_feat-tasks.html) · [PDF](https://github.com/arghya05/hobasa-design-agent/blob/main/docs/FEAT-ZD-48600/DES-04_feat-tasks.pdf) |
| 2 | `org.permission_table` | Allowed tools, secrets and data per agent | — | not applicable for this item, or passed by reference |

The full brief the agent was given, attempt by attempt: `brief_attempt<N>.md` in this folder.

## Output: what the agent produced

| # | Output | What it is | Open |
|---|---|---|---|
| 1 | `task.secrets_scope` | Secrets granted for the task's environments (names only) | [Markdown](https://github.com/arghya05/hobasa-data-science-agent/blob/main/outputs/FEAT-ZD-48600/task.secrets_scope.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-science-agent/main/docs/FEAT-ZD-48600/DS-02_task-secrets_scope.html) · [PDF](https://github.com/arghya05/hobasa-data-science-agent/blob/main/docs/FEAT-ZD-48600/DS-02_task-secrets_scope.pdf) |

**Code:** repository [ap-forecast-sample](https://github.com/arghya05/ap-forecast-sample) · branch [feature/story](https://github.com/arghya05/ap-forecast-sample/tree/feature/story) · commit [d67e866](https://github.com/arghya05/ap-forecast-sample/commit/d67e866)

```
no file changes in this step
```

## Checks and approval

| Attempt | Session | Problems found by the checks | Unsupported statements (judge) | Judge scores |
|---|---|---|---|---|
| 1 | new | 4 | — | — |
| 2 | continued | 1 | 1 | {'correct': 5, 'complete': 4, 'grounded': 4, 'useful': 5} |
| 3 | continued | 0 | 0 | {'correct': 5, 'complete': 5, 'grounded': 5, 'useful': 4} |

Improvements the independent judge suggests (non-blocking):

- Several citations use the form 'journey:feat.clarifications (...)' and 'journey:feat.design (...)'. The brief's allowed citation forms do not include 'journey:'. These files were read from the design agent's outputs, so they should be cited as file:hobasa-design-agent/outputs/FEAT-ZD-48600/feat.clarifications.md and file:hobasa-design-agent/outputs/FEAT-ZD-48600/feat.design.md. The content they point to (CL-DS4, CL-DS8, CL-X9, SE-3, SE-4) is accurate.
- The 'Secrets read by today's code' row cites only explain.py, models.json and test_forecast.py, but it describes a search of the whole repository. Citing ci.py, predict.py and src/ap_forecast/__init__.py as well, or naming the search scope, would make the 'whole repository' claim easier to check.
- SSQ-1 (who holds the acme-llm provider credential when a request is actually sent) is reasonable to raise as non-blocking. It could also note that feat.design's re-trigger list counts 'any storage or network call from the libraries' as a trigger, so any sender that needs a credential would sit outside this design.

## How the agent worked

- **Approach:** I followed the DS-02 procedure in the data-science-steps skill. I read feat.tasks to find the Data Science lane's tasks: TSK-03 and TSK-04, both in ap_forecast. org.permission_table is restricted, and context_get refuses it, so I took its answer from the tool view (tool:repo-access). The step hook blocks shell commands, so I checked the code by reading files. I read explain.py, predict.py, models.json, ci.py, tests/test_forecast.py, README.md and CLAUDE.md, and searched the repository for secret, key, token, credential, password and environment-variable access. As background, I read feat.design (SE-3, SE-4) and feat.clarifications (CL-X9, CL-DS4, CL-DS8). The CI facts now cite their exact sources: the command comes from README.md and CLAUDE.md, what CI does from ci.py, test credentials from test_forecast.py, and synthetic test data from CL-X9. I rewrote task.secrets_scope in full.
- **Tools used:** Skill (data-science-steps), Read, Glob, Grep, mcp__hobasa-context__context_list, mcp__hobasa-context__context_get (org.permission_table refused: restricted), Write
- **Helper subagents:** none
- **Connector calls:** none

## Files in this folder (audit trail)

- `brief_attempt<N>.md`: the exact brief the agent received
- `result_attempt<N>.json`: the agent's structured answer (approach, tools, claims with their sources, open questions)
- `checks_attempt<N>.json`: what the automated guardrail checks found
- `judge_attempt<N>.json`, `judge_brief_attempt<N>.md`: the independent judge
- `outputs/`: the outputs as approved
