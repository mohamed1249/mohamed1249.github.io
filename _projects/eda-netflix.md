---
title: "EDA: Netflix Movies & TV Shows"
date: 2022-05-15
status: "Completed"
field: "EDA"
tools:
  - Python
  - Pandas
  - Plotly
  - Seaborn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/eda-netflix-movies-and-tv-shows"
excerpt: "A fast EDA of Netflix's catalog — seven questions answered across content type, release trends, ratings, countries, cast, directors, and genres."
---

## The Short Version

A focused, fast-paced EDA of the Netflix Movies & TV Shows dataset answering seven clean questions about the catalog's composition. Notable for an early appearance of the `RepetitionDF()` function — the same multi-value list aggregation utility that became a recurring tool in the Google Play Store and IMDB projects — used here to count cast member and director appearances across comma-separated fields.

## Problem

What does Netflix's catalog actually look like? How are movies and TV shows distributed across release years, months, countries, ratings, cast, directors, and genres?

## Approach

Seven questions, one chart each:

1. **Movies vs. TV Shows ratio** — Pie chart; movies dominate the catalog.
2. **Release year trends** — Grouped bar chart (post-1970) by content type; Netflix's original production ramp-up is clearly visible in recent years.
3. **Monthly additions** — Grouped bar chart by month; content additions spike in certain months, with July and December standing out.
4. **Content ratings distribution** — Pie chart; TV-MA and TV-14 account for the large majority of content.
5. **Top production countries** — Country column split on commas (first country retained); countries with fewer than 100 titles filtered out. Grouped bar chart reveals the USA's overwhelming dominance, with India and UK as distant second and third.
6. **Most-appearing cast members** — `cast` column split on commas → `RepetitionDF()` → top 20 bar chart.
7. **Most-appearing directors** — Same pipeline on `director` column.
8. **Most common genres** — Same pipeline on `listed_in` (multi-genre comma-separated) → top 20 bar chart.

## Result

- Movies make up ~70% of the catalog; TV shows ~30%.
- USA produces by far the most Netflix content; India and UK are the only other countries clearing 100+ titles in the filtered set.
- TV-MA is the most common content rating — Netflix's catalog skews toward adult audiences.
- The top cast members by appearances are predominantly actors with long-running TV shows or recurring Netflix originals.
- International Movies and Dramas are the two most common genre tags.

## What I Learned

This is one of the earlier EDA notebooks in the portfolio, and it shows the `RepetitionDF()` function appearing in its initial form — the same utility that was later extended in the IMDB deep analysis and Google Play Store projects to handle weighted aggregation, percentage sunburst charts, and multi-key lookups. Starting simple and reusing the function across progressively more complex projects is a more effective pattern than rebuilding it from scratch each time. The country filtering step (dropping countries with fewer than 100 titles before plotting) is also a small but important data cleaning decision — without it, dozens of single-entry countries crowd the chart and make the dominant patterns unreadable.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
