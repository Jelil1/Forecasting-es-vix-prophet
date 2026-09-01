# Forecasting Equity Market Trends and Volatility with Meta's Prophet

Forecasting E-mini S&P 500 futures (ES) and the CBOE Volatility Index (VIX),
with out-of-sample validation against ARIMA and a naive random walk.

**[Read the full write-up →](https://jelil1.github.io/Forecasting-es-vix-prophet/)**

## Summary

Prophet models were fitted to 4,926 trading days of daily data (2007–2026) for
both series, then validated using rolling-origin cross-validation across 38 folds
at a 30-trading-day horizon.

**Result: neither Prophet nor ARIMA beat a naive random walk.**

| Series | Prophet RMSE vs naive | Directional accuracy |
|:---|---:|---:|
| ES  | 1.93x worse (p < 0.001) | 0.480 |
| VIX | 1.25x worse (p = 0.01)  | 0.552 (p = 0.26, not significant) |

ARIMA converged on the naive forecast rather than improving on it, which is the
expected behaviour on a near-random-walk series.

The ES/VIX relationship itself is real and quantified — the correlation of daily
log returns is −0.74 — but that descriptive finding did not translate into
predictive skill.

## Method

- Rolling-origin cross-validation, 38 folds, expanding training window
- Benchmarks: naive random walk and `auto.arima()` on log values
- Metrics: RMSE, MAE, directional accuracy, with paired t-tests on fold-level errors
- Explicit audit for silent fallbacks to the benchmark
- Tested whether Prophet's default yearly and weekly seasonality helped: disabling
  it improved RMSE by 1.2% on ES, but Prophet remained 1.9x worse than naive, so
  the defaults were not the cause of the underperformance
- Fixed data window so results are reproducible across runs

## Tools

R · Prophet · forecast · quantmod · ggplot2 · RMarkdown

## Files

- `index.html` — rendered write-up
- `.Rmd` — full source
