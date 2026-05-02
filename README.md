# Five Days of Fame
### The Half-Life of Google Trends Attention and Its Economic Payoff  
### for Stock Prediction in Companies with Divergent ESG Profiles

*M.Sc. Business Analytics Research Project — Dublin Business School, 2025*  
*Milcah M. Joseph*

---

## Overview

Can the online footprint of investor curiosity, and its sustainability-focused 
strands, move the needle on near-term equity forecasts?

This study tests whether Google Trends search-volume indices (SVI) add 
predictive power to stock-return models, and whether that power differs across 
companies with varying ESG profiles. Across 45 listed firms from the US, UK, 
and India, 740 weekly SVI series were generated and tested against stock returns 
using autoregressive models with exogenous inputs (AR-X), rigorous 
multiple-testing controls, and out-of-sample forecasting evaluation.

**Key finding:** Google Trends SVI improves one-week-ahead return forecasts for 
43 of 45 stocks tested. That edge decays within a single trading week. ESG 
rating does not predict where the largest forecasting gains arise.

---

## Research Question

> Does investor attention, proxied by Google Trends SVI, improve 
> weeks-ahead stock-return forecasts across multiple markets, and how does 
> the required model complexity vary across firms with varying ESG profiles?

---

## Data

| Dataset | Description |
|---|---|
| `ESG_Selected_Companies.csv` | 45 companies with FTSE Russell ESG ratings (2016-2025), stratified by country and ESG tier |
| `Trends_Keywords.csv` | 80 themed keywords across 6 categories (Economic, ESG, Global, Industrial, Political, Stock-related) plus 44 company name terms |
| `trends_raw_combined.csv` | Weekly SVI data for all keyword-region pairs, Jan 2020 - Dec 2024 |
| `stock_returns_selected_companies.csv` | Weekly returns from FactSet for all 45 companies, aligned to Sunday-Saturday weeks |
| `granger_results_broad_filter.csv` | Results of 128,682 AR-X Granger causality tests (primary analysis) |
| `significant_predict_20250503_1310.csv` | Significant predictors passing all secondary analysis gatekeeping criteria |

**Coverage:** US, UK, India | **Period:** 2021-2024 (post-COVID) | **Frequency:** Weekly  
**SVI Regions:** Australia, India, Singapore, United Kingdom, United States, Worldwide

*Stock data sourced from FactSet. Google Trends data collected via the Google 
Trends API.*

---

## Dataset Construction

The dataset underpinning this study was built entirely from scratch using two 
live APIs, requiring substantial ETL work across every stage of collection, 
alignment, and validation.

### Stock Returns (FactSet API)
- Continuous weekly OHLC and return data collected for 45 companies across 
  the US, UK, and India
- Companies selected through a multi-year FTSE Russell ESG screening process, 
  requiring 10 consecutive annual snapshots (2016-2025) per firm to filter out 
  score volatility
- Trading week end-dates vary by market and holiday calendar. All dates were 
  backdated to the Sunday opening of each week to align with Google Trends' 
  Sunday-Saturday convention, eliminating interpolation error

### Google Trends SVI (pytrends / Google Trends API)
- 124 keywords collected across 6 regions (Australia, India, Singapore, UK, 
  US, Worldwide), producing 744 SVI series and 740 usable weekly series in total
- Keywords designed and categorized manually into 6 thematic groups (Economic, 
  ESG, Global, Industrial, Political, Stock-related), informed by the literature 
  on search-based investor attention
- Raw SVI is normalized by Google on a 0-100 scale per query. Weekly change in 
  SVI was computed to align with the returns (change in price) framing

### Stationarity Enforcement
- Augmented Dickey-Fuller tests applied to all 785 numeric series (45 return 
  series and 740 SVI series), with lag length selected automatically by AIC
- 784 of 785 series were stationary at the 5% level. The single exception was 
  differenced once and re-tested (ADF p = 0.01)

---

## Methodology

The analysis follows the CRISP-DM framework and distinguishes pre-registered 
(confirmatory) from post-hoc (exploratory) steps throughout.

**Primary Analysis**
- AR-X model fitted for each of 45 stocks against each of 744 SVI variables
- Across 6 estimation windows (4y, 3y, four rolling 1y windows) and 8 lag orders
- 128,682 Granger causality tests, corrected with Benjamini-Hochberg FDR control

**Secondary Analysis**
- Top 5 SVI predictors per company fed into multi-term AR-X models
- Rolling-origin out-of-sample forecasting (80/20 train/test split)
- Four-hurdle gatekeeping: beat AR benchmark, beat naive by 10%, RMSE/volatility below 0.5, 
  and pass all prior weeks sequentially

**Validation**
- RMSE computed at 1, 2, 3, and 4-week horizons
- Volatility-normalized RMSE (RMSE/sigma) used as economic significance threshold
- Chi-square independence test for ESG vs. gatekeeping success

---

## Key Results

| Metric | Result |
|---|---|
| Granger tests run | 128,682 |
| Passed joint significance (q < 0.05, F > 5) | 704 (0.55%) |
| Companies with at least 1 viable week-1 predictor | 43 / 45 |
| Models passing all 4 gatekeeping criteria at week 1 | 592 (22%) |
| Models surviving to week 4 | 13 |
| ESG vs. gatekeeping: chi-square result | chi-sq = 1.84, p = 0.40 (non-significant) |

**The dominant lag is 1 week.** Information in search queries is incorporated 
into prices within a single trading week, consistent with the attention-shock 
hypothesis (Da et al., 2011). Signal decays sharply beyond that horizon.

**ESG rating does not predict forecasting gains.** High-, medium-, and low-ESG 
firms show comparable proportions of successful models.

---

## Tech Stack

- **Python** (pandas, numpy, statsmodels, matplotlib, seaborn)
- **FactSet API** — stock return data
- **Google Trends API (pytrends)** — SVI collection
- **Jupyter / Google Colab** — analysis environment
- **Statistical methods:** ADF stationarity testing, Wald F-test, 
  Benjamini-Hochberg FDR correction, rolling-origin RMSE forecasting, 
  chi-square goodness-of-fit

---

## Repository Structure
```
├── data/
│   ├── raw/         # Source data (ESG ratings, keywords, SVI, stock returns)
│   └── results/     # Granger and forecasting output tables
├── notebooks/       # Analysis notebook
├── reports/         # Thesis PDF
└── README.md
```
---

## Full Paper & Code

The complete thesis, including all appendices, figures, and references, is available in
[Five_Days_of_Fame_Thesis.pdf](https://github.com/M1ck3yJ0/five-days-of-fame/blob/main/reports/Five_Days_of_Fame_Thesis.pdf)

The Jupyter Notebook, with complete code and interactive plotly charts, is available in [Jupyter Notebook](https://nbviewer.org/github/M1ck3yJ0/five-days-of-fame/blob/main/notebooks/five_days_of_fame_analysis.ipynb)

---

## Citation

Joseph, M. M. (2025). *Five Days of Fame: The Half-Life of Google Trends 
Attention and Its Economic Payoff for Stock Prediction in Companies with 
Divergent ESG Profiles.* M.Sc. Business Analytics, Dublin Business School.

## Acknowledgements

Thesis supervisor: Dr Richard O’Callaghan, Dublin Business School.

Analysis, methodology, and content are the author's own.<br>
ChatGPT (OpenAI) and Claude (Anthropic) were used for assistance with<br>
debugging code snippets, proofreading report drafts and README drafting.
