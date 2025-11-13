# Global Strategies, Local Impact: The Jane Street Episode & Structural Dynamics of Indian Capital Markets
Analyzing Nifty Bank Index returns, volatility, and retail participation in light of the July 2025 Jane Street-SEBI Episode

This repository contains all analysis, code, and results for my research paper:

“To what extent did the July 3, 2025 Jane Street–driven derivatives disruption produce temporary or lasting changes in the participation and risk posture of retail investors in India’s index-options market?”

The analysis combines baseline regression modeling, event-study abnormal behavior analysis, and cross-segment comparisons between the overall index derivatives market and Bank Nifty (≈10.8% of the market).

All code is written in Python via Jupyter Notebooks.

## Stage 1 — Baseline Regression Models

(Full details in baseline_regressions.ipynb)

### Total Models: 24

- 2 Market Segments:
-     Overall Index Derivatives
-     Bank Nifty–specific (10.8% weight)

- 4 Model Specifications: X1–X4
-     Institutional Positioning
-     Lagged Effects
-     Market Stress
-     Expiry Dynamics

- 3 Dependent Variables:
-     Retail Share
-     Retail Short Ratio
-     Retail Put Share

### Key Findings 

- NSE VIX is highly significant in all retail share models (p < 0.001).
- FII Net Positioning significantly predicts retail behavior
- Retail Share: negative relationship
- Retail Shorts & Put Share: positive relationship (retail mirrors FIIs)
- Market Stress indicators: not significant beyond baseline volatility effects
- Expiry Dynamics: minimal to no explanatory power
- Lag effects: weak — retail is reactive, not predictive

### Model Fit:

- Retail Share: R² = 0.31–0.36 (strong for behavioral data)
- Retail Short Ratio: R² = 0.07–0.16 (noisy)
- Retail Put Share: R² = 0.01–0.12 (very noisy)

### Conclusion: Strong support for H₀ (null hypothesis) wherein retail behavior is primarily driven by contemporaneous market conditions, not structural breaks

## Stage 2 — Event Study

### Parameters
Event Date: July 3, 2025
Window: (−1, +1) trading days
Counterfactual: Predictions from Stage 1 baseline models

### Main Results
- Retail participation showed abnormal increases—a contrarian “buy-the-dip” dynamic
-     Significant CAR: +7.87% (t = 2.25)
-     Abnormal participation spikes: 3.01, 1.05, 3.80
- Retail did not increase hedging or short exposure
-     Suggests directional opportunism rather than risk-management behavior
-     Behavior returned to predicted baseline quickly → temporary shock, not structural

### Overall: Retail investors briefly deviated from model predictions by increasing participation after the event, but did not alter their risk posture (shorting/puts). No lasting behavioral change detected.

## Data Sources
- NSE Participant-Category Open Interest Data (Index Derivatives)
- Bloomberg Terminal: NIFTY & Bank Nifty volatility, returns, and institutional positioning
- Internal transformations & merged datasets located under /data/processed/
