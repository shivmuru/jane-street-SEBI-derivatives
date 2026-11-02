# The Jane Street Enforcement Shock: Retail Behavior in India's Index Options Market

**Shivani Murugapiran — Georgia Institute of Technology**

📄 **[Read the full paper (PDF)](paper/The_Jane_Street_Enforcement_Shock.pdf)**

This repository contains the code, data and results behind my research paper on how retail traders in India's index options market responded to SEBI's July 2025 interim order against Jane Street. The order barred the firm from Indian markets over alleged index manipulation and impounded ₹4,843.57 crore (~$576.6M USD) in alleged gains.

> **Research question:** To what extent did the July 2025 SEBI–Jane Street shock affect retail participation and retail risk exposure in India's index options market?

---

## Key Findings

| Outcome | Result (CAR, July 3 → July 11, 2025) | Interpretation |
|---|---|---|
| **Retail participation** (`RetailShare`) | **+26.89 pp** by trading day 7 | Retail traders *expanded* their market share after the shock instead of retreating. |
| **Retail risk exposure** (`RetailPutShare`) | **−13.41 pp** by trading day 7 | Retail traders systematically *reduced* put exposure. |

- **The original hypothesis was rejected.** I expected retail participation and risk exposure to fall. Participation rose, while put exposure fell.
- **The response was selective rather than uniform.** Retail traders increased their market presence while cutting specific risk exposures. This fits the *Volatility-Seeking* and *Structural Stability* schools on participation and a weaker form of the *Retail Retreat* school on risk.
- **The effect lasted over a week.** Abnormal retail share was positive on every day of the window, so this was more than a one-day reaction.
- **One possible mechanism is crowd-out.** Retail traders may have filled the liquidity gap left by a dominant institutional player. Social-media narratives about a "level playing field" may also have amplified participation.

### Baseline drivers (pre-event, Jan 15 – Jul 2, 2025; Newey-West HAC SEs)

| | RetailShare (R² = 0.353) | RetailPutShare (R² = 0.441) |
|---|---|---|
| Turnover-weighted market return | n.s. | **+** (p = 0.013) |
| India VIX | **−** (p < 0.001) | n.s. |
| Expiry day | n.s. | **+** (p = 0.014) |
| Liquidity (aggregate options volume) | **−** (p = 0.001) | **+** (p < 0.001) |

---

## Methodology

**Design:** an event study with a 120-trading-day pre-event estimation window (following MacKinlay, 1997). The window runs from **Jan 15, 2025 to Jul 2, 2025**. **Event date:** July 3, 2025. CARs are tracked over the [0,+1], [0,+3], [0,+5] and [0,+10] windows.

**Baseline model:**

```
Y_t = α + β1·WtdMarketReturn_t + β2·IndiaVIX_t + β3·isExpiry_t + β4·Liquidity_t + ε_t
```

**Dependent variables**
- `RetailShare` = Retail (Client) OI / Total OI
- `RetailPutShare` = Retail put OI (long + short) / Retail options OI
- `RetailShortRatio` = Retail short OI / Retail total OI *(also estimated; not part of the headline results)*

**Constructed regressors**
- **WtdMarketReturn:** turnover-weighted log returns across BANKNIFTY, FINNIFTY, MIDCPNIFTY, NIFTY and NIFTYNXT50. NSE participant OI is aggregated across index products, so the market factor is too.
- **Liquidity:** aggregate options volume across NIFTY, BANKNIFTY, FINNIFTY and NIFTYNXT50.
- **IndiaVIX:** NSE implied volatility.
- **isExpiry:** expiry-day dummy taken from the NSE calendar.

---

## Repository Structure

```
├── paper/
│   └── The_Jane_Street_Enforcement_Shock.pdf     # Final paper
├── final-model/
│   └── Method-Tests-vF.ipynb                     # Final baseline regressions + event study (paper methodology)
├── cleaned-data/
│   ├── conglomerated-updated-policy.csv          # Participant-wise OI + VIX + expiry, Aug 2024 – Oct 2025 (final model input)
│   ├── index-prices.csv                          # Daily closes for NSE indices (Bloomberg)
│   ├── index-turnover-nse-only.csv               # Daily turnover per index (Bloomberg)
│   ├── index-options-vol-nse-only.csv            # Daily options volume per index (Bloomberg)
│   ├── conglomerated-all.csv                     # Earlier-stage dataset (Feb – Oct 2025)
│   └── conglomerated-all-from-2024.csv           # Earlier-stage dataset (extended history)
├── raw-data/                                     # Original Bloomberg .xlsx pulls
├── baseline-model/                               # Exploratory stage: X1–X5 specifications (all indices & Bank Nifty)
├── event-study/                                  # Exploratory stage: (−1,+1) event study around July 3
├── master-analysis.ipynb                         # Exploratory stage: combined notebook
└── requirements.txt
```

### Exploratory stage (earlier work)

Before the final specification, I tested five alternative baseline specifications (`baseline-model/`) on the 3 retail metrics:
- **X1:** institutional (FII net positioning)
- **X2:** lagged effects
- **X3:** market stress
- **X4:** expiry dynamics
- **X5:** comprehensive

I also ran a short (−1, +1) event study (`event-study/`). That study found an early abnormal increase in retail share (3-day CAR = +7.87, t = 2.25) and no significant change in short ratio or put share over that narrow window. These results led to the final design, which uses a cleaner 120-day estimation window, turnover-weighted market returns, an explicit liquidity control and a longer post-event window.

---

## Data Sources

- **NSE:** participant-wise open interest reports (Client / DII / FII / Pro) for index derivatives, and the NSE expiry calendar
- **Bloomberg Terminal:** index prices, turnover, options volume and India VIX
- **Regulatory and qualitative context:** SEBI's July 2025 interim order, financial press (FT, Economic Times, Business Standard) and Jane Street's public responses

## Reproducing

```bash
pip install -r requirements.txt
```

The notebooks read their input CSVs from the working directory. Before you run a notebook, copy the files it needs from `cleaned-data/` into that notebook's folder, or start Jupyter from `cleaned-data/`.

## Limitations

- NSE participant OI is aggregated across all index products. There is no Bank Nifty-specific retail OI and no firm-level data on Jane Street itself.
- The 2024–25 period also contained several other SEBI regulatory changes, so the estimates may partly reflect cumulative regulatory uncertainty.
- The event-study design identifies abnormal behavior but cannot by itself establish the causal mechanism.
