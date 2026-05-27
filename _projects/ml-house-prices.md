---
title: "ML: House Prices — Advanced Regression Techniques"
date: 2022-01-04
status: "Completed"
tools:
  - Python
  - Pandas
  - Scikit-learn
  - XGBoost
  - Seaborn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-house-prices"
excerpt: "My first ML project — a four-model regression benchmark on the Kaggle House Prices dataset, built one month into learning machine learning."
---

## The Short Version

The project that started everything. Built in my first month of learning ML, this notebook benchmarks four regression algorithms — KNN, XGBoost, Gradient Boosting, and Linear Regression — on the Kaggle House Prices dataset. It established the core workflow I've used in every regression project since: correlation analysis, a clean sklearn Pipeline with separate numeric and categorical transformers, k-sweep tuning for KNN, and retrain-on-full-data before final submission.

## Problem

The Kaggle House Prices competition: predict the sale price of residential homes in Ames, Iowa from 79 features covering lot size, neighborhood, quality ratings, garage type, basement finish, and dozens more. The challenge is the breadth — many features, mixed types, significant missing data, and high variance in the target.

## Approach

**EDA** — A full correlation heatmap across all numeric features, plus ranked lists of the top 20 positive and negative correlations with `SalePrice`. This identified `OverallQual`, `GrLivArea`, and `GarageCars` as the strongest predictors before any modeling.

**Preprocessing Pipeline** — A scikit-learn `ColumnTransformer` with two branches:
- Numeric: `KNNImputer` (k=5) → `StandardScaler`. KNN imputation was chosen over mean imputation to better preserve the local structure of missing values across correlated features.
- Categorical: `SimpleImputer` (most frequent) → `OneHotEncoder` (ignore unknown).

**Models trained and compared by MAE and R²:**

- **KNN Regressor** — k swept from 3 to 99 (odd values only) using a manual loop; best k=9 selected by minimum validation MAE.
- **XGBoost Regressor** — 1,000 estimators, max depth 10, learning rate 0.1, 70% row subsampling, 80% column subsampling.
- **Gradient Boosting Regressor** — 1,000 estimators, learning rate 0.04, 34% subsampling, squared error loss.
- **Linear Regression** — unregularized baseline.

After model selection, the winning model was retrained on the full training set (not just the 75% train split) before generating the competition submission.

## Result

Gradient Boosting Regressor outperformed the other three models and was selected for final submission. The lower learning rate (0.04) and aggressive subsampling (34%) acted as implicit regularization, giving it an edge over XGBoost's more aggressive settings on this dataset. Linear Regression served as a useful sanity check — its MAE and R² established the floor that all tree-based models comfortably beat. KNN (k=9) landed between Linear Regression and the boosting models.

The final model was retrained on the complete labeled dataset before predicting test prices, squeezing additional signal from the ~365 rows held out during validation.

## What I Learned

This project built the mental model I still use: preprocess in Pipelines (never leak), sweep hyperparameters systematically rather than guessing, and always retrain on the full dataset before submitting. The KNNImputer choice over `SimpleImputer` for numeric features was my first deliberate decision about *why* an imputation strategy fits a problem — correlated features like `GarageArea` and `GarageCars` carry mutual information that mean imputation ignores. Looking back, the XGBoost config was likely undertrained (no early stopping, aggressive depth setting) — something I fixed in later projects. But getting to a working four-model comparison with a clean preprocessing pipeline in month one set the foundation for everything that followed.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
