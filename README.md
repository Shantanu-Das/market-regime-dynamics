# Market Regime Dynamics

## Overview
Study of how the predictive power of simple stock-level signals changes with observable market
conditions. Four signals from the companion cross-sectional project (12-1 momentum, short-term
reversal, realised volatility, volume surprise) are evaluated conditional on a market volatility
regime defined only from information available at each forecast date, and a transfer experiment checks
whether signal weights calibrated in one environment carry over to another out of sample.

## Data
Daily prices from Yahoo Finance (via `yfinance`) for the current S&P 1500 constituents, 2004 to 2026,
giving about 298,000 stock-months across roughly 1,500 tickers. A separate longer SPY history from
1993 provides the market-state variables and a burn-in for the regime thresholds.

## Methodology
- Market state from SPY: 63-day realised volatility (primary), 200-day trend, and drawdown.
- Volatility regimes (low / mid / high) from expanding-window terciles of realised volatility using
  only history up to each date, so labels are known in real time and use no future information.
- Rank IC per signal with Newey-West (HAC) t-statistics, overall, rolling, and conditional on regime;
  regime differences tested by regressing the monthly IC series on regime dummies.
- Transfer experiment: IC-based signal weights calibrated on 2005-2015 (per regime and pooled),
  evaluated on 2016-2026, including a regime-conditional combination.
- Small beta-neutral decile long-short check linking the IC results to portfolio behaviour, with
  turnover and transaction-cost sensitivity.

## Results
- Signal predictiveness is regime-dependent. Momentum and the low-volatility effect are both
  concentrated in low-volatility markets and weaken or reverse when volatility is high.
- Momentum rank IC is about +0.03 (t around 2.2) in the low-volatility regime versus roughly -0.02 in
  the mid and high regimes. Realised volatility has an IC near -0.05 (t around -3.8) in the
  low-volatility regime, weakening toward zero and reversing sign in the high-volatility regime.
- Reversal and volume surprise show no meaningful regime dependence.
- In the transfer experiment, weights transfer best to the same regime they were calibrated in, and
  no calibration works in high-volatility evaluation months. Conditioning the combination on the
  regime raises out-of-sample IC from about 0.019 to 0.030, but the improvement is not statistically
  significant and does not survive transaction costs.
- Decile long-short portfolios confirm the pattern (for example momentum earns positive returns in
  low-volatility months and loses in high-volatility months).

Full numbers, tables, and figures are in the executed notebook and under `figures/`.

## Reproducibility
1. Python 3.11 or newer is recommended; the notebook was developed and run on Python 3.14.
2. Create an environment and install dependencies:
   ```
   python -m venv .venv
   .venv\Scripts\activate      # Windows
   source .venv/bin/activate   # macOS / Linux
   pip install -r requirements.txt
   ```
3. Launch Jupyter from the repository root:
   ```
   jupyter notebook
   ```
4. Open `market_regime_analysis.ipynb` and run all cells.

## Limitations
- Survivorship bias: the universe is the current S&P 1500, and it is not neutral across regimes.
  Stocks that fell hard in a high-volatility episode and are still in the index necessarily recovered,
  which most affects the high-volatility regime results, so the sign reversal of the low-volatility
  effect there should be read with caution.
- Multiple testing: several regime-difference tests are run; the two highlighted effects were the
  pre-specified hypotheses, and individual t-statistics are not adjusted for the number of tests. The
  momentum effect weakens under alternative regime definitions.
- The high-volatility regime is a minority of months concentrated in a few episodes (2008-2009, 2011,
  2020, 2022), so its statistics lean on a small number of distinct events.
- State variables come from SPY only; the whole study is one 21-year sample of the US large- and
  mid-cap market. The transfer split is fixed and the conditional improvement is not significant.
