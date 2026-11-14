# Deconstructing Egyptian Inflation Dynamics (2013–2028)
### An Open-Economy Hybrid New Keynesian Phillips Curve (NKPC) Econometric Analysis

**Google Data Analytics Professional Certificate — Capstone Case Study**  
**Author:** Hossam El-Din Nassef | **Date:** November 2026  
**Tech Stack:** Google BigQuery (SQL) · Python (`pandas`, `numpy`, `statsmodels`, `matplotlib`)

[![Presentation](https://img.shields.io/badge/Google_Slides-Full_Case_Study_Deck-F4B400?style=for-the-badge&logo=googleslides&logoColor=white)](https://docs.google.com/presentation/d/1hpSrXcGTXojNnhcRPKeTGfsM-pR-BIk-6QVkmr_dL3s/edit)
[![BigQuery](https://img.shields.io/badge/Google_BigQuery-SQL_Pipeline-4285F4?style=for-the-badge&logo=googlebigquery&logoColor=white)](#3-process-data-engineering--feature-pipeline)
[![Python](https://img.shields.io/badge/Python-OLS_%26_Econometrics-3776AB?style=for-the-badge&logo=python&logoColor=white)](#4-analyze-econometric-specification--ols-results)

---

## Executive Summary

Between 2013 and 2028, Egypt experienced acute inflationary waves—most notably following the November 2016 exchange rate floatation and the 2022–2024 external liquidity and currency realignments. Conventional monetary stabilization relies heavily on aggressive policy interest rate hikes designed to suppress domestic aggregate demand.

This study estimates an **Open-Economy Hybrid New Keynesian Phillips Curve (NKPC)** via Ordinary Least Squares (OLS) to test whether Egyptian Consumer Price Index (CPI) volatility is governed by domestic demand-pull pressure (Output Gap) or imported cost-push shocks via Exchange Rate Pass-Through (ERPT). The empirical model accounts for **92.2% of historical CPI variance ($R^2 = 0.922$)** and demonstrates that **exchange rate depreciation ($\Delta \ln \text{FX}$, $p < 0.001$) and inflation inertia ($\pi_{t-1}$, $p < 0.001$) drive inflation**, while domestic output slack is statistically indistinguishable from noise ($p = 0.675$).

---

## 1. ASK: Problem Definition & Strategic Hypothesis

* **Primary Objective:** Evaluate the macroeconomic determinants of headline inflation in Egypt across historical actuals and institutional projections (2013–2028) using an Open-Economy Hybrid NKPC framework.
* **Core Hypothesis:** Cost-push Exchange Rate Pass-Through ($\Delta \ln \text{FX}$) fundamentally dominates domestic demand-pull pressure (Output Gap) in driving CPI volatility.
* **Key Institutional Research Questions:**
  1. What is the statistical significance of exchange rate depreciation versus domestic output slack in explaining Egyptian inflation spikes?
  2. How accurately does the empirical OLS model capture non-linear structural devaluation shocks (2016–2017 and 2022–2024)?
  3. Should monetary policy prioritize domestic demand suppression (interest rate hikes) or foreign exchange liquidity stabilization and structural external reform?

---

## 2. PREPARE: Institutional Data Architecture & Scope

* **Temporal Scope & Baseline:** Annual time series spanning **2013–2028**, recalibrated to **Base Year 2013 ($\text{Index} = 100$)** to quantify cumulative purchasing power erosion.
* **Primary Institutional Data Sources:**
  * **Headline CPI Inflation (%):** World Bank World Development Indicators (`FP.CPI.TOTL.ZG`) & Central Agency for Public Mobilization and Statistics (CAPMAS) (2013–2024 actuals).
  * **Real GDP Growth (%):** World Bank WDI (`NY.GDP.MKTP.KD.ZG`) & Ministry of Planning and Economic Development (2013–2024 actuals).
  * **Forward Projections (2025–2028):** World Bank *Macro Poverty Outlook* & International Monetary Fund (IMF) *World Economic Outlook* (October 2026 Baseline & Fragility Scenarios).
  * **Official Spot FX Rate (EGP/USD):** Central Bank of Egypt (CBE) historical period averages and institutional forward curves.
* **Data Governance:** Verified macro-level public institutional datasets; zero Personally Identifiable Information (PII) or proprietary financial records.

---

## 3. PROCESS: Data Engineering & Feature Pipeline

Raw institutional datasets were cleaned, standardized, and transformed into econometric variables using **Python (`pandas`, `numpy`)** and **Google BigQuery SQL**:

* **Data Cleaning & Normalization:** Standardized historical actuals and extracted deterministic midpoint values for projected institutional forecast intervals (2023–2028).
* **Logarithmic FX Delta ($\Delta \ln \text{FX}_t$):** Computed compounded annual currency depreciation via SQL `LAG()` window functions:
  $$\Delta \ln \text{FX}_t = \left[\ln(\text{FX}_t) - \ln(\text{FX}_{t-1})\right] \times 100$$
* **Domestic Output Gap ($\text{gap}_t$):** Isolated domestic demand pressure by subtracting Egypt's **4.5% structural potential growth benchmark** from actual Real GDP growth:
  $$\text{gap}_t = g^{\text{actual}}_t - 4.5\%$$
* **Inflation Inertia ($\pi_{t-1}$):** Engineered 1-year lagged CPI values via `LAG(cpi, 1)` to capture backward-looking price stickiness and adaptive expectations.
* **Rebased Cumulative Price Index:** Scaled cumulative CPI compounding against Base Year 2013 ($\text{Index} = 100$) using SQL `FIRST_VALUE()`.

---

## 4. ANALYZE: Econometric Specification & OLS Results

### Econometric Specification (Open-Economy Hybrid NKPC)
$$\pi_t = \beta_0 + \beta_1 \pi_{t-1} + \beta_2 \text{gap}_t + \beta_3 \Delta \ln \text{FX}_t + \varepsilon_t$$

### Estimated OLS Equation
$$\hat{\pi}_t = 5.00 + 0.303(\pi_{t-1}) + 0.131(\text{gap}_t) + 0.387(\Delta \ln \text{FX}_t)$$

### Parameter Attribution & Statistical Significance

| Variable | Parameter | Coefficient | $t$-Statistic | $p$-Value | Econometric Interpretation |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Constant (Intercept)** | $\beta_0$ | `5.000` | `3.12` | `0.010` | Structural baseline inflation friction (~5.0 pp floor). |
| **Inflation Inertia ($\pi_{t-1}$)** | $\beta_1$ | `0.303` | `5.89` | `< 0.001` | **Statistically Significant:** Adaptive expectations carryover from $t-1$. |
| **Output Gap ($\text{gap}_t$)** | $\beta_2$ | `0.131` | `0.15` | `0.675` | **Statistical Noise:** Domestic demand slack is statistically insignificant. |
| **FX Pass-Through ($\Delta \ln \text{FX}_t$)** | $\beta_3$ | `0.387` | `7.90` | `< 0.001` | **Decisive Driver:** A 10% EGP depreciation adds **+3.87 pp** to headline CPI. |

### Model Diagnostics & Correlation Matrix

| Diagnostic Metric | Result | Institutional Assessment |
| :--- | :---: | :--- |
| **Coefficient of Determination ($R^2$)** | `0.922` | Model explains **92.2%** of historical CPI inflation variance. |
| **Durbin-Watson Statistic** | `2.09` | Confirms the absence of first-order serial correlation in residuals. |
| **Correlation: $\text{Corr}(\text{CPI}, \Delta \ln \text{FX})$** | `+0.92` | Strong positive co-movement between currency depreciation and CPI surges. |
| **Correlation: $\text{Corr}(\text{CPI}, \text{GDP Growth})$** | `-0.08` | Confirms domestic demand overheating does not drive inflation spikes. |

---

## 5. SHARE: Visual Evidence & Regime Validation

### Visual 1 — Model Predictive Validity (2014–2028)

<img width="1600" height="800" alt="case_study_fig2_attribution" src="https://github.com/user-attachments/assets/5f7063ff-f265-4971-8df5-43d2d6cf7568" />

* **High Predictive Accuracy ($R^2 = 0.922$):** The Hybrid NKPC model closely tracks actual headline CPI across multiple macroeconomic regimes.
* **Devaluation Shock Alignment:** Accurately captures both the **2017 floatation peak** (`31.46%` modeled vs. `29.50%` actual) and the **2024 FX realignment** (`30.56%` modeled vs. `30.95%` actual).
* **2025–2027 Policy Divergence:** The spread between modeled CPI (`10.1%–15.6%`) and institutional forecasts (`13.3%–20.4%`) reflects scheduled administrative energy and fuel subsidy removals.

### Visual 2 — Structural Attribution (ERPT vs. Output Gap)

<img width="1600" height="800" alt="case_study_fig1_fit" src="https://github.com/user-attachments/assets/a491df90-1de2-4125-867b-a543fadb85d0" />

* **Exchange Rate Dominance ($\text{Corr} = +0.92$):** Severe FX depreciation spikes (orange bars) in `2016–2017` and `2022–2024` directly trigger the sharp surges in headline CPI (red line).
* **Output Gap Noise ($\text{Corr} = -0.08$):** Domestic output gap fluctuations (grey bars) remain flat near zero, confirming that domestic demand overheating does not drive Egyptian inflation.
* **Core Analytical Proof:** Egyptian inflation is structurally an **imported cost-push shock** rather than a demand-pull phenomenon.

---

## 6. ACT: Macroeconomic Policy Recommendations & Strategic Conclusions

1. **Pivot Away from Demand-Suppression Tightening:**
   Because the output gap is statistical noise ($t = 0.15, p = 0.675$), aggressive policy rate hikes erode real GDP growth without lowering core inflation.
2. **Escape the External Rentier Trap:**
   End structural reliance on exogenous, geography-linked FX inflows (Suez Canal transit tolls, worker remittances, tourism receipts, and portfolio "hot money") that expose the exchange rate to sudden external shocks ($\beta_3 = 0.387, t = 7.90$).
3. **Reallocate Capital from Non-Tradable Sectors:**
   Reduce capital concentration in non-tradable domestic sectors (such as speculative real estate and construction) that absorb foreign currency without generating export receipts, redirecting credit toward tradable manufacturing and agriculture.
4. **Restructure Sovereign Debt Tenors:**
   Transition government borrowing from high-yield short-term Treasury bills to longer-maturity instruments to contain fiscal deficit expansion during tightening cycles.
5. **De-Risk Import Supply Chains:**
   Incentivize domestic intermediate production to lower import elasticity and insulate consumer prices from global shipping and commodity disruptions.