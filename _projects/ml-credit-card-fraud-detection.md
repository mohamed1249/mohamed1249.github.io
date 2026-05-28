---
title: "ML: Credit Card Fraud Detection"
date: 2022-08-15
status: "Completed"
tools:
  - Python
  - Pandas
  - Scikit-learn
  - eli5
  - Plotly
  - Seaborn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-credit-card-fraud-detection"
excerpt: "A fraud detection classifier on 284,000+ credit card transactions — tackling extreme class imbalance (0.17% fraud) through three progressively stable under-sampling strategies before landing on a feature-selected SVC with zero false negatives."
---

## The Short Version

A four-model fraud detection benchmark on the canonical Credit Card Fraud Detection dataset — 284,807 transactions, only 492 frauds (0.17%). The core challenge isn't the model; it's the class imbalance. The notebook develops three progressively more robust under-sampling strategies to produce a stable, representative balanced dataset, then selects SVC as the final model based on a clinically motivated false negative priority. Permutation importance-based feature selection reduces the feature set further, and the final model achieves zero false negatives on the held-out test set.

## Problem

Credit card fraud detection has a direct financial and human cost asymmetry: letting a fraud through (false negative) is far worse than blocking a legitimate transaction (false positive). With 0.17% positive rate, a model that predicts "not fraud" for everything achieves 99.83% accuracy — meaningless. The real challenge is building a stable, low-variance classifier that minimizes false negatives specifically.

## Approach

**Preprocessing** — `Time` and `Amount` were scaled with `RobustScaler` (median-centered, IQR-scaled — appropriate for the heavy-tailed distributions in transaction data) and replaced in-place. The V1–V28 features are pre-anonymized PCA components and required no additional transformation.

**Two correlation heatmaps** — one on the full imbalanced dataset and one on a balanced subsample — were plotted side-by-side to show how imbalance distorts apparent correlations. The balanced heatmap is labeled explicitly as the reliable reference.

**Three under-sampling strategies, iterated toward stability:**

*Strategy 1 — Random under-sampling (`balance()`)* — Randomly samples `len(frauds)` non-fraud rows. Simple but non-deterministic: running `cross_vs_confusion()` twice produced different model rankings, making comparison unreliable.

*Strategy 2 — Stratified segment sampling (`take_fair_samples()`)* — Divides the non-fraud population into `n_splits` equal segments and takes a fixed number of rows from each segment, ensuring coverage across the full temporal range of the dataset rather than random concentration. More stable than random sampling, but the number of splits still affected results.

*Strategy 3 — Averaged stratified sampling (`super_fair_cross_vs_confusion()`)* — Loops over a range of `n_splits` values (e.g., 100 to 500 in steps of 50), trains all four models at each split count, and accumulates a running average of CV scores and confusion matrices across all iterations. This produces stable, variance-reduced estimates of true model performance regardless of which specific non-fraud rows were selected. The optimal split count (492) was identified and used for final evaluation.

**Four classifiers, all tuned with `GridSearchCV`:**
- **Logistic Regression** — penalty (L1/L2) × C (7 values).
- **KNN** — k (2–4) × algorithm (4 options).
- **SVC** — C (4 values) × kernel (rbf, poly, sigmoid, linear).
- **Decision Tree** — criterion (gini/entropy) × max_depth × min_samples_leaf.

**Model selection on false negative priority** — SVC was selected not for highest CV accuracy but for lowest false negative rate — the priority in fraud detection. A fraud flagged as legitimate is the worst outcome.

**Feature selection on SVC** — Permutation importance via `eli5` run on the first 1,000 test rows. `SelectFromModel` with `threshold=0.01` reduced the feature set to only the most predictive PCA components. SVC was retrained on the reduced feature set.

## Result

The full-feature SVC produced strong confusion matrix results. After permutation importance-based feature selection:

- **CV Score: 92.75%** on the balanced training set.
- **False negatives: 0** on the held-out test set — no fraud transaction was classified as legitimate.

The three-strategy progression toward stable evaluation is the methodological centrepiece: random under-sampling produced inconsistent model rankings across runs; stratified segment sampling improved consistency but remained sensitive to split count; the averaged stratified approach produced stable, trustworthy results across all four models simultaneously.

## What I Learned

The core insight here isn't about models — it's about evaluation stability under under-sampling. Random under-sampling introduces variance not just in the data but in which model "wins," which makes it unreliable for model selection. The `super_fair_cross_vs_confusion()` approach — averaging scores and confusion matrices over many different split configurations — is an original solution to that instability problem: rather than picking one balanced dataset and hoping it's representative, it averages over many. The `RobustScaler` choice is also deliberate and worth naming: `StandardScaler` is distorted by outliers in `Amount`, which has a heavy right tail; `RobustScaler`'s IQR centering is more stable. And the false-negative-priority model selection is the most practically important decision — in fraud detection, a model with slightly lower overall accuracy but zero missed frauds is always preferred over one that optimizes aggregate metrics.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
