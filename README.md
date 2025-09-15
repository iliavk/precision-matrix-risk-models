# Precision-Matrix Risk Models

**Quick baseline to compare Ledoit–Wolf (LW) vs Graphical Lasso (GL) precision/covariance estimators for portfolio risk forecasting and stability.**

> One-file demo: see the notebook **precision_matrix_risk_models.ipynb**. Colab-ready; minimal dependencies; clean plots.

## What this does
- Pulls market data (daily) for a representative large-cap set (replace with your universe if desired).
- Rolling estimation: 2-year training window, monthly rebalancing.
- Models: **Ledoit–Wolf** shrinkage and **Graphical Lasso** (with CV to pick alpha).
- Portfolios:
  - **Equal-weight (EW)** baseline for risk forecast comparison.
  - **Minimum-variance (MV)** under each model (analytic, sum-to-1; unconstrained/no-short limit).
- **Risk forecast** vs **realized risk** (next-month variance) with MSE/MAE/Bias/Correlation + calibration.
- **Stability**: MV turnover across rebalances.
- **Structure**: GL precision-graph edge density over time.

## Why this is useful (short version)
- LW is a strong shrinkage baseline that reduces sampling noise.
- GL estimates a **sparse inverse covariance** (precision) which can stabilize structure and aid interpretability.
- This repo shows how those differences show up in **out-of-sample risk** and **portfolio stability**.

## Quickstart
```bash
# (optional) create a venv
python -m venv .venv && source .venv/bin/activate  # on Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

Open the notebook and run all cells.
- To change the universe, edit the `tickers` list in section **1) Data**.
- For more assets, be mindful of GraphicalLassoCV runtime; you can pre-specify an `alphas` grid or reduce frequency.

## Methods (1-paragraph intuition)
- **Ledoit–Wolf**: shrinks the sample covariance toward a structured target to reduce variance of the estimator.
- **Graphical Lasso**: solves an L1-penalized maximum-likelihood problem for a *sparse* precision matrix; zero off-diagonals imply conditional independencies.
- **Min-variance portfolio** (unconstrained w/ sum=1): \(w \propto \Sigma^-1\mathbf{1}\). We evaluate stability by **turnover** between consecutive rebalances.
- **Risk evaluation**: Compare **forecast** \(\hat{\sigma}^2 = w^\top \hat{\Sigma} w\) against **realized** next-window variance of the portfolio returns.

## Repo structure (suggested)
```
precision-matrix-risk-models/
├── precision_matrix_risk_models.ipynb
├── requirements.txt
├── README.md
└── LICENSE (optional, e.g., MIT)
```

## Next steps / extensions
- Add **no-short** or **box** constraints via `cvxpy` for MV; compare stability again.
- Try sector-neutral or factor-neutral portfolios.
- Expand universe to full S&P 500 and compare runtime vs accuracy.
- Evaluate **tail risk**: realized semi-variance or VaR/ES calibration.
- Compare to **Oracle Approximating Shrinkage (OAS)** or **Nonlinear shrinkage** as extra baselines.

---

*Generated on 2025-09-15.*
