# DS-05 Coding (Data Science agent)

**Item:** FEAT-ZD-48600 · **status:** done
**Trigger:** Previous outcome  
**Action:** Coding for the implementation required for changes in the components/layers with proper comments and required documentation
Access for the secrets in all the environments  
**Outcome expected:** Updated repo with the new implemented code changes

## Inputs (what the agent was given)

| Item | Prepared input (this repository) | Read from (producer in GitHub) | Commit | Produced by |
|---|---|---|---|---|
| task.components | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/task.components.md | hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.components.md | a126bc4 | DS-04 |
| feat.design | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/feat.design.md | hobasa-design-agent/outputs/FEAT-ZD-48600/feat.design.md | 722a98f | DES-02 |
| feat.stories | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/feat.stories.md | hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.stories.md | 0590ea4 | REQ-08 |
| org.engineering_stack | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/org.engineering_stack.md | hobasa-agentic-sdlc/org/org.engineering_stack.md | a6b3e2a | source |
| org.lessons | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/org.lessons.md | hobasa-agentic-sdlc/org/org.lessons.md | a6b3e2a | source |
| task.secrets_scope | not applicable or by reference | — | — | — |

Full brief per attempt: `brief_attempt<N>.md` (trigger event, owner answers, tool view, refusal reasons).

## Outputs (what the agent produced)

| Item | GitHub file |
|---|---|
| task.code | hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.code.md |

## Code

Repository `ap-forecast-sample`, branch `feature/story`, commit `d67e866`

```
no file changes in this step
```

## How the agent worked

- Approach: I fixed the single G1 point by removing the unsupported statement about this step's tool permissions. Section 3 of task.code.md now says only what the inputs support: the tests planned for the new criteria (CMP-4, CMP-5) are in task.components, and they go under tests/, outside this step's write scope (record:brief). I also dropped the other statements that named a later step's duty without a source, and tied the coverage follow-up to task.components section 4, which leaves the confirming CI run to "DS-05/DS-06". The code and docs are unchanged and already match feat.design B2.1/B2.2 and the tool view. I re-ran /usr/bin/python3 ci.py and git status to confirm the state, then rewrote task.code.md in full. Figures that come only from the CI run appear only in its verbatim output.
- Tools used: Write, Bash (/usr/bin/python3 ci.py, git status)
- Helper subagents: none
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 4 | — | — |
| 2 | resumed | 1 | — | — |
| 3 | resumed | 1 | 1 | {'correct': 5, 'complete': 4, 'grounded': 4, 'useful': 4} |
| 4 | resumed | 0 | 0 | {'correct': 5, 'complete': 3, 'grounded': 5, 'useful': 4} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- CI does not pass. The coverage gate fails: 83.3 against the 90% minimum in file:ap_forecast/ci.py (COVERAGE_MIN = 90.0). The brief says to run CI 'until it passes', and the workbook outcome is an updated repo. The write scope excludes tests/, so this step cannot meet the gate. That conflict is filed as CQ-C1, a non-blocking question. It should be raised to the spine or owner as a blocking or escalated item, with a request to widen the scope to tests/ or move the gate to DS-06. Otherwise DS-05 is handed off with a red CI.
- The output file ends with a stray line, '[Tool result] [ap_forecast] Sources compile' (line 182). It belongs to neither section 8 nor any other section and should be removed.
- Sections 4 and 8 cite 'CI run, section 4' as a source. Cite file:ap_forecast/ci.py (gate, static checks) together with the CI output the step recorded, so the coverage, gate and static-check claims trace to a readable source.
- Section 4 says the CI was 're-run in this attempt; code unchanged since the first attempt', but nothing in the output proves it. Add the run's timestamp or receipt, or drop the qualifier.
- In docs/ap_forecast.md, the risk_by_vendor example uses `delays`, which the snippet never defines. Add a one-line comment saying where it comes from (ap_pipeline.delays.delays_by_vendor(records)).
- Secrets per environment still rest on CQ-DS-3. DS-02's approved task.secrets_scope (journey:task.secrets_scope) could be cited for the per-environment grants, instead of carrying the question forward.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.
