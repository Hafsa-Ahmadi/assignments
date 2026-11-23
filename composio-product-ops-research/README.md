# Composio: 100-App Integration Readiness Research

**Handoff status (23 Nov 2026):** the matrix represents all 100 supplied applications across 10 categories. 36 dossiers are manually source-reviewed; the other 64 are explicitly pending. The automated 100-app run and measured agent-accuracy evaluation have not been executed. Do not claim full 100-app research completion or a fabricated accuracy improvement.

## Project contents
- `data/apps.json`: exact 100-app assignment input.
- `agent/research_agent.py`: two-pass agent using Tavily retrieval and OpenAI structured extraction.
- `analysis/metrics.py`: descriptive metrics for the manually reviewed subset.
- `verification/evaluate.py`: claim-level outcome accounting after the automated verification pass.
- `tests/test_dataset.py`: checks the 100-app / 10-category invariant.
- Full current report, result matrix, verification notes and local validation artifacts are available in the downloadable submission bundle linked from the chat handoff.

## Current evidence snapshot
The 36 source-reviewed apps are a purposive sample across categories, not a random sample. Within that subset:
- REST was documented for 32/36 (88.9%).
- OAuth was present in 24/36 (66.7%; multi-label authentication).
- Official MCP was confirmed for 6/36 (16.7%); 29/36 remained unverified, which does not mean absent.
- Credential/scope complexity was analyst-tagged as a blocker in 22/36 (61.1%; blockers overlap).

These are subset-only descriptive statistics, not estimates for all 100 applications.

## Run the research agent
Python 3.10+:

```bash
python -m venv .venv
# Windows PowerShell: .venv\\Scripts\\Activate.ps1
# macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
```

Set `OPENAI_API_KEY`, `TAVILY_API_KEY`, and optionally `OPENAI_MODEL` in `.env`.

```bash
python agent/research_agent.py --limit 100 --verify-sample 15
python agent/research_agent.py --limit 100 --verify-sample 15 --resume
python analysis/metrics.py
python verification/evaluate.py
pytest -q
```

The agent keeps the machine's first-pass findings separate from fresh-search verification output. The verification sample is low-confidence/risk-prioritized, not random. Human review is required for ambiguous entitlement gates, partnership requirements, app-review processes, and strategic prioritization.

## Evidence and safety policy
Prefer official developer docs, API references, authentication pages, credential/pricing access pages, and official MCP repositories. Do not infer self-serve access from API existence. Keep Official, Third-party, Community and Unverified MCP distinct. Unknown is not No. Never commit API keys, OAuth tokens, or MCP session URLs/headers.

## Limitations
The live automated run requires OpenAI and Tavily credentials. Composio SDK/MCP dogfooding is optional and must only be described as tested after creating and exercising a real session. No public deployment is claimed by this repository branch.