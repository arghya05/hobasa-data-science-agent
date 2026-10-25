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
| 1 | new | 0 | 0 | {'correct': 5, 'complete': 5, 'grounded': 5, 'useful': 4} |

Improvements the independent judge suggests (non-blocking):

- Section 3 gives 'Granted: None' separately for local, CI, dev and main. The tool view only says 'no secrets required or granted' for the ap_forecast repository and does not break this down by environment. A one-line note that the repository-level answer was applied to every environment would make that reading explicit.
- Section 2 says the search found no environment-variable reads, API keys, tokens or passwords, but cites only explain.py and ci.py. The claim covers the whole checkout, so the citation should cite the search over the repository, including tests/. That file, tests/test_forecast.py, is also cited elsewhere but is missing from the brief's list of files the agent read. It exists and supports the claim.
- Section 1 says CI 'needs no credentials'. That is correct, but ci.py imports the third-party `coverage` package. Saying this is a package dependency and not a secret would rule out any confusion with the 'standard library only' statement.

## How the agent worked

- **Approach:** I used the data-science-steps skill (DS-02) for the procedure, then read feat.tasks through context_get (receipt b0fd4d2) to find the two Data Science tasks for ap_forecast, TSK-03 and TSK-04. org.permission_table is restricted, so I used it only through the tool:repo-access view, which says no secrets are required or granted for ap_forecast. To check that against the code, I searched the ap_forecast checkout case-insensitively for environment-variable, key, token, secret and password patterns. I also read explain.py, models.json and ci.py. The only matches were the model setting max_tokens. explain.py only builds the request dict and sends nothing, and CI runs locally with no credentials. Both the required set and the granted set are empty, so I raised no access request. A shared-memory search showed that BE-02 reached the same result for ap_aging. I did not use the verifier because the output makes few factual statements and each is cited to a file I read.
- **Tools used:** Skill:data-science-steps, mcp:hobasa-context context_get, mcp:hobasa-context memory_search, Read, Grep, Glob, Write
- **Helper subagents:** none
- **Connector calls:** none

## Files in this folder (audit trail)

- `brief_attempt<N>.md`: the exact brief the agent received
- `result_attempt<N>.json`: the agent's structured answer (approach, tools, claims with their sources, open questions)
- `checks_attempt<N>.json`: what the automated guardrail checks found
- `judge_attempt<N>.json`, `judge_brief_attempt<N>.md`: the independent judge
- `outputs/`: the outputs as approved
