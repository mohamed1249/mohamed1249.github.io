---
title: "ML: E-Commerce Sales Forecasting & Customer Clustering"
date: 2022-02-07
status: "Completed"
tools:
  - Python
  - Pandas
  - Scikit-learn
  - CatBoost
  - Plotly
  - Seaborn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-e-commerce-data-forecasting-clustering"
excerpt: "A dual-objective ML project on a UK retail dataset — forecasting daily order quantities with three regression models and segmenting customers into behavioral personas using K-Means clustering."
---

## The Short Version

A two-part machine learning project on a real UK e-commerce transaction dataset. The first half benchmarks three regression models — KNN, Random Forest, and CatBoost — for forecasting daily product order quantities. The second half builds an RFM-style customer segmentation using K-Means, profiling each cluster by spending frequency and monetary value. Both parts share a common, carefully cleaned base dataset.

## Problem

Two practical business questions: can daily sales volume be forecast from date and product features alone? And which distinct customer types does this retailer actually serve — and how differently do they behave?

## Approach

**Preprocessing** was substantial. The raw transaction data contained cancelled orders (flagged by a "C" prefix on InvoiceNo), extreme outliers in Quantity and UnitPrice, ~25% missing CustomerIDs, hidden "nan" strings in the Description field, and non-standard StockCodes. Each issue was addressed explicitly before any modeling began — cancelled orders were removed by filtering for positive Revenue; IQR-based outlier removal was applied twice (once on raw data, once on the daily-aggregated dataset); stock codes were filtered to only well-formed 5-digit numeric entries.

**Forecasting** — Transactions were aggregated by date and StockCode to create a daily quantity dataset, then split 80/20 without shuffling (preserving time order). Three models were trained and compared by MSE:

- **KNN Regressor** — k tuned via `GridSearchCV` across k=3 to 99, selecting k=47.
- **Random Forest Regressor** — 100 trees, max depth 20.
- **CatBoost Regressor** — 1,000 iterations with early stopping (`od_wait=40`), depth 4, L2 regularization, and `has_time=True` to respect the temporal ordering of the data.

CatBoost outperformed the other two. Predictions vs. actuals were plotted as overlaid line charts for visual validation.

**Clustering** — A per-customer feature table was built by aggregating transaction data: order frequency (transaction count), monetary value (total revenue), min/max/mean revenue per order, total quantity, and country. After another round of outlier removal, the elbow method identified **k=3** as the natural cluster count. Each cluster was profiled by re-merging with the full transaction data and computing descriptive statistics.

## Result

CatBoost won the forecasting comparison, producing a closer fit to the validation time series than either KNN or Random Forest — the temporal structure of the problem (`has_time=True`) and its handling of date-derived features gave it an edge.

The three customer clusters broke down as follows:

| Cluster | Avg. Frequency | Avg. Monetary Value | Avg. Order Revenue | Profile |
|---|---|---|---|---|
| 1 | 39 orders | £260 | £10.34 | Low-engagement, occasional buyers |
| 3 | 77 orders | £709 | £12.45 | Mid-tier, regular customers |
| 2 | 97 orders | £1,496 | £16.99 | High-value, high-frequency loyalists |

The three groups form a clear loyalty ladder — each step up roughly doubles total spend and increases order frequency by ~60%.

## What I Learned

Running GridSearchCV to tune KNN's k parameter — rather than guessing — is a small but meaningful discipline; k=47 would not have been an intuitive starting point. CatBoost's `has_time=True` flag is an easy win on time-ordered data that's often overlooked — it signals to the model that the training data is not i.i.d., which matters for any sequence-dependent pattern. On the clustering side, building a proper per-customer aggregate table (rather than clustering on raw transactions) was the key design decision — clustering transaction rows would have conflated high-frequency customers with multiple low-value rows, obscuring the behavioral signal entirely.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
