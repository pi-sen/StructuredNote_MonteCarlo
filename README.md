
Contingent Income Auto-Callable Securities Valuation (Monte Carlo Simulation)
=============================================================================

📈 Overview
-----------
This project implements a Monte Carlo simulation to price a **2-year Contingent Income Auto-Callable Note** linked to **three underlying equities**: Halliburton (HAL), Valero (VLO), and Exxon Mobil (XOM). The note pays **quarterly coupons** if all stocks are above 50% of their initial prices and auto-calls if all exceed 100% on observation dates. A **worst-of payoff** applies at maturity if any stock breaches the downside threshold.

The final note price is estimated at **$952.91**, averaged from simulations using 100% and 50% implied volatilities.

💡 Key Features
---------------
- Monte Carlo simulation with **1,024,000 paths** for robust convergence
- **Correlated asset dynamics** modeled with geometric Brownian motion
- Backward induction incorporating:
  - Auto-call feature
  - Conditional coupon logic
  - Downside protection
- Historical correlations and implied volatility surface from Bloomberg
- Rigorous **convergence and sensitivity analysis**

🔧 Product Specs
----------------
- **Issuer**: UBS AG London Branch  
- **Principal**: $1,000  
- **Contingent Coupon**: $25.775 quarterly (10.31% p.a.)  
- **Auto-call Condition**: All assets ≥ 100% of initial  
- **Coupon Barrier / Downside Threshold**: 50% of initial for all assets  
- **Maturity**: March 11, 2027  
- **Observation Dates**: Quarterly (8 in total)

🧠 Methodology
--------------
### 1. **Stock Simulation**
- Asset paths simulated using **discretized GBM** with correlated shocks.
- Inputs:
  - Initial prices, dividend yields, implied volatilities (both 50% & 100% moneyness)
  - Historical correlation matrix (via Cholesky decomposition)
  - Forward SOFR-based rates between quarterly dates
- Result: 3D array of shape `(1024000, 8, 3)` containing stock price paths

### 2. **Valuation Framework**
- Backward induction across 8 determination dates
- Conditions evaluated per path at each date:
  - All assets ≥ call threshold → auto-call and exit
  - All assets ≥ coupon barrier → receive discounted coupon
  - Any asset < coupon barrier → continuation only
- Maturity logic includes worst-of return payout if thresholds breached

📊 Sensitivity Analysis
------------------------
- **Volatility Scenarios**:
  - 100% moneyness: Price = $980.98
  - 50% moneyness: Price = $924.83
  - Final Price = Average = **$952.91**
- Higher volatility → more paths hit auto-call threshold → lower final value

- **Convergence**:
  - Std. error reduced to < $0.30 at 1.024 million simulations
  - Matches theoretical convergence: `O(1/√N)`
  - Linearly scalable simulation time


▶️ How to Run (if applicable)
-----------------------------
> If implemented in Python:
1. Install dependencies (`numpy`, `pandas`, `matplotlib`)
2. Run `python simulate_autocall.py`
3. Output includes price estimate and error metrics

📌 Authors
----------
- Piyush Sen
- Srikari Rallabandi
- Aastha Shah

📄 License
----------
MIT License (or your preferred open-source license)
