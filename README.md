# riskcheck

Checks whether LLM-written volatility commentary is actually backed by the data.

## What it does
1. Simulates daily returns (GARCH(1,1) with a volatility shock in the last month).
2. Computes ground-truth stats: 21-day annualized vol, change vs prior month, max drawdown, regime.
3. Gets commentary as structured claims (Claude if `ANTHROPIC_API_KEY` is set, otherwise a deliberately flawed mock).
4. Scores it automatically:
   - **supported / contradicted / unverifiable** for each claim (10% tolerance)
   - **hallucination rate** = share of claims not supported
   - **overconfident forecasts**: forward-looking sentences with no hedge word

## Run
    pip install -e ".[dev]"
    python -m riskcheck.cli
    pytest

## How I know the checker is right
Unit tests cover tolerance edges, invented metrics, hedge detection, and a hand-computed drawdown.

## Limits / next steps
- Tolerance is a fixed 10%; it should be tuned per metric.
- Hedge detection is keyword-based and misses subtle overconfidence.
- Next: swap simulated data for real returns, add a labeled test set of good/bad commentary, and track hallucination rate across prompt versions.
