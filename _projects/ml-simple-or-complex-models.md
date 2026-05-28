---
title: "ML: Simple or Complex Models?"
date: 2022-03-21
status: "Completed"
tools:
  - Python
  - Pandas
  - Scikit-learn
  - Plotly
  - Seaborn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-simple-or-complex-models"
excerpt: "A deliberate experiment on the Acoustic Extinguisher Fire dataset — comparing Decision Tree, Random Forest, and SVC to test whether model complexity correlates with accuracy."
---

## The Short Version

A concise, thesis-driven experiment: does a more complex model always perform better? Three classifiers — Decision Tree, Random Forest (5 trees), and SVC — are trained on the Acoustic Extinguisher Fire dataset and compared by both accuracy and training time. The answer is no.

## Problem

The intuitive assumption in ML is that complexity pays off. SVC is mathematically more sophisticated than a Decision Tree. A Random Forest combines multiple trees. So which actually wins? And at what cost in compute time?

## Approach

The dataset is a clean tabular binary classification problem — fire extinguishment status predicted from acoustic and physical features (frequency, decibel level, distance, airflow, fuel type). `FUEL` was ordinal-encoded; all features were `StandardScaler`-scaled before training; an 80/20 train/test split.

Three models trained with default or minimal configuration:
- **Decision Tree** — default hyperparameters.
- **Random Forest** — 5 trees, `bootstrap=False`.
- **SVC** — default RBF kernel, default C.

Accuracy reported via `accuracy_score` on the held-out test set. Training time observed directly (SVC: ~8.4 seconds; Decision Tree: ~0.2 seconds — a 42× difference).

## Result

| Model | Accuracy | Training Time |
|---|---|---|
| Decision Tree | **96%** | ~0.2s |
| Random Forest (5 trees) | **96%** | — |
| SVC | 94% | ~8.4s |

The simplest model tied the ensemble and outperformed the most complex one — at 42× the speed advantage. Conclusion stated directly in the notebook: simple models (and ensembles of simple models) can be more accurate *and* more efficient than complex models with longer processing time.

## What I Learned

Model complexity and model performance are not the same axis. SVC's margin-maximizing geometry is powerful in many contexts, but on a clean, low-dimensional tabular problem with well-separated classes, a single Decision Tree captures the structure just as well — and in a fraction of the time. The Random Forest result (matching the single tree with only 5 estimators, no bootstrapping) reinforces the same point: the problem's inherent structure, not the algorithm's sophistication, determines how much complexity is actually needed. This is the kind of experiment worth running early and often — it's easy to default to the most complex available model without asking whether the problem requires it.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
