# Portfolio Optimization Engine

A Mean-Variance Optimization (MVO) engine built on a 10-stock Nifty 50 universe spanning three risk tiers (safe, moderate, risky), using Modern Portfolio Theory to construct a Sharpe-Ratio-maximizing portfolio and visualize diversification effects.

## Overview

This project answers a core asset management question: *given a set of stocks, what allocation maximizes return per unit of risk taken?*

Built using Harry Markowitz's Modern Portfolio Theory (1952), the engine pulls historical price data, estimates risk/return characteristics, and solves a constrained optimization problem to find the portfolio with the highest Sharpe Ratio — the same core methodology used by wealth managers, mutual funds, and pension funds.

## Stock Universe

10 Nifty 50 stocks were deliberately selected across three risk tiers to demonstrate diversification effects:

| Risk Tier | Stocks |
|---|---|
| **Safe / Low-volatility** | HINDUNILVR, ITC, NESTLEIND |
| **Moderate** | TCS, RELIANCE, ICICIBANK |
| **Risky / High-beta** | M&M, ADANIENT, BAJFINANCE, TATASTEEL |

Data source: Yahoo Finance (`yfinance`), daily adjusted close prices, 2019–2024 (~5 years).

## Methodology

1. **Data Collection** — Downloaded daily price series for all 10 tickers via `yfinance`.
2. **Returns & Risk Estimation** — Computed annualized expected returns (`mean_historical_return`) and the annualized sample covariance matrix (`sample_cov`) using `PyPortfolioOpt`.
3. **Mean-Variance Optimization** — Solved for the Max Sharpe Ratio portfolio using `EfficientFrontier.max_sharpe()`, which internally uses `cvxpy` to reformulate the (non-convex) Sharpe maximization into a solvable convex problem. Risk-free rate assumed at 7% (approx. India 10-year G-Sec yield).
4. **Visualization** — Plotted the correlation matrix heatmap to visually confirm diversification benefits across risk tiers.

## Results

**Optimal Portfolio Weights (Max Sharpe):**

| Stock | Weight |
|---|---|
| ADANIENT.NS | 49.56% |
| M&M.NS | 21.05% |
| ICICIBANK.NS | 13.02% |
| TCS.NS | 11.76% |
| NESTLEIND.NS | 4.61% |
| BAJFINANCE.NS | 0.00% |
| HINDUNILVR.NS | 0.00% |
| ITC.NS | 0.00% |
| RELIANCE.NS | 0.00% |
| TATASTEEL.NS | 0.00% |

**Portfolio Performance:**
- Expected Annual Return: **42.1%**
- Annual Volatility: **32.6%**
- Sharpe Ratio: **1.08**

## Key Insights

- **ADANIENT dominates the allocation (~50%)** due to its outsized historical return (61.5% annualized), which the optimizer favors despite high individual volatility, since its correlation with other holdings remains moderate (0.02–0.07) rather than extreme.
- **Zero-weight stocks are not "bad" stocks** — HINDUNILVR, ITC, RELIANCE, TATASTEEL, and BAJFINANCE were excluded not because they underperform, but because other holdings offered better risk-adjusted contribution to the portfolio. This illustrates a key MVO lesson: being individually "safe" is not the same as being portfolio-efficient.
- **The correlation heatmap confirms the diversification thesis** — the FMCG/defensive cluster (HINDUNILVR, ITC, NESTLEIND) shows visibly lower correlation with high-beta names (ADANIENT, BAJFINANCE), which is precisely the structural feature MVO exploits to reduce portfolio risk.
- **Sharpe Ratio of 1.08** is a strong risk-adjusted outcome (>1 is generally considered good).

## Limitations & Future Extensions

- This is a **historical, in-sample optimization** — it reflects what *would have been* optimal for 2019–2024 based on realized returns, not a forward-looking guarantee. ADANIENT's dominant weight is partly a function of its extraordinary bull run within this specific window, a known limitation of classical MVO (return estimates are highly sensitive to the historical window used).
- Potential extensions: Black-Litterman model to blend historical data with forward-looking views, adding position-size constraints (e.g., max 30% per stock) to reduce concentration risk, or rolling-window backtesting to test out-of-sample robustness.

## Tech Stack

- **Data**: Yahoo Finance (`yfinance`)
- **Optimization**: `PyPortfolioOpt`, `cvxpy`
- **Data Handling**: `pandas`, `numpy`
- **Visualization**: `matplotlib`

## Skills Demonstrated

Mean-Variance Optimization · Efficient Frontier theory · Sharpe Ratio maximization · Covariance/correlation analysis · Constrained convex optimization · Financial data engineering
