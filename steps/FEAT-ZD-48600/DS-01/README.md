# DS-01 Checking the coding prerequisites ⁶ (Data Science agent)

**Item:** FEAT-ZD-48600 · **status:** done
**Trigger:** Task creation, or any re-entry trigger ¹  
**Action:** Check for repo availability, repo creation and write access for the same  
**Outcome expected:** Repo is available with write access

## Inputs (what the agent was given)

| Item | Prepared input (this repository) | Read from (producer in GitHub) | Commit | Produced by |
|---|---|---|---|---|
| feat.tasks | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-01/feat.tasks.md | hobasa-design-agent/outputs/FEAT-ZD-48600/feat.tasks.md | b0fd4d2 | DES-04 |
| org.engineering_stack | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-01/org.engineering_stack.md | hobasa-agentic-sdlc/org/org.engineering_stack.md | 3b03f15 | source |

Full brief per attempt: `brief_attempt<N>.md` (trigger event, owner answers, tool view, refusal reasons).

## Outputs (what the agent produced)

| Item | GitHub file |
|---|---|
| task.repo_access | hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.repo_access.md |

## Code

Repository `ap-forecast-sample`, branch `feature/story`, commit `d67e866`

```
no file changes in this step
```

## How the agent worked

- Approach: I followed the DS-01 procedure in the data-science-steps skill. From feat.tasks I took the Data Science lane's tasks (TSK-03 and TSK-04, both in ap_forecast). From org.engineering_stack I took the language, CI command, coverage bar and branch convention, and the statement that no repository uses Databricks or a database. The step's tool policy allows no shell commands. So availability, the branch's origin and write access come from the spine's tool:repo-access view. I checked them against files read in the checkout: .git/HEAD and each branch ref file (main, dev, feature/story), the repository's CLAUDE.md, __init__.py, predict.py, explain.py, tests/test_forecast.py and ci.py. Every file citation in the output names a file that was actually read.
- Tools used: Skill:data-science-steps, Read, Glob, Grep, Write
- Helper subagents: none
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 1 | — | — |
| 2 | resumed | 0 | 0 | {'correct': 5, 'complete': 5, 'grounded': 5, 'useful': 5} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- RAQ-1's interim answer names 'DS-05/DS-06' as the steps where CI first runs. No cited input names these step IDs. It sits in an open question, so it does not block, but it would be cleaner to say 'the first CI run in the coding step' or to cite the step list.
- Section 4 item 2 says 'DS-01 may not run shell commands under its tool policy'. That is true: the spine hook G9 enforces it. But the output cites no source for it. Cite record:brief or the tool policy, or drop the clause.
- The file ends with a stray line, '[Tool result] [ap_forecast] Branch feature/story at d67e866; write access confirmed'. It matches tool:repo-access, but it reads as pasted tool output rather than part of the document. Remove it or fold it into section 2.
- The 'Repository creation needed: No' row says the repository already exists. That is inferred from tool:repo-access ('available locally'), which is fine, but it could say outright that the existing remote is what the feature branch will push to.
- TSK-03 says risk.py is new, and the output takes this from feat.tasks. It could also confirm from the checkout that src/ap_forecast/risk.py does not exist yet.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.
