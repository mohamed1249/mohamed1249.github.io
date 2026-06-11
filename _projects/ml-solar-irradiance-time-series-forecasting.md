---
title: "ML: Solar Irradiance Tracking with Time Series Forecasting"
date: 2023-03-11
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - LSTM
  - Bidirectional LSTM
  - KNN
  - SVR
  - ARIMA
  - Scikit-learn
  - Keras
  - Statsmodels
  - GridSearchCV
github: "https://github.com/mohamed1249/Solar-Irradiance-Tracking-using-Time-Series-Forecasting"
demo:
kaggle:
excerpt: "A time-series forecasting project that predicts next-hour solar irradiance from hourly weather and irradiance data, comparing LSTM, bidirectional LSTM, KNN, SVR, and ARIMA models with saved model artifacts."
---

## The Short Version

This project predicts next-hour solar irradiance using hourly weather and irradiance data. The work compares several forecasting approaches, including LSTM, bidirectional LSTM, KNN, SVR, and ARIMA, then evaluates them with MAE, MSE, RMSE, and R2.

The original brief also asked for plots suitable for a Node-RED dashboard workflow, so the notebook focuses heavily on visual comparison between actual and predicted irradiance.

## Problem

Solar irradiance changes with daily cycles, weather, humidity, wind, cloud conditions, and the sun's position. The goal was to forecast the next irradiance value from historical hourly readings so a dashboard could show both recent observed values and the coming forecast.

The target variable was `AllSky_Irr (Wh/m^2)`, the all-sky solar irradiance measurement.

## Approach

**Data** - The project uses an hourly 2019 dataset with 8,760 rows and 13 columns, including temperature, specific humidity, relative humidity, precipitation, pressure, wind speed and direction, all-sky irradiance, clear-sky irradiance, and zenith angle.

**Time-series features** - The improved notebook builds a solar time series and uses ACF/PACF analysis to identify correlated lags. Lags 1, 2, 10, 11, 12, 13, 23, 24, and 25 are used as model features.

**Preprocessing** - The data is scaled, split chronologically without shuffling, and evaluated on the final 10% of the time series to preserve the temporal order.

**Models compared** - The notebook trains and compares:

- LSTM.
- Bidirectional LSTM.
- KNN regressor with grid search.
- SVR with grid search.
- ARIMA.

Saved model artifacts include `KNN.pickle`, `svr.pickle`, and `LSTM.pickle`.

## Result

The strongest models were SVR and bidirectional LSTM, with all major models producing high R2 scores on the validation period.

| Model | MAE | RMSE | R2 |
|---|---:|---:|---:|
| LSTM | 22.06 | 33.24 | 0.986 |
| Bidirectional LSTM | 18.30 | 29.60 | 0.989 |
| KNN | 16.28 | 32.05 | 0.987 |
| SVR | 19.61 | 29.72 | 0.989 |
| ARIMA | 27.43 | 37.83 | 0.982 |

KNN achieved the lowest MAE, while SVR and bidirectional LSTM produced the strongest balance of RMSE and R2. ARIMA worked as a useful statistical baseline but did not outperform the machine learning models.

## What I Learned

This project showed how much solar forecasting depends on the right lag structure. The model choice mattered, but the signal became much clearer after turning the hourly series into supervised lag features.

It also reinforced that deep learning is not automatically the winner. A tuned SVR and KNN were very competitive against LSTM models, which made the comparison more useful than a single-model notebook.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
