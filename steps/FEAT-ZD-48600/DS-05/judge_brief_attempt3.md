# Evaluate step DS-05: Coding for FEAT-ZD-48600

- Workbook action: Coding for the implementation required for changes in the components/layers with proper comments and required documentation
Access for the secrets in all the environments
- Workbook outcome: Updated repo with the new implemented code changes

## Outputs to evaluate
- `hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.code.md`

## Inputs the agent was given
- task.components: `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/task.components.md`
- feat.design: `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/feat.design.md`
- feat.stories: `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/feat.stories.md`
- org.engineering_stack: `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/org.engineering_stack.md`
- org.lessons: `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/org.lessons.md`

## Tool results the spine recorded from the real tools in this step (a valid source; the section 'Tool results' at the end of an output file is written by the spine, not the agent)


[Tool result] [ap_forecast] Sources compile

## Other GitHub files the agent read (valid sources when cited as file:<repository>/<path>)
- `~/.claude/projects/-Users-arghyamukherjee-Documents-automaiton-final-document-e2e-run-runs-run-20261025-205213-feat-zd-48600-workspace-ap-forecast/1f7c454c-9741-4ba2-843d-d23b16565f86/tool-results/hook-622a73c7-7894-4a31-a80c-9ab33d3fddf6-1-additionalContext.txt`
- `hobasa-data-science-agent/.claude/skills/data-science-steps/SKILL.md`
- `hobasa-data-science-agent/CLAUDE.md`
- `hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.code.md`
- `ap-aging-sample/CLAUDE.md`
- `ap-forecast-sample/CLAUDE.md`
- `ap-forecast-sample/README.md`
- `ap-forecast-sample/ci.py`
- `ap-pipeline-sample/CLAUDE.md`
- `ap-portal-sample/CLAUDE.md`
- `hobasa-agentic-sdlc/CLAUDE.md`
- `hobasa-agentic-sdlc/features/FEAT-ZD-48600/JOURNEY.md`
- `hobasa-agentic-sdlc/memory/MEMORY.md`
- `final_document/hobasa-claude-code-plugin/skills/data-science-steps/SKILL.md`
- `final_document/hobasa-claude-code-plugin/skills/grounded-output/SKILL.md`

Shared memory and conventions: `memory/MEMORY.md` and `CLAUDE.md` in the SDLC repository.

## The step brief the agent was given (a valid source, cited as record:brief)

Your previous result for this step was refused. Fix every point, rewrite the output files in full, and return the structured result again. The outputs describe the work itself: never mention earlier attempts, refusals or this review in them.

## Refusal reasons
G1 judge found an unsupported statement: "| `build_explanation_request` | `src/ap_forecast/__init__.py` | `tests/test_forecast.py` | mcp:codegraph | ... | `vendor_risk` (new) | `src/ap_forecast/__init__" (The section 4 blast-radius table cites mcp:codegraph and quotes a CodeGraph result word for word ("no covering tests found"). The spine recorded only one tool r)
G1 judge found an unsupported statement: "`codegraph affected` on the three changed source files reported no affected test files." (This states the output of a shell command, but no recorded tool view or file contains it. The spine's tool results hold only 'Sources compile'.)
G1 judge found an unsupported statement: "The full current source of the three changed modules is in the tool view (tool:repo) and in mcp:codegraph. / No existing test calls `vendor_risk` or passes `ris" (mcp:codegraph is cited again, with no recorded CodeGraph response in this step. tool:repo alone supports both statements, so the mcp:codegraph part of the citat)

## Step brief (unchanged except the attempt number)
# Step DS-05: Coding (attempt 3) for FEAT-ZD-48600

- Workbook trigger: Previous outcome
- Workbook action: Coding for the implementation required for changes in the components/layers with proper comments and required documentation
Access for the secrets in all the environments
- Workbook outcome: Updated repo with the new implemented code changes
- Owner of this step: Developer
- Gate (who approves this output): no gate: automated checks, the independent judge and sampled human evaluation

## Inputs (GitHub checkout; read them with Read, or context_get for a receipt)
- task.components (Components, layers, sources or pipelines to change): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/task.components.md` (from hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.components.md at commit 7cb32bd)
- feat.design (HLD, LLD and contracts): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/feat.design.md` (from hobasa-design-agent/outputs/FEAT-ZD-48600/feat.design.md at commit 722a98f)
- feat.stories (Epics, stories and tasks with numbered AC, frame links, assumptions and open items (latest revision)): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/feat.stories.md` (from hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.stories.md at commit 0590ea4)
- org.engineering_stack (Repositories, languages, versions and exact commands): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/org.engineering_stack.md` (from hobasa-agentic-sdlc/org/org.engineering_stack.md at commit e8afce8)
- org.lessons (Approved lessons and eval cases): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/org.lessons.md` (from hobasa-agentic-sdlc/org/org.lessons.md at commit e8afce8)
- task.secrets_scope (Secrets granted for the task's environments (names only)): restricted, passed by reference only

## Outputs (write each file in full; the spine commits and pushes after evaluation and approval)
- task.code (Code changes on the working branch): write `hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.code.md`

## Code
Work in repository `ap-forecast-sample` (Python 3.9 (standard library)). You may change only: src/ap_forecast/, docs/, README.md. Understand before you change: the repository is indexed with CodeGraph. Before editing, use it (MCP tools codegraph_explore and codegraph_node with projectPath `ap-forecast-sample`, or `codegraph explore <symbols>`, `codegraph impact <symbol>`, `codegraph callers <symbol>`) to read the exact source of what you change, its callers and callees and the blast radius. After editing, run `codegraph sync` and `codegraph affected <changed files>` to find the tests your change touches, and run them through the CI. Shell commands allowed in this step: `/usr/bin/python3 ci.py`, `git status`, `git diff`, `git log`, `codegraph explore`, `codegraph node`, `codegraph callers`, `codegraph callees`, `codegraph impact`, `codegraph affected`, `codegraph query`, `codegraph files`, `codegraph status`, `codegraph sync`. Run the CI yourself with `/usr/bin/python3 ci.py` from the repository root (or delegate to the ci-runner subagent) until it passes. Do not commit or push; the spine does. Document what you change: update the repository's README.md and docs/ (module overview, new functions with signatures, inputs, outputs and examples).

## How to work
Decide the best way to do this step from the inputs and the workbook action, plan it (TodoWrite for multi-part work), then do it. Helper subagents you can delegate to: explorer, verifier, ci-runner (use the verifier before you finish when the output makes many factual statements). Use the shared memory (memory_search) for decisions and lessons. Connectors approved for you: api:osv (Known vulnerabilities for a dependency version (POST /v1/query with {"package":{"name":...,"ecosystem":"PyPI|npm"},"version":...})), api:pypi (Python package metadata: latest version, licence, release dates (GET /pypi/<package>/json)), mcp:codegraph (Code intelligence over the repository: a symbol's source, its callers and callees, the blast radius of a change and the tests it affects (codegraph_explore, codegraph_node and related tools; pass projectPath = the repository checkout)). Use one only when your inputs do not hold what you need; request a missing one with capability_request. You may also read any other file in the GitHub checkouts your step needs (for example other requirements when grouping, or code); cite it as file:<repository>/<path>.

## Tool view (cite as tool:repo)
--- ap_forecast/src/ap_forecast/__init__.py ---
"""AP forecast: predicted payment delay, a rule-based vendor risk level and an LLM explanation request."""
from .predict import predict_delay_days
from .explain import build_explanation_request
from .risk import vendor_risk

__version__ = "0.10.0"
__all__ = ["predict_delay_days", "build_explanation_request", "vendor_risk"]

--- ap_forecast/src/ap_forecast/explain.py ---
"""Build the LLM request that explains a prediction to the AP user. The provider and model come from models.json."""
import json
from pathlib import Path

CONFIG = Path(__file__).with_name("models.json")


def load_config():
    return json.loads(CONFIG.read_text())


def build_explanation_request(vendor: str, predicted_days: int, config=None, risk=None) -> dict:
    """Request dict for the configured provider. risk (STORY-04) is optional and last, so existing calls are unchanged.

    When risk is not None the prompt ends with ' Risk level: <risk>.'; otherwise the request is exactly as before.
    Provider, model and max_tokens always come from config or models.json.
    """
    cfg = config or load_config()
    prompt = f"Explain in one sentence why vendor {vendor} is predicted to be paid {predicted_days} days late."
    if risk is not None:
        prompt += f" Risk level: {risk}."
    return {"provider": cfg["provider"], "model": cfg["model"], "max_tokens": cfg["max_tokens"],
            "messages": [{"role": "user", "content": prompt}]}

--- ap_forecast/src/ap_forecast/models.json ---
{
  "provider": "acme-llm",
  "model": "acme-text-2",
  "max_tokens": 300
}

--- ap_forecast/src/ap_forecast/predict.py ---
"""Predict a vendor's payment delay in days from its history (simple, explainable baseline model)."""
from statistics import mean
from typing import Iterable


def predict_delay_days(history: Iterable[int]) -> int:
    """Rounded mean of past delays in days; 0 when there is no history; negative delays (early payments) count as 0."""
    values = [max(int(d), 0) for d in history]
    return round(mean(values)) if values else 0

--- ap_forecast/src/ap_forecast/risk.py ---
"""Rule-based vendor payment-risk level from the predicted delay (STORY-03). No LLM, no network, no config read.

The rule is a fixed, auditable threshold check on predict_delay_days, so every level can be traced to the vendor's
delay history. This module imports only predict, never explain, so the rule cannot reach the LLM request path.
"""
from typing import Iterable

from .predict import predict_delay_days

# Thresholds in days on the rounded prediction; strict '>' so exactly 30 is 'medium' and exactly 10 is 'low'.
HIGH_ABOVE_DAYS = 30
MEDIUM_ABOVE_DAYS = 10


def vendor_risk(history: Iterable[int]) -> str:
    """'high' if the predicted delay is above HIGH_ABOVE_DAYS, 'medium' if above MEDIUM_ABOVE_DAYS, else 'low'.

    history is one vendor's delay days (ap_pipeline delays_by_vendor value). An empty history and early payments
    only (negative delays, counted as 0 by predict_delay_days) both give 'low'.
    """
    # Predict once and keep the result: a generator history is consumed on the first pass.
    days = predict_delay_days(history)
    if days > HIGH_ABOVE_DAYS:
        return "high"
    if days > MEDIUM_ABOVE_DAYS:
        return "medium"
    return "low"

--- ap_forecast/tests/__init__.py ---

--- ap_forecast/tests/test_forecast.py ---
import unittest

from ap_forecast import build_explanation_request, predict_delay_days


class ForecastTest(unittest.TestCase):
    def test_predict(self):
        self.assertEqual(predict_delay_days([10, 20, 30]), 20)
        self.assertEqual(predict_delay_days([]), 0)
        self.assertEqual(predict_delay_days([-5, 5]), 2)

    def test_request_uses_config(self):
        req = build_explanation_request("V-1", 12)
        self.assertEqual(req["model"], "acme-text-2")
        self.assertIn("V-1", req["messages"][0]["content"])

    def test_request_with_explicit_config(self):
        req = build_explanation_request("V-2", 3, {"provider": "p", "model": "m", "max_tokens": 10})
        self.assertEqual((req["provider"], req["max_tokens"]), ("p", 10))


if __name__ == "__main__":
    unittest.main()


Finish with the structured result: approach, tools_used, claims (each citing an input item name, a tool view, api:<name>, mcp:<server>, file:<repository>/<path> for any other GitHub file you read, record:intake, record:owner_answer, record:memory or record:brief for this brief; a claim drawing on several sources lists them separated by ';'; every number must appear in what you cite), open questions (anything missing or unclear: BLOCKING: <what you need and who should answer>; never assume), summary.