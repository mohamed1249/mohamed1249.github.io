---
title: "EDA: Google Play Store — Data Wrangling"
date: 2022-12-02
status: "Completed"
tools:
  - Python
  - Pandas
  - MAna (custom package)
  - Matplotlib
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/eda-google-play-store-data-wrangling"
excerpt: "A systematic data wrangling notebook that produces a publication-ready clean version of the Google Play Store dataset — handling six messy columns, a tiered Rating imputation strategy, duplicate resolution, and genre normalization."
---

## The Short Version

A dedicated data wrangling notebook that takes the raw Google Play Store CSV and produces a fully cleaned `Cleaned_App_dataset.csv` ready for downstream modeling. This is the preprocessing counterpart to the [Google Play Store EDA + ML](../eda-ml-google-play-store-apps) project — the cleaning decisions made here directly enabled the analysis and rating prediction work done there. The notebook documents every transformation step-by-step, with distributions checked before and after to verify that cleaning produced expected results.

## Problem

The raw Google Play Store dataset is one of the messiest on Kaggle: numeric columns stored as formatted strings, mixed units in `Size`, `Rating` nulls with different causes, duplicate apps at different scrape times, and an uninformative `Genres` multi-value column. None of it is usable as-is.

## Approach

Each column was cleaned in sequence, with explicit reasoning at each step:

**Rating (tiered imputation):**
- Missing ratings with zero reviews → assigned `0` (unrated, never reviewed).
- Remaining missing ratings (apps with reviews but no rating) → assigned `-1` (unknown, flagged as a distinct state rather than dropped or mean-imputed).
- Rationale: treating both groups as a single missing value would collapse two meaningfully different states — an app no one has reviewed vs. an app with reviews but a missing rating field.

**Reviews:** `"M"` suffix (millions) expanded to `000000`; `.0` floating-point strings stripped; cast to `int`.

**Size:** `"M"` suffix stripped; non-numeric strings (e.g., `"Varies with device"`) converted to `-1` via a `check_numerics()` try/except function rather than dropped — preserving rows at the cost of a sentinel value.

**Installs:** `+` and `,` characters removed; cast to `int`.

**Price:** `$` stripped; cast to `float`. Apps priced above $200 filtered as outliers (checked against the top-50 most expensive apps to verify the threshold is appropriate). Confirmed ~93% of apps are free.

**Last Updated:** Parsed to datetime; `Year`, `Month`, `DayOfMonth`, and `DayOfWeek` extracted. Apps updated before 2013 inspected as potential stale/test entries.

**Duplicates:** Two-stage deduplication — exact row duplicates first (`drop_duplicates()`), then app-name duplicates keeping the most recently updated entry (`drop_duplicates(subset='App', keep='last')`). The two-stage approach matters: keeping `last` on app-name duplicates preserves the most current data snapshot for each app.

**Content Rating:** `"Unrated"` entries removed (too few to model, semantically distinct from all rated categories).

**Genres:** Semicolons and ampersands replaced; split into lists. A `get_first()` function extracted the primary genre into a new `one_genere` column for use in downstream modeling.

**Output:** `df.to_csv('Cleaned_App_dataset.csv', index=False)`.

## Result

The cleaned dataset retains the overwhelming majority of rows with all six messy columns converted to usable types. The tiered Rating imputation preserved the semantic distinction between two types of null values rather than collapsing them. The two-stage deduplication correctly resolves the scraping artifact where the same app appears at multiple update timestamps. The `MAna.dist()` function was used after each numeric cleaning step to verify that the resulting distribution was plausible — a before/after pattern that makes cleaning bugs immediately visible.

## What I Learned

Tiered imputation — assigning different sentinel values to structurally different types of missingness — is a more honest approach than blanket mean or mode imputation. Here, `0` and `-1` each carry semantic meaning: they're not placeholders, they're informative values. The `check_numerics()` try/except pattern for `Size` is also a reusable idiom worth keeping — rather than writing a multi-condition string parser for every edge case, a single try/except that returns a sentinel on failure handles unknown formats gracefully. The two-stage deduplication order matters: dropping exact duplicates first, then app-name duplicates, is safer than reversing the order, because keeping `last` by app name could inadvertently remove rows that should be considered distinct if exact duplicates hadn't been resolved first.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
