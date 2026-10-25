# Evaluate step DS-06: Unit Test Cases for FEAT-ZD-48600

- Workbook action: Write the unit test cases to cover the updated or newly written code as per the requirement documents with at least 90 percent of the code coverage
Access for the secrets in all the environments ²
- Workbook outcome: Unit testing completed

## Outputs to evaluate
- `hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.unit_tests.md`

## Inputs the agent was given
- task.code: `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-06/task.code.md`
- feat.stories: `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-06/feat.stories.md`

## Tool results the spine recorded from the real tools in this step (a valid source; the section 'Tool results' at the end of an output file is written by the spine, not the agent)


[Tool result] [ap_forecast] Unit tests: 25 run, 0 failing; coverage 100.0%

## Other GitHub files the agent read (valid sources when cited as file:<repository>/<path>)
- `hobasa-data-science-agent/.claude/skills/data-science-steps/SKILL.md`
- `hobasa-data-science-agent/CLAUDE.md`
- `hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.unit_tests.md`
- `ap-aging-sample/CLAUDE.md`
- `ap-forecast-sample/CLAUDE.md`
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
G1 judge found an unsupported statement: "The tests avoid means that end in exactly .5, so each case shows the threshold rule clearly." (The tests in the tool view contradict this. `test_above_30_is_high` uses `[45, 60]` (mean 52.5). `test_between_10_and_30_is_medium` uses `[20, 25]` (mean 22.5).)
G1 judge found an unsupported statement: "The only file they read is the packaged `models.json`, through `load_config`." (The tool view contradicts this. `test_module_imports_only_typing_and_predict` in tests/test_risk.py runs `Path(risk_module.__file__).read_text()`, which reads `)

## Step brief (unchanged except the attempt number)
# Step DS-06: Unit Test Cases (attempt 3) for FEAT-ZD-48600

- Workbook trigger: Code completion
- Workbook action: Write the unit test cases to cover the updated or newly written code as per the requirement documents with at least 90 percent of the code coverage
Access for the secrets in all the environments ²
- Workbook outcome: Unit testing completed
- Owner of this step: Developer
- Gate (who approves this output): no gate: automated checks, the independent judge and sampled human evaluation

## Inputs (GitHub checkout; read them with Read, or context_get for a receipt)
- task.code (Code changes on the working branch): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-06/task.code.md` (from hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.code.md at commit 7b73e9c)
- feat.stories (Epics, stories and tasks with numbered AC, frame links, assumptions and open items (latest revision)): `hobasa-data-science-agent/inputs/FEAT-ZD-48600/DS-06/feat.stories.md` (from hobasa-requirement-agent/outputs/FEAT-ZD-48600/feat.stories.md at commit 0590ea4)

## Outputs (write each file in full; the spine commits and pushes after evaluation and approval)
- task.unit_tests (Unit tests and coverage (≥90%)): write `hobasa-data-science-agent/outputs/FEAT-ZD-48600/task.unit_tests.md`

## Code
Work in repository `ap-forecast-sample` (Python 3.9 (standard library)). You may change only: tests/. Understand before you change: the repository is indexed with CodeGraph. Before editing, use it (MCP tools codegraph_explore and codegraph_node with projectPath `ap-forecast-sample`, or `codegraph explore <symbols>`, `codegraph impact <symbol>`, `codegraph callers <symbol>`) to read the exact source of what you change, its callers and callees and the blast radius. After editing, run `codegraph sync` and `codegraph affected <changed files>` to find the tests your change touches, and run them through the CI. Shell commands allowed in this step: `/usr/bin/python3 ci.py`, `git status`, `git diff`, `git log`, `codegraph explore`, `codegraph node`, `codegraph callers`, `codegraph callees`, `codegraph impact`, `codegraph affected`, `codegraph query`, `codegraph files`, `codegraph status`, `codegraph sync`. Run the CI yourself with `/usr/bin/python3 ci.py` from the repository root (or delegate to the ci-runner subagent) until it passes. Do not commit or push; the spine does.

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

--- ap_forecast/tests/test_explain_risk.py ---
"""STORY-04: optional risk level in the explanation request (AC-04.1 to AC-04.5)."""
import unittest

from ap_forecast import build_explanation_request
from ap_forecast.explain import load_config

CFG = {"provider": "p", "model": "m", "max_tokens": 10}
BASE = "Explain in one sentence why vendor Synthetic Vendor A is predicted to be paid 12 days late."


class ExplanationRiskTest(unittest.TestCase):
    def test_risk_appended_to_prompt(self):  # AC-04.1
        for level in ("high", "medium", "low"):
            req = build_explanation_request("Synthetic Vendor A", 12, CFG, risk=level)
            content = req["messages"][0]["content"]
            self.assertEqual(content, BASE + f" Risk level: {level}.")
            self.assertTrue(content.endswith(f" Risk level: {level}."))

    def test_risk_positional_fourth_argument(self):  # AC-04.1: risk is the last parameter
        req = build_explanation_request("Synthetic Vendor A", 12, CFG, "high")
        self.assertEqual(req["messages"][0]["content"], BASE + " Risk level: high.")

    def test_without_risk_request_is_unchanged(self):  # AC-04.2
        expected = {"provider": "p", "model": "m", "max_tokens": 10,
                    "messages": [{"role": "user", "content": BASE}]}
        self.assertEqual(build_explanation_request("Synthetic Vendor A", 12, CFG), expected)

    def test_explicit_none_is_not_given(self):  # AC-04.5 [Pending TQ-4]
        self.assertEqual(build_explanation_request("Synthetic Vendor A", 12, CFG, risk=None),
                         build_explanation_request("Synthetic Vendor A", 12, CFG))
        self.assertNotIn("Risk level", build_explanation_request("Synthetic Vendor A", 12, CFG, None)["messages"][0]["content"])

    def test_existing_positional_config_call(self):  # AC-04.3
        req = build_explanation_request("Synthetic Vendor A", 12, CFG)
        self.assertEqual((req["provider"], req["model"], req["max_tokens"]), ("p", "m", 10))

    def test_provider_model_tokens_from_config_with_risk(self):  # AC-04.4 (explicit config)
        req = build_explanation_request("Synthetic Vendor A", 12, CFG, risk="high")
        self.assertEqual((req["provider"], req["model"], req["max_tokens"]), ("p", "m", 10))
        self.assertEqual(len(req["messages"]), 1)
        self.assertEqual(req["messages"][0]["role"], "user")

    def test_provider_model_tokens_from_models_json_with_risk(self):  # AC-04.4 (models.json)
        cfg = load_config()
        req = build_explanation_request("Synthetic Vendor A", 12, risk="medium")
        self.assertEqual((req["provider"], req["model"], req["max_tokens"]),
                         (cfg["provider"], cfg["model"], cfg["max_tokens"]))
        self.assertTrue(req["messages"][0]["content"].endswith(" Risk level: medium."))


if __name__ == "__main__":
    unittest.main()

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

--- ap_forecast/tests/test_risk.py ---
"""STORY-03: rule-based vendor risk level (AC-03.1 to AC-03.8). Boundaries are tested on the rounded prediction (L-007)."""
import ast
import unittest
from pathlib import Path
from unittest import mock

import ap_forecast
from ap_forecast import predict_delay_days, vendor_risk
from ap_forecast import risk as risk_module


class VendorRiskThresholdTest(unittest.TestCase):
    def test_above_30_is_high(self):  # AC-03.1
        self.assertEqual(vendor_risk([31]), "high")
        self.assertEqual(vendor_risk([45, 60]), "high")

    def test_exactly_30_is_medium(self):  # AC-03.2
        self.assertEqual(vendor_risk([30]), "medium")

    def test_between_10_and_30_is_medium(self):  # AC-03.3
        self.assertEqual(vendor_risk([11]), "medium")
        self.assertEqual(vendor_risk([20, 25]), "medium")

    def test_exactly_10_is_low_and_11_is_medium(self):  # AC-03.4
        self.assertEqual(vendor_risk([10]), "low")
        self.assertEqual(vendor_risk([11]), "medium")

    def test_empty_history_is_low(self):  # AC-03.5 [Pending PQ-1]
        self.assertEqual(vendor_risk([]), "low")

    def test_early_payments_only_are_low(self):  # AC-03.6 [Pending TQ-5]
        self.assertEqual(vendor_risk([-5, -20]), "low")

    def test_negative_delays_count_as_zero_not_netted(self):  # AC-03.6 / predict.py: negatives count as 0
        self.assertEqual(predict_delay_days([-100, 62]), 31)
        self.assertEqual(vendor_risk([-100, 62]), "high")

    def test_threshold_applies_to_rounded_prediction(self):  # IF-3.4 via AC-03 preamble
        # Mean 30.33 rounds to 30 and mean 10.33 rounds to 10, so both stay below the next level.
        self.assertEqual(vendor_risk([30, 30, 31]), "medium")
        self.assertEqual(vendor_risk([10, 10, 11]), "low")
        self.assertEqual(vendor_risk([31, 31, 30]), "high")

    def test_generator_history(self):  # history is any Iterable[int]; predicted once
        self.assertEqual(vendor_risk(d for d in [31, 31]), "high")

    def test_constants(self):
        self.assertEqual((risk_module.HIGH_ABOVE_DAYS, risk_module.MEDIUM_ABOVE_DAYS), (30, 10))


class VendorRiskIsolationTest(unittest.TestCase):
    def test_no_config_read_or_llm_request(self):  # AC-03.7
        with mock.patch("ap_forecast.explain.load_config", side_effect=AssertionError("models.json read")), \
                mock.patch("ap_forecast.explain.build_explanation_request",
                           side_effect=AssertionError("LLM request built")):
            for history in ([], [5], [11], [31], [-3]):
                vendor_risk(history)

    def test_module_imports_only_typing_and_predict(self):  # AC-03.7: no explain, network or config import
        tree = ast.parse(Path(risk_module.__file__).read_text())
        imported = set()
        for node in ast.walk(tree):
            if isinstance(node, ast.Import):
                imported.update(alias.name for alias in node.names)
            elif isinstance(node, ast.ImportFrom):
                imported.add(("." * node.level) + (node.module or ""))
        self.assertEqual(imported, {"typing", ".predict"})


class VendorRiskExportTest(unittest.TestCase):
    def test_exported_in_all(self):  # AC-03.8
        self.assertIn("vendor_risk", ap_forecast.__all__)
        self.assertIs(ap_forecast.vendor_risk, risk_module.vendor_risk)

    def test_existing_exports_kept(self):  # NFR-4
        for name in ("predict_delay_days", "build_explanation_request"):
            self.assertIn(name, ap_forecast.__all__)

    def test_returns_only_known_levels(self):  # AC-03.8
        levels = {vendor_risk([d]) for d in range(-5, 61)}
        self.assertEqual(levels, {"high", "medium", "low"})


if __name__ == "__main__":
    unittest.main()


Finish with the structured result: approach, tools_used, claims (each citing an input item name, a tool view, api:<name>, mcp:<server>, file:<repository>/<path> for any other GitHub file you read, record:intake, record:owner_answer, record:memory or record:brief for this brief; a claim drawing on several sources lists them separated by ';'; every number must appear in what you cite), open questions (anything missing or unclear: BLOCKING: <what you need and who should answer>; never assume), summary.