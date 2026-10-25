# DS-04 Identify the components/layers to be changed (Data Science agent)

**Item:** FEAT-ZD-48600 · **status:** done
**Trigger:** Previous outcome  
**Action:** Read the existing code and understand it.
Identify the components/layers that needs to be created/updated with minimum lines of code
Access for the secrets in all the environments ⁵  
**Outcome expected:** The list of components/layers needs to be created/updated with minimum lines of code

## Inputs (what the agent was given)

| Item | Prepared input (this repository) | Read from (producer in GitHub) | Commit | Produced by |
|---|---|---|---|---|
| feat.tasks | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-04/feat.tasks.md | hobasa-design-agent/outputs/FEAT-ZD-48600/feat.tasks.md | b0fd4d2 | DES-04 |
| feat.stories | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-04/feat.stories.md | hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.stories.md | 0590ea4 | REQ-08 |
| feat.prd | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-04/feat.prd.md | hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.prd.md | 908b58d | REQ-05 |
| feat.trd | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-04/feat.trd.md | hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.trd.md | 3637e51 | REQ-07 |
| feat.design | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-04/feat.design.md | hobasa-design-agent/outputs/FEAT-ZD-48600/feat.design.md | 722a98f | DES-02 |
| task.repo_access | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-04/task.repo_access.md | hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.repo_access.md | c7b4be8 | DS-01 |

Full brief per attempt: `brief_attempt<N>.md` (trigger event, owner answers, tool view, refusal reasons).

## Outputs (what the agent produced)

| Item | GitHub file |
|---|---|
| task.components | hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.components.md |

## Code

Repository `ap-forecast-sample`, branch `feature/story`, commit `d67e866`

```
no file changes in this step
```

## How the agent worked

- Approach: I followed the data-science-steps procedure for DS-04. I read all six contracted inputs: feat.tasks, feat.design, task.repo_access, and the STORY-03/04 sections of feat.stories, feat.trd and feat.prd. I also read the ap_forecast code in the step's tool view. Shell commands are blocked in this step (G9), so I listed the repository with Glob instead. From shared memory I learned that file:ap_forecast/... citations fail G2, so the code is cited only as tool:repo. I fetched DS-03's task.db_changes with context_get so it could be cited as background. I then mapped TSK-03 and TSK-04 onto the existing layers: package API, prediction rule, LLM request, config and tests. That gave five components (CMP-1 to CMP-5) inside each task's file budget, a minimum-code sketch that follows feat.design B2.2, the tests, the secrets and database position per environment, and the dependencies. A verifier subagent checked the draft against the sources. I fixed all five of its findings and its two minor items: coverage is now stated as expected rather than measured, the per-environment secrets claims are narrowed, and citations and agent-judgement labels are corrected.
- Tools used: Skill:data-science-steps, Read, Grep, Glob, mcp__hobasa-context__context_list, mcp__hobasa-context__context_get, mcp__hobasa-context__memory_search, Agent:verifier, Write, Edit
- Helper subagents: Agent
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 5} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- Section 5 row 'Per-environment secret grants … Not re-checked in this step. The DS-02 record is not among this step's inputs' and open question CQ-DS-3: the agent read hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.secrets_scope.md (DS-02). That file already answers the question: for Local/CI, `feature/<story>`, `dev` and `main`, secrets required = None, granted = None, and no access request was raised (section 2, from tool:repo-access). The workbook action explicitly includes 'Access for the secrets in all the environments', so the output should cite that record and close CQ-DS-3 rather than defer it.
- The 'journey:task.db_changes' citations (section 1 closing note; section 5 Database row) do not use the convention the brief gives for other GitHub files read (file:<repository>/<path>). Cite them as file:hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.db_changes.md. The content they support is accurate.
- 'The coding step (DS-05) writes the final code' (section 3): no input names DS-05 as the coding step. task.repo_access RAQ-2 only refers to 'the first CI run in DS-05/DS-06'. Cite RAQ-2 or soften the wording.
- Optionally give a rough line-count estimate per component, e.g. about 12 lines for risk.py, 2 lines in explain.py and 3 lines in __init__.py. The sketches already make this visible, and a figure would speak more directly to the workbook outcome's 'minimum lines of code'.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.
