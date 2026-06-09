---
title: "ML: Evaluating PCA Impact on Churn Prediction Models"
date: 2025-05-19
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - Scikit-learn
  - XGBoost
  - MAna (custom package)
  - Seaborn
  - Matplotlib
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-evaluating-pca-impact-on-churn-models"
excerpt: "A controlled experiment on 790,624 real telecom records — measuring the impact of PCA dimensionality reduction on four classifiers across accuracy, F1, ROC AUC, and training time, with findings that confirm PCA is model-type-dependent."
---

## The Short Version

A principled controlled experiment on a real-world telecom churn dataset — not a tutorial, not a benchmark, but a deliberate investigation into a question that appears constantly in ML pipelines: does PCA help? Four classifiers (Logistic Regression, SVC, Random Forest, XGBoost) are each trained twice — once on the full 67-feature space and once on the 45-component PCA-reduced space — and compared across five metrics plus training time. The findings confirm the theoretical expectation: PCA benefits linear models, harms tree-based ones, and the gap is large enough to matter in production.

## Problem

PCA is commonly applied as a pre-processing step without asking whether the downstream model actually benefits from it. Does dimensionality reduction help all model types equally? And what does "help" mean — accuracy, training speed, or both? This experiment was designed to answer that question empirically on a large, real dataset rather than a toy example.

## Approach

**Dataset** — 790,624 telecom customer records from a private client, preprocessed in a separate project (cleaning, encoding, scaling). 71 numeric features, binary `churned` target. Feature data arrives pre-scaled — a prerequisite for meaningful PCA.

**Class balancing** — `balance_classes()` function: downsamples the majority class to 1.2× the minority class size using `resample`, concatenates, and shuffles. The 1.2× factor (rather than strict 1:1) was a deliberate choice to retain slightly more majority class signal.

**PCA** — `PCA(n_components=0.95, svd_solver='full')`: automatically selects the number of components needed to retain 95% of variance. Result: 67 → 45 components, a 33% dimensionality reduction.

**Experiment design** — Each of the four classifiers was trained and evaluated in two identical runs: once on the original feature matrix, once on the PCA-reduced matrix. Timing was measured with `time.time()` brackets around each `fit()` call. Five metrics reported per run: accuracy, precision, recall, F1, ROC AUC. `MAna.confusion_matrix()` provided visual per-run validation.

Each model was theoretically characterized before running — the notebook documents *expected* PCA impact for linear vs. tree models before revealing the results, making the experiment a proper hypothesis test rather than post-hoc rationalization.

## Result

| Model | Metric | Without PCA | With PCA | Δ |
|---|---|---|---|---|
| Logistic Regression | F1 | ~0.65 | ~0.64 | −0.01 |
| Logistic Regression | Train time | 0.33s | **0.13s** | −60% |
| SVC | F1 | 0.65 | 0.65 | 0.00 |
| SVC | Train time | 46.69s | **36.43s** | −22% |
| Random Forest | F1 | **0.66** | 0.63 | −0.03 |
| Random Forest | Train time | 6.16s | 20.20s | **+228%** |
| XGBoost | F1 | **0.67** | 0.61 | −0.06 |
| XGBoost | ROC AUC | **0.69** | 0.65 | −0.04 |

**Linear models:** PCA delivered a 60% training speedup for Logistic Regression and a 22% speedup for SVC at a cost of ≤0.01 in F1 — a clear win when speed matters.

**Tree-based models:** PCA degraded both RF (−0.03 F1) and XGBoost (−0.06 F1) while actually *increasing* Random Forest training time by 228%. No benefit on any axis.

**Most unexpected finding:** Random Forest was slower *with* PCA than without. This is attributable to PCA components lacking the intuitive feature structure that trees use to find efficient splits — abstract components require more tree traversal to achieve comparable separation.

## What I Learned

The most valuable aspect of this experiment is its confirmation of a theoretical prediction with real data and real timing numbers. Knowing that PCA "tends to help linear models" is textbook; watching it deliver a 60% speedup on LR and simultaneously slow down Random Forest by 3× makes it concrete and memorable. The pre-experiment hypothesis sections in the notebook are worth preserving in the write-up because they distinguish between post-hoc rationalization and actual empirical validation — the difference between "we ran PCA and here's what happened" and "we predicted what would happen and here's whether we were right." The 1.2× majority-class factor in `balance_classes()` is also a more honest choice than strict 1:1 balancing — retaining slightly more majority signal often produces better generalization on the imbalanced test set, which reflects real-world class distribution.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
