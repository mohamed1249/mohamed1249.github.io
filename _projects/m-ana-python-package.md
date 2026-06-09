---
title: "Library: M-Ana Data Science Package"
date: 2023-03-18
status: "Alpha / v0.1.0"
field: "Tool"
tools:
  - Python
  - Pandas
  - NumPy
  - Scikit-learn
  - Plotly
  - Matplotlib
  - Statsmodels
  - TensorFlow
  - PyTorch
  - SQL
github: "https://github.com/mohamed1249/M-Ana"
demo:
kaggle:
excerpt: "A personal Python data-science toolkit, now documented as v0.1.0, that collects reusable tools for data loading, cleaning, visualization, modeling, time-series analysis, statistics, A/B testing, and database workflows."
---

## The Short Version

M-Ana is a personal Python package for data manipulation, analysis, modeling, and experimentation. It started as a way to collect the repeated helper functions I needed while working on data projects, then grew into a broader toolkit for cleaning data, visualizing patterns, evaluating models, handling time series, running statistical tests, and working with databases.

It is an alpha-stage library, now documented locally as `v0.1.0`, but it shows an important shift in my work: moving from one-off notebooks toward reusable tools.

## Problem

Data science projects often repeat the same work:

- load data from different formats,
- clean missing values and outliers,
- make the same diagnostic plots,
- evaluate models,
- compare experiments,
- create time-series features,
- save models,
- connect to databases.

Instead of rewriting these pieces every time, M-Ana packages them into reusable modules.

## Approach

**Data utilities** - The package includes helpers for reading files, reading images, loading tables from HTML, connecting to databases, filling missing values, removing outliers, validating columns, cleaning entry errors, and producing cleaning reports.

**Visualization tools** - The analysis module wraps common visualizations such as line plots, scatter plots, bar charts, distributions, heatmaps, pairplots, treemaps, Sankey diagrams, funnels, waterfall charts, dashboards, and animated plots.

**Modeling utilities** - M-Ana includes model evaluation, feature preprocessing, model registry/versioning, save/load helpers for scikit-learn, Keras, and PyTorch models, auto-classifier and auto-regressor helpers, callbacks, and PyTorch training utilities.

**Time-series toolkit** - The time-series modules cover validation, datetime indexing, resampling, missing timestamp handling, stationarity tests, differencing, feature creation, anomaly detection, decomposition, forecasting, evaluation, and visualization.

**Statistics and experimentation** - The `stata` modules include hypothesis tests, effect sizes, confidence intervals, bootstrap utilities, A/B testing, multivariate testing, lift analysis, correction methods, and statistical visualizations.

**Database layer** - The package includes SQL helpers, query-building utilities, and connectors for relational databases and MongoDB-style workflows.

**Packaging** - The project includes `setup.py`, refreshed README documentation, package metadata, a license, changelog, built wheel/source distributions, and test files. That makes it a real Python package rather than only a folder of helper scripts.

## Result

The current package covers a wide part of the data-science workflow:

- data cleaning and validation,
- exploratory visualization,
- statistical testing,
- A/B testing,
- machine learning evaluation,
- model persistence,
- time-series forecasting,
- anomaly detection,
- database access,
- dashboard-style plotting.

As a portfolio project, M-Ana shows engineering maturity: it is about building reusable infrastructure for future analysis, not only solving a single dataset.

## What I Learned

This project taught me how different a reusable library is from a notebook. In a notebook, a function only has to work once. In a package, naming, structure, documentation, defaults, error handling, dependencies, and consistency matter much more.

It also taught me that scope needs discipline. A personal data-science package can become powerful, but it needs careful organization, tests, documentation, and versioning so it stays useful instead of becoming a drawer full of clever functions.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
