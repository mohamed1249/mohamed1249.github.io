---
title: "ML: Customer Churn Prediction for Telecom Dataset"
date: 2023-10-05
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - Scikit-learn
  - XGBoost
  - Random Forest
  - SVC
  - Logistic Regression
  - LSTM
  - ELI5
  - Power BI
github: "https://github.com/mohamed1249/Customer-Churn-Prediction-for-Telecom-Dataset"
demo:
kaggle:
excerpt: "A telecom churn prediction project that labels customer-week records from disconnected-number data, preprocesses usage and revenue features, balances the classes, compares LR/SVC/RF/XGBoost/LSTM models, and exports trained models, feature importances, predictions, and Power BI artifacts."
---

## The Short Version

This project builds a churn prediction pipeline for telecom customer data. It combines weekly subscriber records, revenue and usage behavior, recharge activity, call/SMS/data features, demographic fields, activation and recharge dates, and a disconnected-number file to label customers as churned or not churned.

The final workflow compares multiple models, saves trained model artifacts, exports feature importances and best predictions, and connects to a Power BI dashboard workflow.

## Problem

Telecom churn is difficult because the signal is spread across many small behavioral traces: revenue changes, recharge gaps, call activity, data usage, network status, package type, activation date, and recent engagement.

The goal was to turn raw customer-week records into a model that can identify likely churned customers early enough for retention teams to act.

## Approach

**Data assembly** - The project starts from multiple telecom CSV files and a September 2023 data drop. The raw files include hundreds of thousands of weekly customer records with package, nationality, gender, revenue, recharge, call, SMS, and data usage columns.

**Disconnected-number labeling** - A separate disconnected-number file is used to label customers. The pipeline marks whether a mobile number appears in the disconnected list, then builds a `churned` target from customer history.

**Preprocessing** - The notebook converts date columns, derives month/day/weekday/year features, fills numeric and categorical missing values, encodes categorical columns, removes unused ID-like fields, scales numeric features, and exports modeling datasets.

**Class balancing** - Because churn is a minority class, the training set is balanced by downsampling the majority class. In the September workflow, the modeling table contains about 115,000 rows, with roughly 113,000 non-churned and 2,256 churned examples before balancing.

**Models compared** - The project trains and tunes:

- Logistic Regression.
- SVC.
- Random Forest.
- XGBoost.
- LSTM neural network.

Feature selection uses permutation importance, and the final XGBoost workflow exports a confusion matrix, feature-importance file, trained model, and best prediction table.

## Result

The final XGBoost predictions file contains 452 validation predictions. On that saved output, the model reached:

| Metric | Score |
|---|---:|
| Accuracy | 79.6% |
| Precision | 73.3% |
| Recall | 89.2% |
| F1 score | 80.4% |
| ROC AUC | 80.2% |

The confusion matrix shows the model catching most churned customers:

|  | Predicted not churned | Predicted churned |
|---|---:|---:|
| Actual not churned | 171 | 69 |
| Actual churned | 23 | 189 |

The strongest features in the saved XGBoost importance file included activation weekday, last recharge weekday, activation month-day, last recharge month-day, birth year, local outgoing-call active days, birth month, total outgoing call count, incoming SMS count, and last revenue weekday.

Saved artifacts include trained Logistic Regression, SVC, Random Forest, XGBoost, and LSTM models, plus prediction CSVs, feature importances, correlations, and a Power BI dashboard file.

## What I Learned

This project showed that churn labeling is often more important than model choice. The model could only become useful after the disconnected-number file was turned into a clean target and the customer-week history was shaped into stable features.

It also became the foundation for a later PCA experiment, where the same telecom churn problem was used to test how dimensionality reduction affects linear models versus tree-based models.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
