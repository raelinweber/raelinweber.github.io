# Gaussian Hidden Markov Model for S&P 500 Market Regime Detection

**Category:** Machine Learning · Financial Data Science · Time-Series Analysis  
**Institution:** Eduvos · ITDPA3-34  
**Project type:** Collaborative academic group project  
**Study period:** S&P 500 daily data, 2000–2024  
**Out-of-sample evaluation:** 2010–2024

## Overview

This group project investigated whether a Gaussian Hidden Markov Model (GHMM) could identify changing market conditions and act as a risk-management filter for two rule-based S&P 500 trading strategies.

The model was used for market-regime identification, not exact price forecasting. Its inferred stress probability controlled whether each strategy remained active or moved to cash for the next trading day.

## Research question

Does regime-based filtering improve the risk-adjusted performance of a 50/200-day simple moving-average (SMA) crossover and a 50-day breakout strategy compared with their unfiltered versions?

## Methodology

- Historical daily S&P 500 index data (^GSPC), covering 2000–2024.
- Three engineered inputs: 20-day realised volatility, RSI (14), and Rate of Change (14).
- Gaussian HMM with seven hidden states, selected using a held-out validation period.
- Hidden states ranked from calmer to more stressed using their mean volatility.
- Stress states defined using a training-period coverage rule; their probabilities were combined.
- When the combined stress probability reached 0.5 or higher, the strategy was switched off for the next trading day.
- Expanding-window annual walk-forward evaluation over 15 out-of-sample folds from 2010 to 2024, producing 3,748 out-of-sample daily returns.
- Scaling and model fitting used training data only; filtered probabilities used past information only.
- One-day execution delay and transaction costs of 5 basis points per position change.

## Results

The HMM filter reduced maximum drawdown for both strategies and improved the reported Sharpe ratio of both strategy variants. The strongest improvement was observed for the SMA crossover.

| Strategy | Sharpe, unfiltered | Sharpe, HMM-filtered | Max drawdown, unfiltered | Max drawdown, HMM-filtered |
|---|---:|---:|---:|---:|
| 50/200-day SMA crossover | 0.212 | **0.539** | -40.7% | **-20.8%** |
| 50-day breakout | 0.063 | **0.200** | -33.1% | **-25.9%** |

### Statistical checks and interpretation

- Buy-and-hold remained the stronger benchmark, with a reported Sharpe ratio of 0.758 and annualised return of 12.3%.
- The SMA HMM variant recorded a Probabilistic Sharpe Ratio (PSR) of 0.980.
- No strategy variant reached the stated 0.95 Deflated Sharpe Ratio (DSR) reliability threshold; the best reported DSR was 0.822.
- The placebo test supported the SMA result (p = 0.003); the breakout result was not significant at the 0.05 threshold (p = 0.070).
- The seven-state model's seed agreement was limited (ARI = 0.634), which is an important model-stability qualification.

**Conclusion:** The results suggest that the GHMM filter can help reduce downside exposure, particularly for the SMA crossover, but do not establish a consistently reliable trading edge. It did not outperform buy-and-hold, and the statistical and stability checks warrant caution.

## Tools and concepts

- Python
- pandas and NumPy
- hmmlearn (GaussianHMM)
- scikit-learn (StandardScaler)
- SciPy
- Matplotlib and Seaborn
- Jupyter Notebook
- Hidden Markov Models, feature engineering, time-series analysis, walk-forward validation, backtesting and statistical evaluation

## Collaboration

This was a collaborative academic group project completed by Group 3 for Eduvos module ITDPA3-34. I worked on the project with my group members across the research, implementation, experimentation and evaluation. The work and findings presented here are the group's project outcomes.

## Limitations

- The experiment covers one index and one historical evaluation period.
- The S&P 500 cash index is not directly tradable and excludes dividends.
- Transaction costs are simplified; market impact, borrowing costs and other real-world frictions are not fully represented.
- Both tested strategies are trend-following, so findings may not generalise to other strategy types or markets.
- The model's hidden states are statistical estimates, not verified economic labels.
- The seven-state model showed limited seed agreement, and the DSR did not meet the project's reliability threshold.

This is an academic historical simulation, not an investment recommendation or evidence of a live trading system.

## Reproducibility and project files

The public project page currently documents the research design and the final reported findings. The notebook and supporting data/figures are being cleaned and checked for consistency before publication. Historical market-data redistribution rights will also be considered before any dataset is included.

## Acknowledgements

Developed collaboratively by Eduvos ITDPA3-34 Group 3. The final academic submission is the source for the methodology and reported results.
