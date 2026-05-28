---
title: "EDA + ML: Google Play Store App Analysis & Rating Prediction"
date: 2022-06-08
status: "Completed"
tools:
  - Python
  - Pandas
  - Scikit-learn
  - XGBoost
  - eli5
  - Plotly
  - Seaborn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/eda-ml-google-play-store-apps"
excerpt: "A dual-dataset EDA of 10,000+ Google Play apps and their user reviews — with a custom composite popularity score, sentiment-app feature cross-analysis, and a three-model regression benchmark for predicting app ratings."
---

## The Short Version

A full-pipeline project on the Google Play Store dataset — two CSVs (app metadata and user reviews), merged and analyzed together. The EDA half uses a nine-function reusable library to answer 25+ questions about categories, ratings, installs, pricing, size, update frequency, and user sentiment. The ML half benchmarks three regression models for predicting app ratings, with permutation importance-based feature selection applied after model comparison. Includes a custom popularity score engineered from three metrics — the same formula pattern used in the Goodreads project, applied to a new domain.

## Problem

What makes an app popular on the Play Store — and can that popularity be quantified fairly? And separately: given an app's metadata, can a model predict its rating well enough to be useful?

## Approach

**Cleaning** — The raw dataset had numerous encoding issues: `Installs` contained `+` and `,` characters and a rogue `"Free"` string entry; `Size` mixed MB (`M`) and KB (`k`) units and `"Varies with device"` strings; `Price` had `$` signs; all required type conversion after cleaning. Duplicated app entries were dropped on `App` name. `Current Ver` and `Android Ver` were dropped as uninformative.

**Nine reusable EDA functions** — built upfront and used throughout:
- `most_frequent()` — top-20 value counts bar chart.
- `dist()` — histogram with mean and mode printed, optional log scale and color split.
- `most_app()` — top-N apps ranked by a numeric column.
- `most_by()` — grouped mean of a metric by a categorical column.
- `most_for_each()` — grouped count of two categorical columns, plotted as a grouped histogram.
- `relations()` — Plotly scatter matrix across multiple numeric columns, colored by a categorical variable.
- `give_me_tops()` — returns top-N most frequent items in a column, optionally per group.
- `percentages()` — Plotly sunburst chart for co-occurrence across categorical columns.
- `most_comp()` — repetition counter for semicolon-separated multi-value columns (used for `Genres`).

**Custom `Popularity` score** — Same design philosophy as the Goodreads `real_best_books` score: ratings cluster around 4–5, reviews range to millions, installs range to billions — incompatible scales. Solved by multiplying `Rating × 10⁷ × Reviews × 10² × Installs / 10¹⁸`, balancing all three signals into a single comparable score. Result: Facebook, WhatsApp, and Instagram top the list — a sanity check that confirms the formula works.

**User reviews dataset** — Merged with app metadata after independent exploration. Sentiment counts (Positive/Neutral/Negative) were OHE-encoded with `pd.get_dummies`, then aggregated per app into percentage columns. A full scatter matrix across sentiment polarity, subjectivity, size, popularity, price, and sentiment percentages was plotted to surface cross-dataset relationships.

**ML — rating prediction** — Features: Category, Reviews, Size, Installs, Type, Price, Content Rating, and first genre (extracted from the semicolon-separated `Genres` column). Target: `Rating` (IQR outlier-filtered before splitting). Pipeline: KNNImputer + StandardScaler for numerics, SimpleImputer (most frequent) + OHE for categoricals. Three models compared by MAE and R²:

- **Linear Regression** — unregularized baseline.
- **KNN Regressor** — k and algorithm swept via `GridSearchCV` (k=3–99, four algorithm options).
- **XGBoost Regressor** — 1,000 estimators, max depth 10, subsample 0.7, colsample 0.8.

KNN was selected as the best model. Permutation importance (via `eli5`) was then applied on the validation set to rank features, and `SelectFromModel` with threshold=0.0006 was used to filter down to the most predictive subset. The KNN model was retrained on the reduced feature set and the final predictions vs. actuals were plotted as overlaid time series. The model was saved with `joblib`.

## Result

Key EDA findings:
- **Family** is the most common category by count; **Communication** apps have the highest average installs; **Social** apps dominate by review count.
- **Most popular apps** by composite score: Facebook, WhatsApp, Instagram, YouTube, Google Maps — the formula correctly surfaces universally-known high-usage apps.
- **Paid apps** consistently appear as outliers: low installs, low reviews, low negative sentiment — a coherent profile of niche, deliberate purchases.
- **Most apps are updated within 50 days** of the dataset's collection date; July is the peak update month; weekdays dominate over weekends.
- **Sentiment polarity and subjectivity are strongly correlated** — apps with very positive language tend to also be more subjective in their reviews.
- **Apps with polarity < 0.02** accumulate high negative review percentages — a useful threshold for flagging poorly-received apps.

For ML: KNN outperformed both Linear Regression and XGBoost on rating prediction. Permutation importance-based feature selection further improved the KNN result by removing noise features, demonstrating that a smaller, well-chosen feature set beats the full set on this problem.

## What I Learned

The `most_comp()` function for multi-value columns — splitting on semicolons and aggregating counts across lists — solves a problem that breaks standard `value_counts()`, and writing it as a reusable function rather than inline code made it immediately applicable to the Genres column. The two-dataset merge structure is the most analytically powerful part of this project: neither dataset alone can answer questions like "do apps with high negative sentiment percentages have lower popularity?" — that requires joining on `App` and engineering the sentiment proportion columns first. The permutation importance post-selection step is also worth noting: XGBoost had more raw capacity than KNN but underperformed, suggesting the rating prediction problem is more local-structure-driven (KNN's strength) than globally-learnable from category/type/price features.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
