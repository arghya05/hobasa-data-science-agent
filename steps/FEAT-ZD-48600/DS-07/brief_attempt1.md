# Step DS-07: Commit & Push (attempt 1) for FEAT-ZD-48600

- Workbook trigger: Unit Testing Completion
- Workbook action: Commit the code to the local repository with proper comment for back tracing into the user story.

If any merge conflicts at times and needs to be manually reviewed and resolve

Push the code to central/main repository
Access for the secrets in all the environments
- Workbook outcome: Code Push completed and PR is being raised
- Owner of this step: Developer
- Gate (who approves this output): no gate: automated checks, the independent judge and sampled human evaluation

## Inputs (GitHub checkout; read them with Read, or context_get for a receipt)
- task.code (Code changes on the working branch): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-07/task.code.md` (from hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.code.md at commit 286af9a)
- task.unit_tests (Unit tests and coverage (≥90%)): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-07/task.unit_tests.md` (from hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.unit_tests.md at commit 79527ef)
- feat.stories (Epics, stories and tasks with numbered AC, frame links, assumptions and open items (latest revision)): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-07/feat.stories.md` (from hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.stories.md at commit 0590ea4)

## Outputs (write each file in full; the spine commits and pushes after evaluation and approval)
- task.commit (Commit traced to the story; CI result): write `hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.commit.md`


## How to work
Decide the best way to do this step from the inputs and the workbook action, plan it (TodoWrite for multi-part work), then do it. Helper subagents you can delegate to: explorer, verifier (use the verifier before you finish when the output makes many factual statements). Use the shared memory (memory_search) for decisions and lessons. Connectors approved for you: api:osv (Known vulnerabilities for a dependency version (POST /v1/query with {"package":{"name":...,"ecosystem":"PyPI|npm"},"version":...})), api:pypi (Python package metadata: latest version, licence, release dates (GET /pypi/<package>/json)). Use one only when your inputs do not hold what you need; request a missing one with capability_request. You may also read any other file in the GitHub checkouts your step needs (for example other requirements when grouping, or code); cite it as file:<repository>/<path>.

Finish with the structured result: approach, tools_used, claims (each citing an input item name, a tool view, api:<name>, mcp:<server>, file:<repository>/<path> for any other GitHub file you read, record:intake, record:owner_answer, record:memory or record:brief for this brief; a claim drawing on several sources lists them separated by ';'; every number must appear in what you cite), open questions (BLOCKING: only when you cannot finish), summary.