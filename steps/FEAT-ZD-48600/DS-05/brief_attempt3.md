Your previous result for this step was refused. Fix every point, rewrite the output files in full, and return the structured result again. The outputs describe the work itself: never mention earlier attempts, refusals or this review in them.

## Refusal reasons
G3 numbers ['83.3', '21', '24', '22'] in "The CI run compiled, ran the 3 existing tests with 0 failures and 0 errors, and reported no static i" do not appear in file:ap_forecast/ci.py; tool:repo

## Step brief (unchanged except the attempt number)
# Step DS-05: Coding (attempt 3) for FEAT-ZD-48600

- Workbook trigger: Previous outcome
- Workbook action: Coding for the implementation required for changes in the components/layers with proper comments and required documentation
Access for the secrets in all the environments
- Workbook outcome: Updated repo with the new implemented code changes
- Owner of this step: Developer
- Gate (who approves this output): no gate: automated checks, the independent judge and sampled human evaluation

## Inputs (GitHub checkout; read them with Read, or context_get for a receipt)
- task.components (Components, layers, sources or pipelines to change): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/task.components.md` (from hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.components.md at commit f956a35)
- feat.design (HLD, LLD and contracts): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/feat.design.md` (from hobasa-design-agent/outputs/FEAT-ZD-48600/feat.design.md at commit 722a98f)
- feat.stories (Epics, stories and tasks with numbered AC, frame links, assumptions and open items (latest revision)): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/feat.stories.md` (from hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.stories.md at commit 0590ea4)
- org.engineering_stack (Repositories, languages, versions and exact commands): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/org.engineering_stack.md` (from hobasa-agentic-sdlc/org/org.engineering_stack.md at commit 3b03f15)
- org.lessons (Approved lessons and eval cases): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-05/org.lessons.md` (from hobasa-agentic-sdlc/org/org.lessons.md at commit 3b03f15)
- task.secrets_scope (Secrets granted for the task's environments (names only)): restricted, passed by reference only

## Outputs (write each file in full; the spine commits and pushes after evaluation and approval)
- task.code (Code changes on the working branch): write `hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.code.md`

## Code
Work in repository `ap-forecast-sample` (Python 3.9 (standard library)). You may change only: src/ap_forecast/, docs/, README.md. Shell commands allowed in this step: `/usr/bin/python3 ci.py`, `git status`, `git diff`, `git log`. Run the CI yourself with `/usr/bin/python3 ci.py` from the repository root (or delegate to the ci-runner subagent) until it passes. Do not commit or push; the spine does. Document what you change: update the repository's README.md and docs/ (module overview, new functions with signatures, inputs, outputs and examples).

## How to work
Decide the best way to do this step from the inputs and the workbook action, plan it (TodoWrite for multi-part work), then do it. Helper subagents you can delegate to: explorer, verifier, ci-runner (use the verifier before you finish when the output makes many factual statements). Use the shared memory (memory_search) for decisions and lessons. Connectors approved for you: api:osv (Known vulnerabilities for a dependency version (POST /v1/query with {"package":{"name":...,"ecosystem":"PyPI|npm"},"version":...})), api:pypi (Python package metadata: latest version, licence, release dates (GET /pypi/<package>/json)). Use one only when your inputs do not hold what you need; request a missing one with capability_request. You may also read any other file in the GitHub checkouts your step needs (for example other requirements when grouping, or code); cite it as file:<repository>/<path>.

## Tool view (cite as tool:repo)
--- ap_forecast/src/ap_forecast/__init__.py ---
"""AP forecast: predicted payment delay, rule-based vendor risk level and an LLM explanation request."""
from .predict import predict_delay_days
from .risk import vendor_risk
from .explain import build_explanation_request

__version__ = "0.10.0"
__all__ = ["predict_delay_days", "vendor_risk", "build_explanation_request"]

--- ap_forecast/src/ap_forecast/explain.py ---
"""Build the LLM request that explains a prediction to the AP user. The provider and model come from models.json."""
import json
from pathlib import Path

CONFIG = Path(__file__).with_name("models.json")


def load_config():
    return json.loads(CONFIG.read_text())


def build_explanation_request(vendor: str, predicted_days: int, config=None, risk=None) -> dict:
    """Request dict for the explanation; only builds it, does not send it.

    `risk` (FEAT-ZD-48600, STORY-04) is optional and last, so existing positional calls are unchanged. When it is not
    None, " Risk level: <risk>." is appended to the prompt; otherwise the prompt and the result are exactly as before.
    Provider, model and max_tokens always come from `config` or models.json.
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
"""Rule-based vendor payment-risk level from the predicted delay (FEAT-ZD-48600, STORY-03).

The rule is fixed and explainable: no LLM call, no network call and no read of models.json. This module deliberately
does not import `explain`, so the risk rule cannot reach the LLM path.
"""
from typing import Iterable

from .predict import predict_delay_days

# Thresholds in predicted delay days, compared with strict ">" on the rounded prediction.
HIGH_ABOVE_DAYS = 30
MEDIUM_ABOVE_DAYS = 10


def vendor_risk(history: Iterable[int]) -> str:
    """'high' if the predicted delay is above HIGH_ABOVE_DAYS, 'medium' if above MEDIUM_ABOVE_DAYS, else 'low'.

    The prediction is predict_delay_days(history), called once (so a generator works): an empty history predicts 0
    and early payments (negative delays) count as 0, so both give 'low'. Exactly 30 is 'medium'; exactly 10 is 'low'.
    """
    days = predict_delay_days(history)
    if days > HIGH_ABOVE_DAYS:
        return "high"
    return "medium" if days > MEDIUM_ABOVE_DAYS else "low"

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


Finish with the structured result: approach, tools_used, claims (each citing an input item name, a tool view, api:<name>, mcp:<server>, file:<repository>/<path> for any other GitHub file you read, record:intake, record:owner_answer, record:memory or record:brief for this brief; a claim drawing on several sources lists them separated by ';'; every number must appear in what you cite), open questions (BLOCKING: only when you cannot finish), summary.