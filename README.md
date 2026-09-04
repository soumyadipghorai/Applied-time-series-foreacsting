# Time Series Forecasting

A set of teaching notebooks working through time series forecasting end to end — from exploratory analysis to ARIMA, multivariate models and evaluation. Each notebook states the intuition, gives the mathematics, and then verifies every claim against real data rather than asserting it.

## Notebooks

Each maps to a module in the study plan and is self-contained.

| Notebook | Covers | Data |
|---|---|---|
| [src/introduction.ipynb](src/introduction.ipynb) | Baselines, AutoARIMA, cross-validation, exogenous features, evaluation metrics — a `statsforecast` walkthrough | Bakery |
| [src/EDA.ipynb](src/EDA.ipynb) | The 10 core EDA plots: line, rolling stats, seasonal, box, histogram, density, lag, ACF, PACF, decomposition | Bakery |
| [src/feature_engineering.ipynb](src/feature_engineering.ipynb) | Lags, rolling/expanding windows, calendar features, log & Box-Cox, differencing, missing values and outliers | Bakery |
| [src/stationarity_diagnostics.ipynb](src/stationarity_diagnostics.ipynb) | Stationarity, white noise, random walks, unit roots, ADF/KPSS/PP, residual analysis, Ljung-Box | Simulated + bakery |
| [src/classical_forecasting.ipynb](src/classical_forecasting.ipynb) | Naïve, seasonal naïve, drift, SMA/WMA, SES, Holt, Holt-Winters, and a like-for-like comparison | NIFTY 50 |
| [src/arima_family.ipynb](src/arima_family.ipynb) | AR, MA, ARMA, ARIMA, SARIMA, SARIMAX, forecasting and prediction intervals | Simulated + bakery |
| [src/multivariate.ipynb](src/multivariate.ipynb) | VAR, lag selection, impulse responses, FEVD, VARMAX, Granger causality | 4 Indian indices |
| [src/model_selection.ipynb](src/model_selection.ipynb) | AIC/BIC/AICc, residual diagnostics, and MAE/RMSE/MAPE/sMAPE/CRPS | Simulated + bakery |
| [src/advanced_models.ipynb](src/advanced_models.ipynb) | ARCH, GARCH, Prophet, XGBoost with lag features, LSTM and transformers (conceptual), scored on all five metrics | NIFTY 50 + bakery |

## Data

| File | Contents |
|---|---|
| `data/daily_sales_french_bakery.csv` | Daily unit sales by product for a French bakery, 2021-01 → 2022-09. Long format (`unique_id`, `ds`, `y`, `unit_price`). Most notebooks use `TRADITIONAL BAGUETTE` — 637 days, strong weekly seasonality, and 37 closure days that make the missing-vs-zero distinction concrete. |
| `data/nifty50_daily.csv` | NIFTY 50 daily OHLCV, 2016-08 → 2026-08 (2465 rows). |
| `data/nifty_indices.csv` | Daily closes for NIFTY 50, NIFTY Bank, NIFTY IT and India VIX, 2016-08 → 2026-08 (2447 rows). |

The two market files were pulled from Yahoo Finance and committed, so the notebooks run offline.

## Running

```bash
python -m venv .venv
.venv/Scripts/activate          # Windows;  source .venv/bin/activate on macOS/Linux
pip install -r requirements.txt
jupyter lab                     # or open the folder in VS Code
```

Notebooks read data with relative paths (`../data/...`), so run them from `src/`.

Core dependencies: `pandas`, `numpy`, `matplotlib`, `scipy`, `statsmodels`, `statsforecast`, `utilsforecast`.

[src/advanced_models.ipynb](src/advanced_models.ipynb) additionally needs `arch`, `prophet`, `xgboost` and `scikit-learn`:

```bash
pip install arch prophet xgboost scikit-learn
```

> `requirements.txt` is UTF-16 encoded (a `pip freeze >` redirect from PowerShell). If `pip install -r` fails to parse it, regenerate with
> `.venv/Scripts/python -m pip freeze | Out-File -Encoding utf8 requirements.txt`.

## Conventions

- **Every number in the prose is computed, not asserted.** Each notebook was executed end to end before being committed, and the observations quote the actual output.
- **Findings are reported as they came out**, including the inconvenient ones — over-differencing that inflates variance, information criteria that prefer a model which forecasts worse, MAPE returning `inf` on a zero-sales day, and prediction intervals that miscalibrate in both directions.
- **Single-window results are treated as provisional.** Where a ranking matters it is re-scored over a rolling origin, which more than once reverses the conclusion.
