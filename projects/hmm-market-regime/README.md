# Gaussian Hidden Markov Model for S&P 500 Market-Regime Filtering

**Category:** Machine Learning · Financial Data Science · Time-Series Analysis  
**Institution:** Eduvos · ITDPA3-34  
**Project type:** Collaborative academic group project  
**Dataset:** S&P 500 daily OHLCV (`^GSPC`), 3 January 2000 – 30 December 2024

## Overview

This collaborative project investigated whether a Gaussian Hidden Markov Model (HMM) could identify latent market regimes and filter two predefined S&P 500 trading strategies: a 20/50-day simple moving-average (SMA) crossover and a 20-day breakout.

The HMM was used as a regime-based risk filter, not as a model for predicting exact future prices.

## Methodology

- Processed historical S&P 500 daily OHLCV data from 2000–2024.
- Engineered candidate inputs: realised volatility, SMA measures, RSI (14) and Rate of Change (14).
- Applied training-window-only feature correlation filtering at an absolute correlation threshold of 0.80.
- Standardised features using each training window only.
- Fitted diagonal-covariance Gaussian HMMs with 2, 3 and 4 candidate states; selected the candidate with the lowest Bayesian Information Criterion (BIC).
- Used 20 random initialisations per candidate state count, with a documented numerical transition-prior fallback.
- Identified favourable states separately for each strategy using training-only positive mean gross open-to-open returns on eligible long signals.
- Generated signals at close of day t, entered at the next session's open, and evaluated the holding period through the following open.
- Applied 5 basis points in transaction costs plus 2 basis points in slippage per position change (7 bps total).
- Used a rolling 31-observation training window. The final HMM train/test rows excluded the 2000–2005 pilot period.

## Final out-of-sample evaluation

The final run generated **4,749 regime predictions** from 16 February 2006 to 30 December 2024. It produced **4,747 next-open holding-period returns** from 17 February 2006 to 27 December 2024.

### Strategy results

| Strategy | Net cumulative return | Annualised Sharpe | Maximum drawdown |
|---|---:|---:|---:|
| SMA unfiltered | **128.992%** | **0.446** | **−29.432%** |
| SMA HMM-filtered | 21.577% | 0.166 | **−18.950%** |
| 20-day breakout unfiltered | **98.954%** | **0.396** | **−40.990%** |
| 20-day breakout HMM-filtered | 40.293% | 0.261 | **−29.462%** |

### Interpretation

The HMM filter reduced maximum drawdown for both strategies. However, the filtered strategies had lower net cumulative returns and lower annualised Sharpe ratios than their unfiltered counterparts. The filter therefore showed a narrower potential benefit in downside-risk mitigation, not an improvement in the study's main Sharpe-based performance criterion.

The final report includes a supplementary Probabilistic Sharpe Ratio assessment. A numerical Deflated Sharpe Ratio (DSR) was **not reported**, because a defensible effective number of independent performance-selection trials was not established.

## Validation and limitations

The final experiment's programmed checks passed for 4,749 prediction dates and 4,747 holding-period returns, including date ordering, training-window chronology, valid state probabilities, unchanged original signals, and independent reconciliation of positions, costs and equity calculations.

Important qualifications:

- The 31-observation fitting window follows the inclusive 31-day worked example in the earlier methodology; the earlier prose also refers to 30 days. This inconsistency is disclosed in the submitted report.
- Some fitted windows retained only one feature, and feature dimensions and hidden-state labels could vary between windows.
- High posterior probabilities do not establish that a hidden state is economically valid.
- The S&P 500 cash index is not directly investable; the backtest is a historical index-price simulation.
- Transaction costs and slippage are simplified and do not capture every execution friction.
- The results concern one index, one historical period and two strategies; they should not be assumed to generalise.

## Repository contents and running the notebook

- `HMM_Market_Regime_Detection.ipynb` — cleaned final submission notebook.
- `hmm_experiment_config_FINAL.json` — final experiment configuration (not the earlier exploratory configuration).
- `requirements.txt` — recorded core Python package versions.

**Required local inputs not included in this public repository yet:**

1. `hmm_dataset.csv` — the original S&P 500 OHLCV source file.
2. `final_report_outputs/methodology_aligned_predictions_strict.csv` — the authoritative 4,749-row final prediction checkpoint.

The notebook's default Run All path expects these files. Without them, it cannot reproduce the final tables and figures. The notebook includes an optional HMM retraining path, but that still requires the original dataset and is computationally expensive. The dataset and checkpoint have not been uploaded because their availability and redistribution permissions have not yet been confirmed.

To run once the required inputs are available, place the notebook, `hmm_dataset.csv` and `final_report_outputs/` in the same project directory, install `requirements.txt`, then run the notebook from top to bottom.

## Tools and concepts

Python · Jupyter Notebook · pandas · NumPy · scikit-learn · hmmlearn · SciPy · Matplotlib · Seaborn · Gaussian HMM · BIC model selection · feature engineering · time-series analysis · walk-forward validation · backtesting · statistical evaluation.

## Collaboration

This was a collaborative academic group project completed by Eduvos ITDPA3-34 Group 3. I worked with my group members across the research, implementation, experimentation and evaluation. The methodology, code, experiments and reported findings represent the group's collaborative work.

## Academic-use note

This is an academic historical simulation, not investment advice, a live trading system, or evidence of future profitability.
