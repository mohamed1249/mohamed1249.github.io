---
title: "ML: Australian Horse Racing Rating System"
date: 2024-07-13
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - NumPy
  - XGBoost
  - TensorFlow
  - Keras
  - Pickle
  - CSV
  - Excel
github: "https://github.com/mohamed1249/Australian-Horse-Racing-Rating-System"
demo:
kaggle:
excerpt: "A horse-racing analytics pipeline that collects Australian race data, engineers form and rating features, combines XGBoost and neural-network predictions, and exports daily rating sheets."
---

## The Short Version

This project is a racing analytics and rating system for Australian horse-racing data. It collects daily meeting, field, form, jockey, trainer, and ratings data, transforms the raw inputs into model-ready features, then generates prediction and rating sheets for upcoming races.

I treated it as a structured machine-learning problem: collect the right data, normalize messy racing fields, engineer meaningful form features, load trained models, and produce repeatable daily outputs.

## Problem

Horse racing data is dense and fragmented. Useful signals may be spread across many small pieces of information:

- race meeting and track details,
- horse, jockey, and trainer fields,
- past race form,
- previous margins and finishing positions,
- barrier changes,
- distance changes,
- class changes,
- track-condition history,
- external market and rating feeds.

The goal was to turn those scattered sources into a consistent rating workflow that could be run for a race date and exported into practical CSV/Excel files.

## Approach

**Data collection** - The pipeline pulls daily racing data from multiple racing APIs, including meetings, fields, form text, trainer strike rates, jockey strike rates, ratings sheets, and market-style price data.

**Feature engineering** - The code builds features from recent form history, including previous margins, previous prices, prior barriers, days between runs, distance changes, weight changes, track/distance strike rates, class movement, and number of starters.

**Data preparation** - Raw race and form files are merged into cleaned meeting-level datasets. The workflow also exports intermediate files for ratings, fields, cleaned form, previous runs, and final test inputs.

**Modeling** - The prediction stage loads a trained XGBoost model and a Keras neural-network model, applies saved encoders and feature-column lists, and combines their outputs into a final score.

**Daily exports** - The final predictions are written to dated CSV files with each runner's race context and model score, making the system usable as a repeatable daily ratings process.

## Result

The project produced a working end-to-end pipeline:

- download race-day inputs,
- build cleaned form datasets,
- merge race, horse, trainer, jockey, market, and historical-form features,
- generate test files for the selected date,
- score runners with an XGBoost plus neural-network ensemble,
- export dated prediction sheets.

The local project includes prediction files for November 2023 and a larger December 2023 data-refresh workflow with meeting-specific rating sheets.

This should be read as an analytics and modeling project, not betting advice. A production-grade racing model would need deeper backtesting, calibration, leakage checks, market comparison, bankroll simulation, and live monitoring before anyone could judge whether it has real predictive or economic value.

## What I Learned

This project made feature engineering feel like the real center of applied machine learning. The model is only one piece; the harder work is turning messy racing history into features that are consistent enough for the model to use.

It also reinforced the importance of reproducible daily pipelines. A useful rating system has to fetch fresh data, rebuild features, handle missing or malformed files, and export outputs in a way that can be checked by a human.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
