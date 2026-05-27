---
title: "ML: Text Classification for Women's Clothing Reviews"
date: 2023-02-22
status: "Completed"
tools:
  - Python
  - Pandas
  - XGBoost
  - Scikit-learn
  - NLTK
  - Plotly
  - Seaborn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-text-classification-for-clothing-reviews"
excerpt: "A systematic XGBoost classification experiment on 23,000+ clothing reviews — comparing five configurations across feature type (non-textual, textual, combined) and class balancing strategy (none, under-sampling, over-sampling)."
---

## The Short Version

A structured NLP classification project that predicts the sentiment of women's clothing reviews (Negative / Neutral / Positive) using XGBoost. Rather than training a single model and reporting results, the notebook runs five distinct experiments — varying the input features and class balancing strategy — to isolate what actually moves the needle. The dataset contains over 23,000 reviews with both structured metadata and free-text fields.

## Problem

Can a model trained on structured metadata (rating, product category, recommendation flag) classify review sentiment as accurately as one trained on the review text itself? And when the training classes are heavily imbalanced — Positive reviews vastly outnumber Neutral ones — which balancing approach actually helps?

## Approach

**Preprocessing and feature engineering:**
- Ratings were bucketed into three classes: Negative (<3), Neutral (=3), Positive (>3).
- Categorical features were label-encoded; numerical features were standardized with `StandardScaler`.
- Review text was cleaned (non-alphanumeric removal, lowercasing), stop words removed, and words stemmed with Porter Stemmer, then vectorized into 2,000 TF-IDF features.
- A correlation heatmap was produced before modeling, which immediately surfaced a strong correlation between the `Recommended IND` column and the target — a signal that became confirmed by feature importance after training.

**Five experiments — all using XGBoost (1,000 estimators):**

1. **Non-textual features only** — structured metadata (age, category, recommendation flag, etc.) without review text.
2. **Textual features only** — 2,000 TF-IDF features from the preprocessed review text, no metadata.
3. **Combined features** — both structured and TF-IDF features together.
4. **Combined + under-sampling** — the majority classes were downsampled to match the minority (Neutral) class size using a custom `under_balance_classes` function.
5. **Combined + over-sampling** — the minority classes were upsampled to match the majority (Positive) class size using a custom `over_balance_classes` function via `resample`.

Feature importance was visualized after each experiment using XGBoost's built-in `feature_importances_` scores.

## Result

| Configuration | Positive Acc. | Neutral Acc. | Negative Acc. | Notes |
|---|---|---|---|---|
| Non-textual only | High | Low | Moderate | `Recommended IND` dominated importance |
| Textual only | Slightly lower overall | Low | Moderate | Top TF-IDF words were interpretable sentiment markers |
| Combined (best baseline) | ~98.2% | ~29.8% | ~68.2% | Best overall; combined features complementary |
| Under-sampled | Degraded | Degraded | Degraded | Underfitting from data loss |
| Over-sampled | ~97.5% | ~29.8% → ~28.3%* | ~71.2% | Marginal gains on Negative/Neutral, slower training |

*Over-sampling slightly improved Negative and Neutral recall at a small cost to Positive accuracy and significantly longer training time — a genuine tradeoff the notebook discusses explicitly.

The `Recommended IND` feature was the single most predictive non-textual feature by a wide margin, as anticipated from the correlation heatmap. The Neutral class remained the hardest to predict across all configurations — a natural consequence of its boundary ambiguity and class sparsity.

## What I Learned

Running experiments as a controlled comparison — varying one thing at a time — revealed something that a single-model notebook would have missed: the text features and structured features are genuinely complementary, but neither alone matches their combination. Under-sampling was a clear failure here because the dataset's Neutral class is small enough that equalizing all three classes to it throws away the majority of the training data, producing an underfit model. Over-sampling improved minority-class recall but at a training cost — and the notebook frames this as a context-dependent decision rather than a universal recommendation, which is the right way to present it. The `Recommended IND` dominance in feature importance is also a useful reminder to check for proxy targets before celebrating a model's accuracy.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
