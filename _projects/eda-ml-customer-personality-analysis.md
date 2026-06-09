---
title: "EDA + ML: Customer Personality Analysis & Segmentation"
date: 2022-06-19
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
kaggle: "https://www.kaggle.com/code/ma12492002/eda-ml-customer-personality-analysis"
excerpt: "A full-pipeline analysis of a marketing dataset — EDA surfacing income, spending, and complaint patterns across customer demographics, followed by K-Means clustering validated with both the elbow method and silhouette scores."
---

## The Short Version

A combined EDA and ML project on a marketing campaign dataset containing ~2,200 customers with demographic, behavioral, and purchasing attributes. The EDA half systematically explores how education, marital status, age, income, and family structure interact with spending patterns and complaint behavior. The ML half clusters customers into four distinct personality types using K-Means on scaled, encoded features — validated with both the elbow method and silhouette scoring.

## Problem

Who are this company's customers, really? And beneath the demographic surface, are there meaningful behavioral segments — groups that differ not just in who they are, but in how much they spend, what they buy, and how often they complain?

## Approach

**Feature engineering and cleaning** — Several derived features were created before analysis: `Age` (calculated from birth year relative to 2015), `Kids` (sum of `Kidhome` and `Teenhome`), `day_engaged` (days since first purchase), and `sum_mnt` / `no_purchases` (total spend and purchase count aggregates). Education was collapsed to two tiers (UG / PG) and marital status to two states (Single / Relationship). Income nulls (~1%) were filled with the column mean.

**EDA** — A reusable function library (`most_frequent`, `dist`, `relations`, `percentages`) powered the exploration. Key analytical threads included:
- Distribution of age, income, and kids by education and marital status.
- Pairwise regression plots (`lmplot`) between income, age, kids, and each product category spend — broken out by education tier to catch interaction effects.
- Complaint profiling: whether customers who complained in the last 2 years differed systematically across income, age, recency, and kids.
- Channel behavior: how web, catalog, store, and deal purchases varied across demographic groups.

**Clustering** — Spending-related columns were dropped before clustering to focus on behavioral/demographic features. After label-encoding categorical variables and applying `StandardScaler`, the elbow method and silhouette scores were computed for k=2 through k=10. Both methods converged on **k=4**. Each cluster was then profiled using `describe()` and interpreted manually.

## Result

Key EDA findings:

- PG-educated customers earn significantly more and spend more across every product category; the income–spend relationship is strongly positive for this group and weakly negative for UG customers.
- Income and number of kids have a strong negative relationship — higher earners tend to have fewer children.
- Complainers have lower income, more kids, older age, and longer recency — a coherent profile of a disengaged, lower-satisfaction segment.
- UG customers visit the website more frequently despite purchasing less — suggesting higher browsing-to-conversion friction for that group.

The four customer clusters:

| Cluster | Education | Avg. Income | Avg. Items Bought | Avg. Purchases | Notable |
|---|---|---|---|---|---|
| 1 | PG | ~$70K | ~1,000 | ~20 | High-value, low kids, ~40 yrs old |
| 2 | UG | ~$20K | ~80 | ~5 | Low-engagement, 1 kid, ~35 yrs old |
| 3 | PG | ~$40K | ~115 | ~7 | Mid-tier, 1 kid, ~45 yrs old |
| 4 | PG | ~$40K | ~350 | ~10 | Mid-tier, **all complained**, 1 kid |

Cluster 4 is the most analytically interesting: similar income and demographics to Cluster 3, but 100% complaint rate — suggesting a quality or service issue rather than a demographic one.

## What I Learned

Using both the elbow method and silhouette scores to validate k is a stronger practice than either alone — they can disagree, and when they agree on the same k, confidence in the choice is meaningfully higher. The most valuable EDA insight wasn't the obvious income–spend correlation but the interaction effects: the income–spend relationship inverts sign between PG and UG customers, which wouldn't appear in a flat correlation heatmap. Building `sum_mnt` and `no_purchases` as aggregate features first, then correlating them against demographics, gave a cleaner signal than analyzing each product category in isolation.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
