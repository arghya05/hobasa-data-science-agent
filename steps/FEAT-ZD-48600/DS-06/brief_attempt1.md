# Step DS-06: Unit Test Cases (attempt 1) for FEAT-ZD-48600

- Workbook trigger: Code completion
- Workbook action: Write the unit test cases to cover the updated or newly written code as per the requirement documents with at least 90 percent of the code coverage
Access for the secrets in all the environments ²
- Workbook outcome: Unit testing completed
- Owner of this step: Developer
- Gate (who approves this output): no gate: automated checks, the independent judge and sampled human evaluation

## Inputs (GitHub checkout; read them with Read, or context_get for a receipt)
- task.code (Code changes on the working branch): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-06/task.code.md` (from hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.code.md at commit 286af9a)
- feat.stories (Epics, stories and tasks with numbered AC, frame links, assumptions and open items (latest revision)): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-06/feat.stories.md` (from hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.stories.md at commit 0590ea4)

## Outputs (write each file in full; the spine commits and pushes after evaluation and approval)
- task.unit_tests (Unit tests and coverage (≥90%)): write `hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.unit_tests.md`

## Code
Work in repository `ap-forecast-sample` (Python 3.9 (standard library)). You may change only: tests/. Run the CI yourself with `/usr/bin/python3 ci.py` from the repository root (or delegate to the ci-runner subagent) until it passes. Do not commit or push; the spine does.

## How to work
Decide the best way to do this step from the inputs and the workbook action, plan it (TodoWrite for multi-part work), then do it. Helper subagents you can delegate to: explorer, verifier, ci-runner (use the verifier before you finish when the output makes many factual statements). Use the shared memory (memory_search) for decisions and lessons. Connectors approved for you: api:osv (Known vulnerabilities for a dependency version (POST /v1/query with {"package":{"name":...,"ecosystem":"PyPI|npm"},"version":...})), api:pypi (Python package metadata: latest version, licence, release dates (GET /pypi/<package>/json)). Use one only when your inputs do not hold what you need; request a missing one with capability_request. You may also read any other file in the GitHub checkouts your step needs (for example other requirements when grouping, or code); cite it as file:<repository>/<path>.

## Tool view (cite as tool:repo)
--- ap_forecast/src/ap_forecast/__init__.py ---
"""AP forecast: predicted payment delay, rule-based vendor risk level and an LLM explanation request."""
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
    """Request dict for the configured provider. When risk is given (not None), the prompt ends with ' Risk level: <risk>.'

    Without risk (or with risk=None) the request is exactly as before. Provider, model and max_tokens always come from
    config or models.json.
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
"""Rule-based vendor payment-risk level from the predicted delay (STORY-03). No LLM call and no models.json read."""
from typing import Iterable

from .predict import predict_delay_days

# Thresholds in days, compared with strict ">" on the rounded prediction: exactly 30 is 'medium', exactly 10 is 'low'.
HIGH_ABOVE_DAYS = 30
MEDIUM_ABOVE_DAYS = 10


def vendor_risk(history: Iterable[int]) -> str:
    """'high' if the predicted delay is above HIGH_ABOVE_DAYS, 'medium' if above MEDIUM_ABOVE_DAYS, else 'low'.

    history is one vendor's delay days (for example one value of ap_pipeline's delays_by_vendor). It is read once,
    through predict_delay_days, so an empty history or early payments only (negative delays) give 'low'.
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