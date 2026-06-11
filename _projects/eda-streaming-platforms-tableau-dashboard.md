---
title: "EDA: Streaming Platforms Tableau Dashboard"
date: 2022-09-25
status: "Completed"
field: "EDA"
tools:
  - Tableau
  - Python
  - Pandas
  - CSV
  - Data Modeling
  - Data Visualization
github: "https://github.com/mohamed1249/Tableau-Dashboards"
demo:
kaggle:
excerpt: "An early Tableau dashboard comparing streaming-platform catalogs after reshaping Netflix, Disney+, and Amazon title data into analysis-ready tables."
---

## The Short Version

This project is an early Tableau dashboard about streaming-platform catalogs. It combines title datasets from Amazon Prime, Disney+, and Netflix, then reshapes them into smaller analysis tables for shows, countries, genres, cast, directors, and platforms.

It is a beginner dashboard, but I like it because it already shows a good instinct: before visualization, the data needed structure.

## Problem

Streaming datasets often store rich text fields like cast, countries, directors, and genres as comma-separated strings. That format is fine for reading one row, but it is weak for dashboard analysis.

The project focused on preparing the data so Tableau could answer questions like:

- Which platform has the largest catalog?
- How do movies and TV shows compare across platforms?
- Which countries and genres appear most often?
- Which actors and directors are repeated across titles?
- How do release years and ratings vary by platform?

## Approach

**Data preparation** - A Pandas notebook loads separate streaming datasets, assigns platform IDs, and creates a platform lookup table.

**Relational reshaping** - Multi-value fields are split into separate tables: `casts.csv`, `directors.csv`, `genres.csv`, and `countries.csv`. The cleaned `shows.csv` table keeps the title-level fields, while the supporting tables make cast, genre, director, and country analysis easier.

**Dashboard build** - The Tableau workbook connects to the prepared CSVs and builds a dashboard with eight worksheets around catalog composition, title type, release year, rating, country, genre, and people-related patterns.

## Result

The prepared dataset includes:

- 19,946 title records in `shows.csv`,
- 114,392 cast-title rows,
- 41,542 genre-title rows,
- 16,334 director-title rows,
- 12,371 country-title rows.

The result is a comparative dashboard for understanding streaming catalogs as structured data rather than isolated CSV files.

## What I Learned

This project taught me that beginner dashboard work can still include serious data preparation. Splitting list-like fields into separate tables made the Tableau side much more flexible.

It also made me more aware of a common analytics pattern: good visualization often starts with small data modeling decisions.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
{% if page.demo %}- [Tableau Public]({{ page.demo }}){% endif %}
