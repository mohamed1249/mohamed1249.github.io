---
title: "ML: Mall Customer Segmentation with K-Means Clustering"
date: 2022-02-24
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - Scikit-learn
  - Plotly
  - Seaborn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-clustering-mall-customer-segmentation-data"
---

## The Short Version

An unsupervised machine learning project that segments 200 mall customers into distinct groups using K-Means clustering. Working from age, annual income, and spending score data, the notebook identifies 5 natural customer personas that could inform targeted marketing or retail strategy.

## Problem

How can a mall or retailer make sense of its customer base without any pre-defined labels? The challenge is grouping customers into meaningful segments purely from behavioral and demographic signals — no target variable, no supervision.

## Approach

The dataset (sourced from Kaggle) contains 200 customers with four features: gender, age, annual income (k$), and spending score (1–100). The pipeline was:

- **EDA first:** Distribution plots for all numeric features, a gender breakdown pie chart, and pairwise scatter plots (with gender as hue) to understand relationships between age, income, and spending score before modeling.
- **Elbow method:** KMeans was run for k=1 through k=10, plotting inertia at each step to identify the natural "elbow" — where adding more clusters stops meaningfully reducing within-cluster variance. The elbow fell at **k=5**.
- **K-Means (k=5):** Fitted using `k-means++` initialization (10 random starts, max 300 iterations) on all three numeric features. Each customer was assigned a cluster label.
- **3D scatter plot:** The five clusters were visualized in a Plotly 3D chart across all three axes simultaneously, making the separation between groups immediately legible.

## Result

Five distinct customer segments emerged, broadly interpretable as personas along the income/spending axes — for example, high-income high-spenders (prime targets), low-income high-spenders (potentially over-extended), high-income low-spenders (disengaged affluent), and so on. The 3D visualization confirmed clean cluster separation, particularly between the extreme corners of the income–spending space.

## What I Learned

The elbow method is a simple but effective way to avoid the trap of over-clustering — it provides a principled stopping point rather than an arbitrary choice. Visualizing clusters in 3D rather than multiple 2D projections made the segmentation far more intuitive; the spatial groupings that looked ambiguous in 2D scatter plots were clearly separated when all three dimensions were shown at once. Unsupervised results are only as useful as their interpretability, and investing in the right visualization is as important as the model itself.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
