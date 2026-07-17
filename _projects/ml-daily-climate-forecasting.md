---
title: "ML: Daily Climate Time Series Forecasting"
excerpt: "A forecasting project for Delhi climate variables comparing date-feature KNN baselines, full-feature KNN models, and NeuralProphet time-series forecasts."
date: 2022-01-10
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - Scikit-learn
  - NeuralProphet
  - Plotly
  - Seaborn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-daily-climate-time-series-forecasting"
---

## The Short Version

A time series forecasting project that predicts four daily climate variables — mean temperature, humidity, wind speed, and atmospheric pressure — for Delhi, India. Three modeling approaches are compared: a date-only KNN baseline, a feature-rich KNN model, and NeuralProphet, a neural network model purpose-built for time series. The goal was to see how much improvement a dedicated forecasting model offers over a simple similarity-based approach.

## Problem

Can daily climate conditions be reliably predicted from historical patterns? And does using a time-series-aware model like NeuralProphet meaningfully outperform a general-purpose regression algorithm that treats forecasting as a plain supervised learning problem?

## Approach

The dataset (sourced from Kaggle) provides daily climate readings for Delhi split into train and test CSVs. After merging and sorting chronologically, the preprocessing involved:

- Extracting date features: year, month, day-of-month, and day-of-year.
- Removing outliers from `wind_speed` and `meanpressure` using IQR-based clipping — the two variables with the noisiest raw signals.
- Renaming the date column to `ds` to conform to NeuralProphet's expected input format.

Three models were then trained and evaluated against the held-out test set using R² score:

**Model 1 — KNN (date features only):** A `KNeighborsRegressor` (k=27) trained on year, month, day-of-month, and day-of-year. A minimal baseline that asks: how much signal is there in the calendar alone?

**Model 2 — KNN (all features):** The same KNN model retrained using all available climate variables as inputs, letting each variable inform predictions of the others.

**Model 3 — NeuralProphet:** A neural network forecasting model fitted separately for each climate variable using only the `ds`/`y` time series, with a daily frequency and a forecast horizon equal to the test set length.

## Result

- The date-only KNN was a weak baseline — calendar position alone captures seasonality but misses finer variation.
- Adding all climate features to KNN improved R² scores noticeably across all four variables, confirming that cross-variable correlation (e.g., temperature and humidity) carries real predictive signal.
- NeuralProphet produced the strongest visual fit for `meantemp` and `humidity`, tracking the seasonal cycles closely. Performance on `wind_speed` and `meanpressure` was more variable, likely due to their higher day-to-day noise even after outlier removal.

## What I Learned

The comparison between Model 1 and Model 2 is a useful reminder that the choice of features matters more than the choice of algorithm — swapping date-only inputs for full climate features on the same KNN improved results more than switching algorithms entirely. NeuralProphet's strength is capturing trend and seasonality automatically, but that advantage is most visible on smooth, cyclical signals like temperature; noisier variables benefit less. Outlier removal before modeling — not after — turned out to be a meaningful preprocessing decision for the two noisy variables.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
