---
title: "EDA + ML: Store Sales Time Series Forecasting"
date: 2022-11-27
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - Scikit-learn
  - Plotly
  - Seaborn
  - SciPy
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/eda-ml-store-sales-analysis-forecasting"
excerpt: "A multi-dataset time series EDA and forecasting entry for Kaggle's Store Sales competition — joining five data sources, profiling oil prices and transactions for seasonality, and forecasting Ecuadorian grocery sales with a Linear Regression baseline."
---

## The Short Version

A Kaggle competition entry for Store Sales — Time Series Forecasting, using grocery sales data from a major Ecuadorian retailer. Five data sources (sales, transactions, oil prices, holidays, and store metadata) are loaded, cleaned, and explored. The EDA investigates seasonality in both oil prices and transaction volumes using seasonal plots and periodograms before merging everything into a modeling dataset. The forecasting model is a Linear Regression baseline trained on product family and date features.

## Problem

Predict 15 days of future sales across 54 stores and 33 product families for an Ecuadorian grocery chain — a large-scale multi-series forecasting problem where external signals (oil prices, national holidays) are known to influence sales patterns in an oil-dependent economy.

## Approach

**Five datasets joined and explored independently before modeling:**

- **Holidays** — profiled by transfer status, type (Holiday/Event/Bridge/Work Day), and locale (National/Regional/Local). Transferred holidays were filtered out (they fall on the calendar date but are not actually celebrated), and the cleaned set was used to engineer a binary `holiday` feature.
- **Oil prices** — plotted as a raw time series and with a 365-day rolling average to expose trend. A full seasonality analysis was run: weekly seasonal plot, annual seasonal plot, and a periodogram (using `scipy.signal.periodogram`) to identify dominant frequency components.
- **Transactions** — same treatment: 365-day moving average for trend, per-store average bar chart, and a seasonality/periodogram analysis to check for weekly and annual patterns.
- **Train data** — sales aggregated by date and joined with oil and transactions; sales trends plotted with a rolling average; average sales by store and by product family charted; promotion levels by family visualized; full correlation heatmap of the merged features.

**Feature engineering for modeling:**
- `holiday` flag (binary, from cleaned holidays table).
- `dcoilwtico` (daily oil price, joined on date, back-filled for missing days).
- Date decomposition: `year`, `month`, `monthday`, `weekday`.

**Preprocessing pipeline** — A scikit-learn `Pipeline` with `SimpleImputer` → `OneHotEncoder` on the `family` column (33 product categories). All other features were dropped via `remainder='drop'`, making family the sole categorical predictor in the final model.

**Model** — Linear Regression trained on the OHE-encoded family features, retrained on the full training set before generating test predictions.

## Result

The Linear Regression model established a MAE baseline on the validation set. The model captures average sales levels per product family but cannot model temporal dynamics or cross-feature interactions — by design, as a competition entry baseline. The periodogram analysis of oil prices showed no strong dominant frequency, confirming that oil price variation is largely trend-driven rather than seasonal. Transactions showed a clear weekly seasonality pattern, with lower volumes on weekends — visible in both the seasonal plot and periodogram peaks at 52 cycles/year (weekly frequency).

## What I Learned

This project is an honest baseline — and the write-up treats it as such. The most instructive part isn't the model but the EDA infrastructure: the reusable `seasonality()` function that combines grouped aggregation, seasonal line plots, and a log-scale periodogram into a single call is a pattern worth carrying forward. Applying it to both oil prices and transactions before touching the sales data revealed that these two external series have fundamentally different temporal structures — oil is trend-dominated, transactions are seasonality-dominated — which informs what features a stronger model would need. The decision to `remainder='drop'` in the `ColumnTransformer` rather than pass numeric features through was the main limitation of the final model; including oil price, holiday flag, and date decomposition features alongside the OHE family columns would be the first improvement in a next iteration.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
