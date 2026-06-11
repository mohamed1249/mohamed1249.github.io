---
title: "ML: Incidents Prediction with XGBoost"
date: 2023-03-28
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - XGBoost
  - Scikit-learn
  - GridSearchCV
  - ELI5
  - Statsmodels
  - Plotly
  - Seaborn
  - Parquet
  - Excel
github: "https://github.com/mohamed1249/Incidents-Prediction-with-XGBoost"
demo:
kaggle:
excerpt: "A machine learning project that forecasts daily incident volume and predicts incident severity levels from joined incident, detainee, and profile datasets using XGBoost models and exported Excel prediction reports."
---

## The Short Version

This project uses XGBoost to predict incidents from operational detention data. It has two connected parts: forecasting the daily number of incidents over time, and predicting incident severity levels from joined incident, detainee, and profile records.

The final workflow exports Excel prediction files so the model output can be reviewed outside the notebook.

## Problem

Incident data is messy, relational, and time-dependent. A useful model needs to answer more than one question:

- How many incidents are likely to happen on a given day?
- Can the expected incident levels be estimated?
- Can individual records be classified by incident severity?
- Which features appear to matter most for those predictions?

## Approach

**Data sources** - The project works with multiple operational datasets, including incident reports, incident-detainee links, detainee profiles, individual management plans, and census exports. The main incident report table contains about 103,000 records, while the incident-detainee link table contains about 121,000 records.

**Incident-count forecasting** - The notebook converts `DateOccured` into daily time features, groups incidents by date, cleans unreliable date ranges and outliers, and creates lag features from autocorrelation and partial autocorrelation analysis. Lags 1, 6, 7, and 8 are used as important signals.

**XGBoost regression** - An `XGBRegressor` is tuned with `GridSearchCV` and evaluated on a time-based validation split. After feature-importance analysis, the model is retrained on a smaller feature set including `dayofweek` and selected lag features.

**Incident-level prediction** - The project joins detainee profile data, incident participant data, and incident report data. It cleans categorical fields, drops high-cardinality columns, groups data by day, and predicts counts for critical, major, and minor incident levels.

**Severity classification** - A separate notebook frames incident level as a classification problem using `XGBClassifier`, label encoding, preprocessing pipelines, confusion matrices, and permutation importance.

## Result

For daily incident-count forecasting, the feature-engineered XGBoost model improved from:

| Version | MAE | RMSE | R2 |
|---|---:|---:|---:|
| Initial XGBoost | 4.80 | 5.82 | 0.11 |
| Feature-engineered XGBoost | 4.37 | 5.48 | 0.21 |

The saved daily prediction spreadsheet contains 216 forecast rows, with a rounded prediction MAE of about 4.31 incidents.

For individual incident-level classification, the XGBoost model reached about 77.1% accuracy after feature selection.

The date decomposition models were less reliable: year prediction reached about 46%, month about 16%, and day-of-month about 6%. That made the incident level and daily volume outputs more useful than trying to predict exact incident dates at an individual-record level.

## What I Learned

This project taught me to separate useful prediction from tempting prediction. Daily incident volume and incident severity can provide real planning value, while exact individual date prediction was too noisy to trust.

It also reinforced the importance of feature engineering for time-aware tabular models. The best improvement did not come from making the model more complicated; it came from using the right lag features and removing weak or noisy inputs.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
