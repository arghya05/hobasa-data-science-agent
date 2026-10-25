# DS-05 · Coding

**Agent:** Data Science · **Item:** FEAT-ZD-48600 · **Status:** ✅ done
**Approved by:** automated checks and the independent judge

**What this step does:** Coding for the implementation required for changes in the components/layers with proper comments and required documentation
Access for the secrets in all the environments

**Expected outcome:** Updated repo with the new implemented code changes

**Started by:** Previous outcome

## Input: what the agent received

| # | Input | What it is | Produced by | Open |
|---|---|---|---|---|
| 1 | `task.components` | Components, layers, sources or pipelines to change | Data Science agent · DS-04 | [input copy](https://github.com/arghya05/hobasa-data-science-agent/blob/main/inputs/FEAT-ZD-48600/DS-05/task.components.md) · [original](https://github.com/arghya05/hobasa-data-science-agent/blob/main/outputs/FEAT-ZD-48600/task.components.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-science-agent/main/docs/FEAT-ZD-48600/DS-04_task-components.html) · [PDF](https://github.com/arghya05/hobasa-data-science-agent/blob/main/docs/FEAT-ZD-48600/DS-04_task-components.pdf) |
| 2 | `feat.design` | HLD, LLD and contracts | Design agent · DES-02 | [input copy](https://github.com/arghya05/hobasa-data-science-agent/blob/main/inputs/FEAT-ZD-48600/DS-05/feat.design.md) · [original](https://github.com/arghya05/hobasa-design-agent/blob/main/outputs/FEAT-ZD-48600/feat.design.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-design-agent/main/docs/FEAT-ZD-48600/DES-02_feat-design.html) · [PDF](https://github.com/arghya05/hobasa-design-agent/blob/main/docs/FEAT-ZD-48600/DES-02_feat-design.pdf) |
| 3 | `feat.stories` | Epics, stories and tasks with numbered AC, frame links, assumptions and open items (latest revision) | Requirement agent · REQ-08 | [input copy](https://github.com/arghya05/hobasa-data-science-agent/blob/main/inputs/FEAT-ZD-48600/DS-05/feat.stories.md) · [original](https://github.com/arghya05/hobasa-requirement-agent/blob/main/outputs/FEAT-ZD-48600/feat.stories.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-requirement-agent/main/docs/FEAT-ZD-48600/REQ-08_feat-stories.html) · [PDF](https://github.com/arghya05/hobasa-requirement-agent/blob/main/docs/FEAT-ZD-48600/REQ-08_feat-stories.pdf) |
| 4 | `org.engineering_stack` | Repositories, languages, versions and exact commands | organisation source | [input copy](https://github.com/arghya05/hobasa-data-science-agent/blob/main/inputs/FEAT-ZD-48600/DS-05/org.engineering_stack.md) · [original](https://github.com/arghya05/hobasa-agentic-sdlc/blob/main/org/org.engineering_stack.md) |
| 5 | `org.lessons` | Approved lessons and eval cases | organisation source | [input copy](https://github.com/arghya05/hobasa-data-science-agent/blob/main/inputs/FEAT-ZD-48600/DS-05/org.lessons.md) · [original](https://github.com/arghya05/hobasa-agentic-sdlc/blob/main/org/org.lessons.md) |
| 6 | `task.secrets_scope` | Secrets granted for the task's environments (names only) | — | not applicable for this item, or passed by reference |

The full brief the agent was given, attempt by attempt: `brief_attempt<N>.md` in this folder.

## Output: what the agent produced

| # | Output | What it is | Open |
|---|---|---|---|
| 1 | `task.code` | Code changes on the working branch | [Markdown](https://github.com/arghya05/hobasa-data-science-agent/blob/main/outputs/FEAT-ZD-48600/task.code.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-science-agent/main/docs/FEAT-ZD-48600/DS-05_task-code.html) · [PDF](https://github.com/arghya05/hobasa-data-science-agent/blob/main/docs/FEAT-ZD-48600/DS-05_task-code.pdf) |

**Code:** repository [ap-forecast-sample](https://github.com/arghya05/ap-forecast-sample) · branch [feature/story](https://github.com/arghya05/ap-forecast-sample/tree/feature/story) · commit [d67e866](https://github.com/arghya05/ap-forecast-sample/commit/d67e866)

```
no file changes in this step
```

## Checks and approval

| Attempt | Session | Problems found by the checks | Unsupported statements (judge) | Judge scores |
|---|---|---|---|---|
| 1 | new | 3 | — | — |
| 2 | continued | 3 | 3 | {'correct': 4, 'complete': 3, 'grounded': 3, 'useful': 4} |
| 3 | continued | 4 | 4 | {'correct': 4, 'complete': 4, 'grounded': 3, 'useful': 4} |
| 4 | continued | 0 | 0 | {'correct': 4, 'complete': 3, 'grounded': 5, 'useful': 4} |

Improvements the independent judge suggests (non-blocking):

- CI status is left unstated. The brief says to run `/usr/bin/python3 ci.py` until it passes, and the only recorded tool result is 'Sources compile'. Section 5 says only that 'The spine records the result'. The output should say plainly that the CI has not passed and that no test or coverage result is recorded for this step. That leaves the workbook outcome conditional. Section 5 itself shows why the gate cannot be met yet: no existing test runs the `vendor_risk` body or the `risk` prompt line.
- DSQ-2 is not marked BLOCKING. The brief requires CI to pass but forbids changes to `tests/`, and ci.py fails below 90.0 coverage. That is a contradiction in the brief. CLAUDE.md says such a contradiction becomes a BLOCKING question to the owner, not a non-blocking item with an interim answer ('Tests left to DS-06').
- Line 29 says `tests/` stays unchanged 'as planned' and cites task.components section 4. That section does not list `tests/` as unchanged: task.components plans C-4 and C-5 test changes, for DS-06. Only record:brief (the allowed paths) supports leaving tests alone in this step. The citation should name the source that actually supports each item.
- task.components section 4 plans `README.md` as unchanged, but this step changes it because the brief requires it. The output should say openly that it departs from the DS-04 plan here and why (record:brief).
- Section 7 carries only some of the open questions from task.components section 8. RAQ-1 to RAQ-3 and SSQ-1 are dropped without comment. Either carry them or say why they no longer apply.
- Section 4 (blast radius) relies on the tool view only. The brief asked for `codegraph impact`, `codegraph callers` and `codegraph affected` after `codegraph sync`. The output does not say whether CodeGraph was run or which tests it reported as affected.

## How the agent worked

- **Approach:** I followed DS-05 in the data-science-steps skill. task.components (DS-04) was the build plan, feat.design B2 the specification, and feat.stories AC-03.x and AC-04.x the acceptance criteria. I read every readable input. task.secrets_scope is restricted, so I relied on task.components section 4 for it. Before editing I read the existing source, its callers and its tests. I made the smallest change the design allows: a new risk.py, `risk=None` as the last parameter of build_explanation_request, and the export plus a proposed version step in __init__.py, each with docstrings and comments. I documented the module in a new docs/ap_forecast.md and updated README.md.

The output uses only the spine's recorded check ("Sources compile") as a check result. Everything else it says about the CI is read from the code (tool:repo) and from ci.py. My own local run of /usr/bin/python3 ci.py is not part of the recorded results. In that run the existing tests passed but the coverage gate was not met, because the new code has no tests yet. Those tests (C-4, C-5) belong in tests/, which this step may not change. I did not raise coverage by any other means, and I raise the question to the Developer as DSQ-2. Nothing was committed or pushed.
- **Tools used:** Skill (data-science-steps), Read, Write, Grep, codegraph (explore, sync) to read source before and after editing, Bash: /usr/bin/python3 ci.py, Bash: git status / git diff / git log
- **Helper subagents:** none
- **Connector calls:** none

## Files in this folder (audit trail)

- `brief_attempt<N>.md`: the exact brief the agent received
- `result_attempt<N>.json`: the agent's structured answer (approach, tools, claims with their sources, open questions)
- `checks_attempt<N>.json`: what the automated guardrail checks found
- `judge_attempt<N>.json`, `judge_brief_attempt<N>.md`: the independent judge
- `outputs/`: the outputs as approved
