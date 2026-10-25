# DS-02 Access to secrets (Data Science agent)

**Item:** FEAT-ZD-48600 · **status:** done
**Trigger:** Task creation, or any re-entry trigger ¹  
**Action:** Check the access for required secrets and ask for access as needed  
**Outcome expected:** Secrets are available with access permissions

## Inputs (what the agent was given)

| Item | Prepared input (this repository) | Read from (producer in GitHub) | Commit | Produced by |
|---|---|---|---|---|
| feat.tasks | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-02/feat.tasks.md | hobasa-design-agent/outputs/FEAT-ZD-48600/feat.tasks.md | b0fd4d2 | DES-04 |
| org.permission_table | not applicable or by reference | — | — | — |

Full brief per attempt: `brief_attempt<N>.md` (trigger event, owner answers, tool view, refusal reasons).

## Outputs (what the agent produced)

| Item | GitHub file |
|---|---|
| task.secrets_scope | hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.secrets_scope.md |

## Code

Repository `ap-forecast-sample`, branch `feature/story`, commit `d67e866`

```
no file changes in this step
```

## How the agent worked

- Approach: I read feat.tasks to find this lane's tasks, TSK-03 and TSK-04 in ap_forecast. org.permission_table is restricted, so I used it only through the tool view (tool:repo-access), which says no secrets are required or granted for ap_forecast. To check that nothing needs a credential, I read every source file in ap_forecast (predict.py, explain.py, models.json, __init__.py), the existing test file, ci.py and the repository's CLAUDE.md. CLAUDE.md gives the CI command and the branch roles behind the environment table. None of these files reads a credential or an environment variable, and none sends a network request. I used no connector, because the lane adds no dependencies.
- Tools used: Skill:data-science-steps, Read, Grep, Glob, Write
- Helper subagents: none
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 1 | 1 | {'correct': 5, 'complete': 4, 'grounded': 4, 'useful': 4} |
| 2 | resumed | 1 | — | — |
| 3 | resumed | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 4} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- Section 3 uses branches (`feature/<story>`, `dev`, `main`) and CI as its 'environments'. No source defines deployment environments for ap_forecast. Say that this mapping is an assumption, or name the actual working branch `feature/story` (from `dev` at `d67e866`, per task.repo_access.md), so the scope lines up with what DS-01 established.
- Section 4 says the component that sends the request to `acme-llm` 'will need a provider credential'. No source states this; it is an inference. Mark it as an assumption in that bullet, not only through SSQ-1.
- The 'Permission source' row could note that the step saw only the tool-view summary of the restricted org.permission_table and not the table itself. That makes the limit of the evidence explicit for the sampled human reviewer.
- The trailing '[Tool result] ...' line at the end of the file looks like an appended tool echo, not authored content. Confirm it belongs in the published output.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.
