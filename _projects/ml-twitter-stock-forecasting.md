---
title: "ML: Twitter Stock Price Forecasting with ARIMA, SARIMA & NeuralProphet"
date: 2022-04-20
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - Statsmodels
  - NeuralProphet
  - SciPy
  - Plotly
  - Seaborn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-twitter-stock-price-forecasting"
excerpt: "A rigorous time series analysis of Twitter's stock price — stationarity testing, ACF/PACF-guided ARIMA order selection, SARIMA comparison, and an honest NeuralProphet failure documented and explained."
---

## The Short Version

A full classical time series analysis pipeline on Twitter's daily closing stock price, built on principled diagnostic steps before any modeling: smoothing, seasonal decomposition, stationarity testing via the Augmented Dickey-Fuller test, differencing, and ACF/PACF analysis to derive ARIMA orders from the data rather than guessing. Three models are compared — ARIMA, SARIMA, and NeuralProphet — with SARIMA winning on RMSE and NeuralProphet failing visibly, with the failure documented and reasoned through rather than quietly hidden.

## Problem

Can Twitter's stock price be forecast from its own historical series? And which classical vs. neural approach handles the non-stationary, weakly seasonal structure of daily stock prices better?

## Approach

**Exploratory analysis** — The raw closing price series was plotted alongside five moving averages (4, 6, 8, 12 months, and a 365-day centred trend). `statsmodels` seasonal decomposition (`model='additive'`, `period=30`) separated trend, seasonality, and residual components visually.

**Seasonality analysis** — The same reusable `seasonality()` function from the Store Sales project was applied here: weekly seasonal plot, annual seasonal plot, and a log-scale periodogram. The periodogram identified **semi-annual seasonality** as the dominant frequency component.

**Stationarity testing** — A `test_stationary()` function plotted rolling mean and rolling standard deviation alongside the original series, then ran the **Augmented Dickey-Fuller test** (ADF). The ADF p-value on the raw series was > 0.05 — non-stationary. After applying 12-period differencing (`tc.diff(periods=12)`), the p-value dropped to 0, confirming stationarity. All models were trained on the differenced series.

**ACF/PACF** — Autocorrelation and partial autocorrelation functions were plotted on the differenced series up to lag 30. All lags below 26 showed significance in the PACF — this informed the AR order (p) selection for ARIMA.

**Three models compared by RMSE on the same validation window (indices 700–998):**

- **ARIMA(23, 0, 0)** — AR order p=23 from the PACF analysis; d=0 since the series was already differenced; MA order q=0 as a starting point.
- **SARIMA(14, 0, 0) with trend='c'** — Seasonal ARIMA via `SARIMAX`, with a constant trend term. Slightly lower RMSE than ARIMA.
- **NeuralProphet** — Fitted on the differenced series at daily frequency.

A `predict()` function reconstructs actual future prices from the differenced forecasts by accumulating predicted differences from the last known price — correctly inverting the differencing transformation for real-world interpretability.

## Result

| Model | RMSE (validation) | 4-day forecast | Assessment |
|---|---|---|---|
| ARIMA(23,0,0) | Moderate | Plausible prices | Reasonable baseline |
| SARIMA(14,0,0)+'c' | **Lower (best)** | Plausible prices | Selected model |
| NeuralProphet | Very high | Implausible prices | Rejected |

SARIMA was selected as the final model. Its constant trend term gave it a slight edge over the pure AR ARIMA on this series. The four-day forecasts from both statistical models produced plausible stock price values after undifferencing.

NeuralProphet was a clear failure — its RMSE on the validation window was dramatically higher, and the undifferenced four-day forecasts produced values outside any plausible price range. The notebook's own commentary: *"Absolutely no! These can't even be stock prices"* — and the failure is attributed to NeuralProphet being better suited to series with strong, regular seasonality (like the climate project) than to noisy, weakly seasonal financial data.

## What I Learned

The diagnostic pipeline before modeling — ADF test → differencing → ADF retest → ACF/PACF — is not optional ceremony for time series work. Skipping it and fitting an ARIMA with guessed orders would have produced a model trained on a non-stationary series, which invalidates the statistical assumptions ARIMA relies on. Deriving p=23 from the PACF cutoff rather than a grid search is also a meaningful choice: it uses domain knowledge about the series structure instead of brute-forcing hyperparameters. The NeuralProphet failure is arguably the most instructive result in the notebook — it confirms that neural forecasting models are not universally superior to classical methods, and that the right model depends on the structure of the series, not the sophistication of the algorithm. Stock prices are close to random walks with weak seasonality; a well-specified SARIMA is a more honest fit than a neural model expecting learnable periodic patterns.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
