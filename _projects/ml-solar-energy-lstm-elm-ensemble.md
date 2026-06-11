---
title: "ML: Solar Energy Prediction with LSTM, ELM, and Ensemble Models"
date: 2023-04-15
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - TensorFlow
  - Keras
  - LSTM
  - ELM
  - Ensemble Modeling
  - Bayesian Optimization
  - Scikit-learn
  - Statsmodels
  - Plotly
github: "https://github.com/mohamed1249/Solar-Energy-Prediction-using-LSTM-ELM-and-Ensemble-Models"
demo:
kaggle:
excerpt: "A solar energy forecasting project that preprocesses lagged solar time-series data, trains tuned LSTM and ELM neural models, combines them into an ensemble, and saves models, scalers, predictions, and plotting data for reuse."
---

## The Short Version

This project predicts solar energy from lagged solar time-series data using three neural forecasting approaches: LSTM, ELM, and an ensemble that combines both model outputs.

Compared with the earlier solar irradiance project, this one is more deployment-oriented. It separates preprocessing, modeling, and graph generation into different notebooks, then saves trained `.h5` models, scalers, prediction CSVs, and plotting data.

## Problem

Solar energy forecasting is useful only if the prediction workflow can be repeated. The goal was not just to train one model, but to build a reusable pipeline that can:

- Clean and prepare solar time-series data.
- Build lag features from historical solar values.
- Train multiple neural forecasting models.
- Compare their errors.
- Save model and scaler artifacts.
- Generate future prediction data for plots or dashboard use.

## Approach

**Data preparation** - The preprocessing notebook loads the solar dataset, drops unused columns, removes missing rows, parses `Date Time`, removes duplicate timestamps, sorts the series, and creates lagged features from the `Solar Avg` target.

**Lag selection** - ACF and PACF plots guide the time-series feature design. The final modeling table contains 7,638 rows with `Solar Avg` plus lags 1 through 13.

**Scaling and split** - Features and target values are scaled separately with `StandardScaler`, then split into chronological train/test sets to respect the time-series order.

**LSTM model** - A tuned LSTM model is built with Keras and optimized using Bayesian optimization over hidden units and learning rate. Early stopping is used during training.

**ELM model** - An Extreme Learning Machine style feedforward neural model is trained with a single hidden layer, tuned over hidden units, activation, and learning rate.

**Ensemble model** - The ensemble takes the outputs of the trained LSTM and ELM models, concatenates them, and learns a final dense output layer. This makes the ensemble a learned combination rather than a simple average.

**Saved artifacts** - The project saves `LSTM.h5`, `ELM.h5`, `Ens.h5`, `data_scaler.pkl`, `target_scaler.pkl`, prediction CSVs, and plotting data for later reuse.

## Result

The ensemble model performed best on the saved validation predictions.

| Model | MAE | RMSE | R2 |
|---|---:|---:|---:|
| LSTM | 57.70 | 131.78 | 0.850 |
| ELM | 61.08 | 131.60 | 0.850 |
| LSTM + ELM Ensemble | 55.45 | 117.75 | 0.880 |

The ensemble reduced both MAE and RMSE while improving R2, which suggests that the two base models captured slightly different parts of the solar pattern.

The notebook also reports MAPE, but because solar values frequently approach zero at night or low-light periods, percentage error becomes unstable and misleading. MAE, RMSE, and R2 are more useful for interpreting this project.

## What I Learned

This project made ensemble modeling feel practical rather than decorative. The ensemble improved because it learned how to blend two imperfect predictors, not because it was simply more complex.

It also showed the importance of packaging a modeling workflow. Saved scalers, saved `.h5` models, and plotting CSVs make the project easier to rerun and easier to connect to a dashboard or future prediction interface.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
