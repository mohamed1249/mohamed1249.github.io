---
title: "ML: Predictive Analytics for Work Orders"
date: 2023-09-04
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - XGBoost
  - Linear Regression
  - Hybrid Modeling
  - Scikit-learn
  - Joblib
  - Power BI
  - Parquet
  - Excel
github: "https://github.com/mohamed1249/Predictive-Analytics-for-Work-Orders"
demo:
kaggle:
excerpt: "A large operational forecasting project that predicts work-order volume, facility demand, location and escort risk, and duration bins from work-order, detainee, and census data using XGBoost, linear models, and deployment-ready outputs."
---

## The Short Version

This project builds a predictive analytics pipeline for operational work orders. It combines work-order tasks, detainee profile records, PID records, and daily census exports to forecast future work-order demand and estimate the operational risk and duration of those work orders.

The project grew into a deployment-style workflow: saved models, feature columns, encoders, future prediction data, Excel outputs, CSV outputs, and Power BI dashboards.

## Problem

Operational teams need to plan staffing and workload before the work arrives. The core questions were:

- How many work orders should be expected in the next 14 days?
- How will that demand split across facilities?
- How many work orders should each facility expect?
- What location risk, escort risk, and duration bin should be expected for generated work orders?
- Can the results be exported into a dashboard-friendly table?

## Approach

**Data preparation** - The workflow joins `Workorder_Tasks.parquet` and `Workorder_PIDs.parquet`, then merges daily census data. The raw task table contains about 397,000 rows and the PID table about 427,000 rows.

**Cleaning and filtering** - The project filters to active facilities, removes test records, keeps valid pickup times, drops extreme target outliers, and engineers time features such as month, year, day of week, week of year, day of month, and day of year.

**Lag features** - Work-order count lags are created across 14 previous steps, allowing the models to learn short-term operational patterns from recent demand.

**Forecasting models** - The project compares XGBoost, linear regression, and hybrid XGBoost + linear regression models for work-order volume. Separate workflows forecast total demand, per-facility demand, and hourly/facility-specific patterns.

**Classification models** - Additional XGBoost models predict work-order volume buckets, location risk, escort risk, and duration bins. Encoders and feature-column files are saved so the workflow can be reused during deployment.

**Deployment output** - A deployment notebook loads saved models, predicts 14 future days, expands predicted facility demand into work-order rows, predicts risk and duration fields, and exports `Deployment_data.csv` / `Deployment_data.xlsx` for dashboard use.

## Result

The project produced a full set of model artifacts and reporting outputs:

- Saved XGBoost, linear regression, hybrid, and classification models.
- Per-facility forecast outputs.
- Future deployment prediction data.
- Risk and duration prediction tables.
- Power BI dashboard files.
- Feature-importance CSVs and score reports.

For the overall work-order forecast, the saved score report showed:

| Model | MAE | RMSE | R2 |
|---|---:|---:|---:|
| XGBoost | 3.22 | 4.50 | 0.23 |
| Hybrid | 3.29 | 4.58 | 0.20 |
| Linear Regression | 3.44 | 4.72 | 0.15 |

For the split facility forecast, average MAE stayed close to 1.2 work orders across the six core facilities. The classification models were strongest on risk:

| Target | Result |
|---|---:|
| Location risk accuracy | 82.0% |
| Escort risk accuracy | 79.1% |
| Duration bin accuracy | 42.4% |
| Work-order volume MAE | 2.86 |

The strongest pieces were facility demand and risk prediction. Duration bins were harder, which makes sense: work duration depends on operational friction that is not always visible in historical structured data.

## What I Learned

This project taught me how different production-style forecasting feels from a clean Kaggle notebook. The hard part was not only training a model; it was keeping columns, encoders, saved models, deployment inputs, future dates, and dashboard outputs aligned.

It also showed that a practical system can combine multiple imperfect models. One model predicts volume, another expands work orders by facility, others classify risk and duration. Together, they become an operational planning tool.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
