---
title: "ML: Email Spam Classification & Anomaly Detection"
date: 2022-09-27
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - Scikit-learn
  - XGBoost
  - NLTK / TF-IDF
  - Plotly
  - Seaborn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-email-spam-classification-anomaly-detection"
excerpt: "Four supervised classifiers benchmarked on 5,500+ emails with TF-IDF features, then a Local Outlier Factor anomaly detector applied to the same problem — correctly labeling 81% of emails with zero label supervision."
---

## The Short Version

A two-part spam detection project on a 5,500+ email dataset. The first half benchmarks four supervised classifiers — Logistic Regression, KNN, Random Forest, and XGBoost — all tuned with GridSearchCV and evaluated by confusion matrix. The second half applies Local Outlier Factor, an unsupervised anomaly detection algorithm, to the same task — not because it's the right tool, but as a deliberate experiment: how far can a zero-label method get on a binary classification problem?

## Problem

Can email spam be reliably detected from raw text alone? And as a stretch question: if you had no labels at all and treated spam as an anomaly in a sea of normal email, how much of the signal would an unsupervised outlier detector still capture?

## Approach

**Preprocessing** — The raw CSV had extra columns that were dropped immediately, leaving only the label (`ham`/`spam`) and the email text. Labels were integer-encoded with `LabelEncoder`. A `length` feature was engineered to visualize the distribution of email lengths by class (spam emails are measurably longer on average).

**TF-IDF Vectorization** — The text column was vectorized using `TfidfVectorizer` with English stop words removed. The vectorizer was fit on the training split only and transformed separately for train and validation — no leakage.

**Helper functions** — Two reusable functions kept the modeling section clean:
- `best_estimator()` — wraps `GridSearchCV` and `cross_val_score`, prints the best estimator and its 5-fold CV score, and returns the fitted best model.
- `cf()` — plots a styled Seaborn confusion matrix heatmap for any fitted model against the validation set.

**Four supervised classifiers**, all tuned via `GridSearchCV`:**

- **Logistic Regression** — penalty (L1/L2) and C swept across 7 values.
- **KNN** — k swept from 1 to 99 (odd values only).
- **Random Forest** — 500 estimators, `min_samples_split=5`, `bootstrap=False` (best params from a prior commented-out grid search).
- **XGBoost** — 1,000 estimators, lr=0.1, `gbtree` booster, L1 regularization α=0.5.

**Model selection with a deliberate priority** — rather than selecting purely on cross-validation accuracy, KNN was preferred over Logistic Regression on the grounds that false negatives (spam reaching the inbox) are more acceptable than false positives (ham incorrectly blocked). This is an explicit business-logic decision embedded in the write-up.

**Local Outlier Factor (anomaly detection)** — LOF treats spam as the "outlier" class (label = -1) and ham as inliers (label = 1). The TF-IDF matrix was re-fit on the full dataset, and LOF was run with `n_neighbors=23` and `contamination=0.1`. Predictions were remapped to 0/1 and compared against ground truth labels.

## Result

All four supervised classifiers achieved strong performance on the validation set — Random Forest and XGBoost had the lowest false positive rates, making them the safest choices for production use. Logistic Regression's CV score was higher than KNN's, but KNN was preferred for its more conservative false positive profile.

**LOF: 81% accuracy with zero label supervision.** Out of 5,572 emails, 4,521 were correctly classified using only the TF-IDF structure of the text — no target variable, no training signal, just the geometric assumption that spam looks like an outlier in the feature space. The confusion matrix shows it over-predicted outliers (expected given the broad contamination estimate), but the 81% baseline is a meaningful result given the complete absence of supervision.

## What I Learned

The false positive vs. false negative priority decision is the most practically important insight in this notebook — overall accuracy is a misleading metric when the costs of the two error types are asymmetric. Treating KNN as the better model despite lower CV accuracy, and justifying it explicitly, reflects the kind of business-context reasoning that pure benchmark comparisons miss. The LOF experiment is self-aware: the notebook intro explicitly acknowledges that anomaly detection isn't the right algorithm for a labeled classification problem — and runs it anyway as a curiosity. That framing matters: it shows the experiment was a deliberate probe of the method's limits, not a confusion about when to apply it. Getting 81% with no labels on a binary text problem is a result worth understanding, not dismissing.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
