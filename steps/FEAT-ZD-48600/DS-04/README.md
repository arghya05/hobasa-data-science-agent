# DS-04 · Identify the components/layers to be changed

**Agent:** Data Science · **Item:** FEAT-ZD-48600 · **Status:** ✅ done
**Approved by:** automated checks and the independent judge

**What this step does:** Read the existing code and understand it.
Identify the components/layers that needs to be created/updated with minimum lines of code
Access for the secrets in all the environments ⁵

**Expected outcome:** The list of components/layers needs to be created/updated with minimum lines of code

**Started by:** Previous outcome

## Input: what the agent received

| # | Input | What it is | Produced by | Open |
|---|---|---|---|---|
| 1 | `feat.tasks` | Engineering tasks with goal, AC, dependencies and size budget | Design agent · DES-04 | [input copy](https://github.com/arghya05/hobasa-data-science-agent/blob/main/inputs/FEAT-ZD-48600/DS-04/feat.tasks.md) · [original](https://github.com/arghya05/hobasa-design-agent/blob/main/outputs/FEAT-ZD-48600/feat.tasks.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-design-agent/main/docs/FEAT-ZD-48600/DES-04_feat-tasks.html) · [PDF](https://github.com/arghya05/hobasa-design-agent/blob/main/docs/FEAT-ZD-48600/DES-04_feat-tasks.pdf) |
| 2 | `feat.stories` | Epics, stories and tasks with numbered AC, frame links, assumptions and open items (latest revision) | Requirement agent · REQ-08 | [input copy](https://github.com/arghya05/hobasa-data-science-agent/blob/main/inputs/FEAT-ZD-48600/DS-04/feat.stories.md) · [original](https://github.com/arghya05/hobasa-requirement-agent/blob/main/outputs/FEAT-ZD-48600/feat.stories.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-requirement-agent/main/docs/FEAT-ZD-48600/REQ-08_feat-stories.html) · [PDF](https://github.com/arghya05/hobasa-requirement-agent/blob/main/docs/FEAT-ZD-48600/REQ-08_feat-stories.pdf) |
| 3 | `feat.prd` | Approved PRD | Requirement agent · REQ-05 | [input copy](https://github.com/arghya05/hobasa-data-science-agent/blob/main/inputs/FEAT-ZD-48600/DS-04/feat.prd.md) · [original](https://github.com/arghya05/hobasa-requirement-agent/blob/main/outputs/FEAT-ZD-48600/feat.prd.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-requirement-agent/main/docs/FEAT-ZD-48600/REQ-05_feat-prd.html) · [PDF](https://github.com/arghya05/hobasa-requirement-agent/blob/main/docs/FEAT-ZD-48600/REQ-05_feat-prd.pdf) |
| 4 | `feat.trd` | Approved TRD | Requirement agent · REQ-07 | [input copy](https://github.com/arghya05/hobasa-data-science-agent/blob/main/inputs/FEAT-ZD-48600/DS-04/feat.trd.md) · [original](https://github.com/arghya05/hobasa-requirement-agent/blob/main/outputs/FEAT-ZD-48600/feat.trd.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-requirement-agent/main/docs/FEAT-ZD-48600/REQ-07_feat-trd.html) · [PDF](https://github.com/arghya05/hobasa-requirement-agent/blob/main/docs/FEAT-ZD-48600/REQ-07_feat-trd.pdf) |
| 5 | `feat.design` | HLD, LLD and contracts | Design agent · DES-02 | [input copy](https://github.com/arghya05/hobasa-data-science-agent/blob/main/inputs/FEAT-ZD-48600/DS-04/feat.design.md) · [original](https://github.com/arghya05/hobasa-design-agent/blob/main/outputs/FEAT-ZD-48600/feat.design.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-design-agent/main/docs/FEAT-ZD-48600/DES-02_feat-design.html) · [PDF](https://github.com/arghya05/hobasa-design-agent/blob/main/docs/FEAT-ZD-48600/DES-02_feat-design.pdf) |
| 6 | `task.repo_access` | Repository (and Databricks) availability and write access | Data Science agent · DS-01 | [input copy](https://github.com/arghya05/hobasa-data-science-agent/blob/main/inputs/FEAT-ZD-48600/DS-04/task.repo_access.md) · [original](https://github.com/arghya05/hobasa-data-science-agent/blob/main/outputs/FEAT-ZD-48600/task.repo_access.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-science-agent/main/docs/FEAT-ZD-48600/DS-01_task-repo_access.html) · [PDF](https://github.com/arghya05/hobasa-data-science-agent/blob/main/docs/FEAT-ZD-48600/DS-01_task-repo_access.pdf) |

The full brief the agent was given, attempt by attempt: `brief_attempt<N>.md` in this folder.

## Output: what the agent produced

| # | Output | What it is | Open |
|---|---|---|---|
| 1 | `task.components` | Components, layers, sources or pipelines to change | [Markdown](https://github.com/arghya05/hobasa-data-science-agent/blob/main/outputs/FEAT-ZD-48600/task.components.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-science-agent/main/docs/FEAT-ZD-48600/DS-04_task-components.html) · [PDF](https://github.com/arghya05/hobasa-data-science-agent/blob/main/docs/FEAT-ZD-48600/DS-04_task-components.pdf) |

**Code:** repository [ap-forecast-sample](https://github.com/arghya05/ap-forecast-sample) · branch [feature/story](https://github.com/arghya05/ap-forecast-sample/tree/feature/story) · commit [d67e866](https://github.com/arghya05/ap-forecast-sample/commit/d67e866)

```
no file changes in this step
```

## Checks and approval

| Attempt | Session | Problems found by the checks | Unsupported statements (judge) | Judge scores |
|---|---|---|---|---|
| 1 | new | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 5} |

Improvements the independent judge suggests (non-blocking):

- The blast-radius statement cites mcp:codegraph, but the step left no saved codegraph response, so the judge cannot check that the call happened. The finding itself (the only callers are the `__init__.py` re-export and `tests/test_forecast.py`) matches tool:repo. Cite tool:repo alongside it, or save the connector response.
- Section 8 says the carried RAQ-1 to RAQ-3 and SSQ-1/SSQ-2 do not block coding and cites journey:feat.clarifications section 10. That section covers only the story-level pending items. The non-blocking status of the RAQ and SSQ items comes from task.repo_access section 3 and task.secrets_scope section 3, so cite those as well.
- The workbook action includes 'Access for the secrets in all the environments'. The output carries the per-environment confirmation forward as non-blocking SSQ-2, which DS-02 had already accepted. It should name the environments involved (or record that no input names them) and say who confirms each one, so the person at the gate can see this part of the action is still open and not finished.
- DSQ-1 (the version number replacing 0.9.0) is fine as a non-blocking item, because feat.design DD-11 and feat.tasks section 2 leave the number 'for the lane to confirm'. It would read better to say that explicitly, so it does not look like an unasked default.
- The 'minimum lines of code' outcome is met in qualitative terms (sketches, 'only the signature line and one conditional'). A rough per-component estimate of added or changed lines, marked as agent judgement, would help DS-05 and the gate check the minimality claim.

## How the agent worked

- **Approach:** I followed DS-04 in the data-science-steps skill. I read all six contracted inputs. For feat.stories, feat.prd and feat.trd I read only the Data Science parts (STORY-03 and STORY-04, AC-7 to AC-12, IF-3 and IF-4, T-5 to T-7). I also read the approved background outputs with context_get: task.secrets_scope (DS-02), task.db_changes (DS-03) and feat.clarifications (DES-03). Then I read the existing ap_forecast code: the tool view, ci.py and README.md. I used CodeGraph to check which callers depend on build_explanation_request, predict_delay_days and load_config. I mapped TSK-03 and TSK-04 onto the smallest set of files allowed by each task's file budget and wrote a minimal code sketch for each component. I listed the layers that do not change: predict.py, models.json, CI, storage, secrets and the other repositories. A verifier subagent checked the draft against the cited sources. I then fixed what it found: the per-environment secrets wording (SSQ-2 is still open), wrong section references for TA-3 and TA-4, the approval status of feat.tasks (cited from the journey board, with TKQ-2 still open), two inferences now labelled as agent judgement, and the diagram sources.
- **Tools used:** Skill (data-science-steps), Read, Grep, Glob, mcp:hobasa-context (context_list, context_get), mcp:codegraph (codegraph_explore), Agent (verifier), Write, Edit
- **Helper subagents:** Agent
- **Connector calls:** none

## Files in this folder (audit trail)

- `brief_attempt<N>.md`: the exact brief the agent received
- `result_attempt<N>.json`: the agent's structured answer (approach, tools, claims with their sources, open questions)
- `checks_attempt<N>.json`: what the automated guardrail checks found
- `judge_attempt<N>.json`, `judge_brief_attempt<N>.md`: the independent judge
- `outputs/`: the outputs as approved
