---
title: "ML: US Accidents Severity & Count Prediction"
date: 2022-03-14
status: "Completed"
tools:
  - Python
  - Pandas
  - Scikit-learn
  - XGBoost
  - Seaborn
  - Matplotlib
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-us-accidents"
excerpt: "Two ML models on 3M+ US accident records — a KNN classifier for accident severity and an XGBoost regressor for daily accident count — with a companion EDA notebook and a frank mid-notebook pivot when the first approach underperformed."
---

## The Short Version

A two-model ML project on the US Accidents dataset (2016–2021), building on a prior EDA notebook. The first model predicts accident severity (1–4) from location, weather, date, and road feature data. The second predicts the expected daily accident count from weather and date aggregates. The notebook is notable for a mid-project pivot: XGBoost regression on severity underperformed, so the task was reframed as classification — a transparent course correction that produced a significantly better result.

## Problem

Two questions: given an accident's circumstances (location, weather conditions, road features, time of day), can its severity be predicted? And given a date's weather profile, can the total number of accidents that day be forecast?

## Approach

**Feature engineering** — From the raw dataset, 25 columns were selected covering severity, GPS coordinates, weather metrics, boolean road features (junction, crossing, traffic signal, etc.), and timestamps. Start and End times were converted to a `duration` column (nanoseconds as float). Date components extracted: `year`, `month`, `day`, `dayofweek`, `hour`. Eight continuous columns with visible outliers were cleaned via IQR-based `remove_outliers()`. A correlation heatmap identified `Visibility` and `Precipitation` as uncorrelated with all other features — both were dropped before modeling.

**Preprocessing pipeline** — A three-branch `ColumnTransformer`:
- Numeric: `KNNImputer(n_neighbors=5)` → `StandardScaler`
- Categorical (`Side`): `SimpleImputer(most_frequent)` → `OneHotEncoder`
- Boolean (road features): `KNNImputer` → `OneHotEncoder`

Due to the 3M+ row dataset size, the preprocessor was fit on a 40,000-row training subsample and validated on 10,000 rows — an acknowledged compute constraint noted directly in the notebook.

**Model 1 — Severity prediction:**

*First attempt* — XGBoost Regressor (1,000 estimators, depth 10, η=0.1, subsampling). MAE and R² both unsatisfying. Mid-notebook diagnosis: treating 4-value ordinal severity as a continuous regression target is the wrong framing.

*Pivot* — KNN Classifier (k=21, uniform weights) on the same preprocessed features. Confusion matrix and accuracy score computed. The classifier produced a meaningfully better result — **"Much better!"** noted directly in the notebook.

**Model 2 — Daily accident count prediction:**

A second dataset was constructed by grouping the accident records by `date` using a custom `group_me_a_df()` function that computes mean weather metrics and adds a `count` column (accidents per day). This collapses 3M individual rows into ~2,000 date-level records — a fundamentally different modeling unit. XGBoost Regressor trained on the grouped data with the same configuration. MAE and R² both strong — the notebook's "WOW!" reaction confirms the task was well-matched to the model. Retrained on the full grouped dataset before saving.

Both models saved with `joblib`.

## Result

| Task | Model | Outcome |
|---|---|---|
| Severity classification | XGBoost Regression | Unsatisfying — wrong problem framing |
| Severity classification | KNN Classifier (k=21) | **"Much better!"** — improved accuracy |
| Daily accident count | XGBoost Regression | Strong R² — **"Pretty good score"** |

The second model was explicitly scheduled for future validation: *"retrain on full data and wait till next December to test it on the 2022 dataset update"* — a planned temporal holdout.

## What I Learned

The mid-notebook pivot is the most instructive moment: recognizing that severity (values 1, 2, 3, 4) is a classification target, not a regression target, and switching approaches rather than papering over the poor MAE, is the right instinct. The two problems in this notebook are actually very different in structure — individual accident severity is a sample-level classification problem with high variance; daily accident count is a smooth aggregate regression problem. The `group_me_a_df()` function that collapses row-level data into date-level aggregates is the key design decision for the second model: it transforms a noisy high-dimensional classification problem into a much cleaner regression problem where weather and calendar features can meaningfully explain the outcome. The compute constraint on the severity model (40K/10K subsample) is acknowledged honestly — it's a real limitation, and noting that it may have hurt performance is more useful than pretending the full dataset was used.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
