# DS-01 · Checking the coding prerequisites ⁶

**Agent:** Data Science · **Item:** FEAT-ZD-48600 · **Status:** ✅ done
**Approved by:** automated checks and the independent judge

**What this step does:** Check for repo availability, repo creation and write access for the same

**Expected outcome:** Repo is available with write access

**Started by:** Task creation, or any re-entry trigger ¹

## Input: what the agent received

| # | Input | What it is | Produced by | Open |
|---|---|---|---|---|
| 1 | `feat.tasks` | Engineering tasks with goal, AC, dependencies and size budget | Design agent · DES-04 | [input copy](https://github.com/arghya05/hobasa-data-science-agent/blob/main/inputs/FEAT-ZD-48600/DS-01/feat.tasks.md) · [original](https://github.com/arghya05/hobasa-design-agent/blob/main/outputs/FEAT-ZD-48600/feat.tasks.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-design-agent/main/docs/FEAT-ZD-48600/DES-04_feat-tasks.html) · [PDF](https://github.com/arghya05/hobasa-design-agent/blob/main/docs/FEAT-ZD-48600/DES-04_feat-tasks.pdf) |
| 2 | `org.engineering_stack` | Repositories, languages, versions and exact commands | organisation source | [input copy](https://github.com/arghya05/hobasa-data-science-agent/blob/main/inputs/FEAT-ZD-48600/DS-01/org.engineering_stack.md) · [original](https://github.com/arghya05/hobasa-agentic-sdlc/blob/main/org/org.engineering_stack.md) |

The full brief the agent was given, attempt by attempt: `brief_attempt<N>.md` in this folder.

## Output: what the agent produced

| # | Output | What it is | Open |
|---|---|---|---|
| 1 | `task.repo_access` | Repository (and Databricks) availability and write access | [Markdown](https://github.com/arghya05/hobasa-data-science-agent/blob/main/outputs/FEAT-ZD-48600/task.repo_access.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-science-agent/main/docs/FEAT-ZD-48600/DS-01_task-repo_access.html) · [PDF](https://github.com/arghya05/hobasa-data-science-agent/blob/main/docs/FEAT-ZD-48600/DS-01_task-repo_access.pdf) |

**Code:** repository [ap-forecast-sample](https://github.com/arghya05/ap-forecast-sample) · branch [feature/story](https://github.com/arghya05/ap-forecast-sample/tree/feature/story) · commit [d67e866](https://github.com/arghya05/ap-forecast-sample/commit/d67e866)

```
no file changes in this step
```

## Checks and approval

| Attempt | Session | Problems found by the checks | Unsupported statements (judge) | Judge scores |
|---|---|---|---|---|
| 1 | new | 1 | 1 | {'correct': 5, 'complete': 5, 'grounded': 4, 'useful': 5} |
| 2 | continued | 0 | 0 | {'correct': 5, 'complete': 5, 'grounded': 5, 'useful': 5} |

Improvements the independent judge suggests (non-blocking):

- Section 1, 'Repository exists' row: the file list includes `models.json`, but none of the cited sources (tool:repo-access, __init__.py, tests/test_forecast.py, ci.py) mentions it. It is true on disk, and feat.tasks TSK-03 names it, so add feat.tasks (TSK-03) to that row's citations.
- Section 1, 'Branches' row: the claim that `main` and `dev` exist cites only `.git/HEAD` and the tool view. Cite `.git/refs/heads/main` and `.git/refs/heads/dev` here as well; the next row already cites them.
- RAQ-2 (the `coverage` package is not part of the standard library) is a real readiness risk for the CI command. Consider naming the Developer or infra owner who must confirm it before DS-05 Coding, rather than relying on the first CI run.
- RAQ-1 interim answer (keep the single `feature/story` branch for both STORY-03 and STORY-04) is a reasonable proposal. Make clear that the Developer must confirm it, because org.engineering_stack says 'feature branches per story'.

## How the agent worked

- **Approach:** I read the two contracted inputs, feat.tasks and org.engineering_stack, along with the DS-01 procedure from the data-science-steps skill. feat.tasks shows the Data Science lane's tasks, TSK-03 and TSK-04, change only ap_forecast. This step allows no shell commands, so I checked the repository through file reads instead of git commands. I read .git/HEAD, .git/config, the dev and main refs, src/ap_forecast/__init__.py, ci.py, tests/test_forecast.py and the repository's CLAUDE.md. I compared what I found with the tool view (tool:repo-access), which states write access and a clean git status. I removed the 'Agent actions' row because the brief does not support it. Every remaining statement cites an input, the tool view, record:brief, record:memory or a file in the repository. No connector was needed.
- **Tools used:** Skill:data-science-steps, Read, Glob, Write, tool:repo-access
- **Helper subagents:** none
- **Connector calls:** none

## Files in this folder (audit trail)

- `brief_attempt<N>.md`: the exact brief the agent received
- `result_attempt<N>.json`: the agent's structured answer (approach, tools, claims with their sources, open questions)
- `checks_attempt<N>.json`: what the automated guardrail checks found
- `judge_attempt<N>.json`, `judge_brief_attempt<N>.md`: the independent judge
- `outputs/`: the outputs as approved
