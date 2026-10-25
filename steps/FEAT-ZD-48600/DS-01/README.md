# DS-01 Checking the coding prerequisites ⁶ (Data Science agent)

**Item:** FEAT-ZD-48600 · **status:** done
**Trigger:** Task creation, or any re-entry trigger ¹  
**Action:** Check for repo availability, repo creation and write access for the same  
**Outcome expected:** Repo is available with write access

## Inputs (what the agent was given)

| Item | Prepared input (this repository) | Read from (producer in GitHub) | Commit | Produced by |
|---|---|---|---|---|
| feat.tasks | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-01/feat.tasks.md | hobasa-design-agent/outputs/FEAT-ZD-48600/feat.tasks.md | b0fd4d2 | DES-04 |
| org.engineering_stack | hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-01/org.engineering_stack.md | hobasa-agentic-sdlc/org/org.engineering_stack.md | a6b3e2a | source |

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

- Approach: Attempt 3. Both earlier refusals were G2: the cited ap_forecast files counted as unread, even though attempt 2 re-read every one of them with Read. context_list and context_get showed that only the two contract items (feat.tasks, org.engineering_stack) can be fetched with a receipt; repository files can't. So instead of re-reading the files a third time, I re-fetched both inputs through context_get and rewrote task.repo_access in full. It cites only feat.tasks, org.engineering_stack, tool:repo-access, record:brief and record:memory, with no file: citations. I dropped details that only the repository files supported: the remote URL, the missing upstream, the reflog entries, COVERAGE_MIN and the coverage import. The step outcome rests entirely on the tool view and the inputs, so nothing essential was lost. I also left out a '[Tool result]' line that something outside my writes keeps appending to this file; my rewrite doesn't contain it.
- Tools used: mcp:hobasa-context (context_list, context_get), Read, Write, tool:repo-access
- Helper subagents: none
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 9 | — | — |
| 2 | resumed | 11 | — | — |
| 3 | resumed | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 4} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- The intro says 'Nothing in the repository was changed in this step', but the tool view says feature/story was 'created from dev at d67e866' (the reflog shows the Agent Spine created it). Say that the spine created the branch and the agent made no changes, so the two statements don't look contradictory.
- The last line '[Tool result] [ap_forecast] Branch feature/story at d67e866; write access confirmed' is stray text after the open-questions table. Remove it, or turn it into a cited row in section 2.
- The agent read the ap_forecast files (__init__.py, .git logs). After the previous refusal it dropped those citations. It could still note that __init__.py confirms `__version__ = "0.9.0"` and `__all__`, which backs up the version claim it takes from feat.tasks.
- RAQ-1 rightly flags that one literal `feature/story` branch doesn't match the 'feature branches per story' convention. Name an owner and a deadline (before DS-02/coding starts) so TSK-03 and TSK-04 don't both start on a branch nobody has agreed to.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.
