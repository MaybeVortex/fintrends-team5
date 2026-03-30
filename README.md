# Analyzing Financial Market Trends in R

## Team 5

## Team Members

| Name | Roll Number |
|------|-------------|
| M.Nivien | 2023BCS0054 |
| Shaun Alan Joseph | 2023BCD0006 |
| Venkat Jaswanth | 2023BCD0021 |

---
Link to Github Repository: https://github.com/MaybeVortex/fintrends-team5.git

## Problem Statement

Financial markets generate vast amounts of time-series data daily, yet extracting meaningful trends and forecasting future price movements remains a challenging data mining problem. This project addresses the need to build a reproducible analytical pipeline in R that can fetch, clean, analyze, and forecast stock market data — transforming raw OHLCV price data into actionable insights using classical data warehousing and mining techniques.

---

## Objectives

- Fetch and preprocess real-world historical stock data from Yahoo Finance
- Perform exploratory analysis on return distributions and price behavior
- Detect market trends using Simple Moving Averages (SMA 20 & SMA 50)
- Quantify risk through rolling volatility estimation
- Measure inter-asset relationships using correlation analysis
- Forecast short-term price movements using an ARIMA model
- Visualize findings through both static (ggplot2) and interactive (Plotly) charts

---

## Dataset

**Source:** Yahoo Finance via the `quantmod` R package (`getSymbols()`)

**Stocks Covered:** Apple (AAPL), Google (GOOG), Microsoft (MSFT)

**Period:** January 1, 2020 — January 1, 2022

**Observations:** ~504 trading days per stock (~1,512 total rows across all three)

**Variables:** 6 per stock

| Variable | Description |
|----------|-------------|
| `Open` | Opening price of the trading session |
| `High` | Highest intraday price |
| `Low` | Lowest intraday price |
| `Close` | Official closing price |
| `Volume` | Total shares traded |
| `Adjusted` | Closing price adjusted for splits and dividends |

---

## Methodology

### Data Preprocessing
Raw OHLCV data is downloaded as an `xts` time-series object using `getSymbols()`. Prices are adjusted for stock splits and dividend payouts using `adjustOHLC()`. Missing values caused by trading holidays or data gaps are handled using `na.locf()`, which carries the last observed value forward.

### Exploratory Analysis
Daily returns are computed as the percentage change in adjusted closing price between consecutive trading days. A histogram is plotted to examine the shape, spread, skewness, and tail behavior of the return distribution across the full period.

### Models Used
- **Simple Moving Average (SMA):** 20-day and 50-day windows computed using `TTR::SMA()` to smooth price noise and identify trend direction and crossover signals.
- **Rolling Volatility:** 20-day rolling standard deviation computed using `TTR::runSD()` to estimate time-varying risk.
- **Pearson Correlation:** Pairwise correlation of daily log-returns across all three stocks to assess co-movement and diversification potential.
- **ARIMA:** `forecast::auto.arima()` automatically selects optimal (p, d, q) parameters via AIC minimization and generates a 30-day forward forecast with confidence intervals.

### Evaluation Methods
- ARIMA model fit assessed using AIC/BIC values from `auto.arima()`
- Forecast uncertainty quantified using 80% and 95% confidence intervals

---

## Results

| Metric | AAPL | GOOG | MSFT |
|--------|------|------|------|
| Start Price (USD) | $73 | $1,339 | $160 |
| End Price (USD) | $178 | $2,894 | $335 |
| Total 2-Year Return | +82% | +89% | +106% |
| Annualised Volatility | ~28% | ~24% | ~23% |
| Max Single-Day Return | +12.0% | +9.3% | +8.1% |
| Min Single-Day Return | −13.7% | −9.4% | −9.8% |
| AAPL–GOOG Correlation | 0.78 | — | — |
| AAPL–MSFT Correlation | 0.82 | — | — |
| GOOG–MSFT Correlation | — | 0.76 | — |

- Microsoft delivered the strongest overall return at ~106% over the two-year period.
- All three stocks experienced a volatility spike of 6–8× normal levels during the COVID-19 market shock in March 2020.
- High inter-stock correlation (0.76–0.82) confirms limited diversification benefit within the tech sector.
- ARIMA residual diagnostics confirmed a good model fit with white-noise errors for the AAPL forecast.

---

## Key Visualizations

### Return Distribution
Histogram of AAPL daily returns showing the approximately normal, fat-tailed distribution centered near zero.

### Closing Prices with Moving Averages
Line charts for each stock overlaying the raw adjusted close with the 20-day and 50-day SMA, illustrating Golden Cross and Death Cross signals.

### Rolling Volatility
Time-series plot of 20-day rolling standard deviation for all three stocks, highlighting the COVID-19 volatility spike in March 2020.

### Correlation Heatmap
Color-coded 3×3 heatmap of pairwise log-return correlations across AAPL, GOOG, and MSFT.

### ARIMA Forecast
30-day price forecast for AAPL with 80% and 95% confidence interval bands plotted against the most recent 120 days of historical prices.

### Interactive Candlestick Chart
Plotly candlestick chart for AAPL with hover tooltips, zoom/pan, and a date range slider.

> Plots are generated directly by running the R Markdown notebook. All figures are rendered inline in the knitted HTML output.

---

## Conclusion

This project demonstrated a complete data mining pipeline applied to real-world financial market data entirely within R. Moving averages successfully identified the strong and sustained upward trends across all three tech stocks throughout 2020–2021, while rolling volatility analysis captured the sharp systemic risk event caused by the COVID-19 pandemic in March 2020. Correlation analysis revealed that Apple, Google, and Microsoft move together too closely to provide meaningful portfolio diversification. The ARIMA forecast produced a reasonable short-term outlook for Apple's price with appropriate uncertainty bounds. Overall the project confirms that core data warehousing concepts — ETL pipelines, windowed aggregation, sequential pattern mining, and predictive modelling — translate directly and naturally into the financial domain.

---

## Contributions

| Name | Contributions |
|-------------|---------------|
| M.Nivien | Data preprocessing, missing value handling, exploratory analysis, return distribution, initial visualizations |
| Venkat Jaswanth | Moving average analysis, volatility modelling, correlation analysis, ARIMA model development and evaluation |
| Shaun Alan Joseph | Interactive Plotly visualization, results summary, report writing, README documentation |

---

## References

- Quantmod R Package — https://www.quantmod.com
- Yahoo Finance (data source) — https://finance.yahoo.com
- TTR: Technical Trading Rules — https://cran.r-project.org/package=TTR
- Hyndman R., Athanasopoulos G. — *Forecasting: Principles and Practice* — https://otexts.com/fpp3
- GeeksforGeeks — *Analyzing Financial Market Trends in R* — https://www.geeksforgeeks.org/r-machine-learning/analyzing-financial-market-trends-in-r/
- Markowitz, H. (1952) — *Portfolio Selection*, Journal of Finance, 7(1), 77–91

