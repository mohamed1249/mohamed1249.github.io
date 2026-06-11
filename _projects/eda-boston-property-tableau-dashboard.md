---
title: "EDA: Boston Property Tableau Dashboard"
date: 2023-12-10
status: "Completed"
field: "EDA"
tools:
  - Tableau
  - Excel
  - CSV
  - Data Cleaning
  - Data Visualization
  - Dashboard Design
github: "https://github.com/mohamed1249/Boston-Property-Tableau-Dashboard"
demo:
kaggle:
excerpt: "A Tableau dashboard analyzing Boston condominium assessment data, using cleaned property records to compare city value patterns, construction periods, and features that influence condo prices."
---

## The Short Version

This project is a Tableau dashboard built around Boston property assessment data. The analysis focuses on condominium records so the comparisons stay fair: condos are compared with condos, rather than mixing residential condos with unrelated land-use types.

The final dashboard turns a cleaned property dataset into a set of visual questions about location, value, age, and property features.

## Problem

The original property dataset was large and broad, with many land-use types and many variables. For a dashboard to be useful, the scope needed to become tighter and more explainable.

The project narrowed the analysis to condominium properties and focused on questions such as:

- which Boston-area cities or neighborhoods have the highest average property values,
- which locations look strongest as investment candidates,
- which decades had the most condo construction,
- which property features appear most connected to total assessed value.

## Approach

**Data preparation** - The dataset was cleaned down from a much larger property assessment file into a focused condo dataset with about 63k records. The final fields include location, living area, total value, year built, condition, bedroom and bathroom counts, total rooms, AC type, parking, view, and style fields.

**Dashboard design** - The Tableau workbook uses multiple views inside a single dashboard, including top-N bar charts, time-oriented value views, construction-period analysis, and a parameterized feature comparison.

**Value comparison** - Average property value is compared by city, with a top-10 filter to keep the view readable and focused on the strongest areas.

**Time and construction analysis** - Build years are grouped into broader periods so the dashboard can show when condo construction was most concentrated.

**Feature exploration** - A dynamic scatter-style view lets the user switch the comparison feature, such as total rooms, bedrooms, full baths, living area, and parking, while keeping total value as the target.

## Result

The dashboard highlights several clear patterns:

- Boston has the highest average condo assessment value in the cleaned dataset, around 1.16M dollars.
- Charlestown, South Boston, Jamaica Plain, and Chestnut Hill also appear among the highest-value areas.
- The largest construction waves in the cleaned condo data appear around the 1890s, 1900s, 2010s, 2000s, and 1920s.
- The feature-comparison view makes the dashboard more exploratory, letting a user inspect how property characteristics relate to assessed value.

As a portfolio project, this shows the communication side of data work: cleaning the scope, choosing visuals that match the questions, and turning a table into a dashboard a stakeholder can actually read.

## What I Learned

This project reinforced that dashboard quality starts before Tableau. The analysis only works because the dataset was narrowed to a coherent population and the questions were made specific enough to guide the visual choices.

It also shows why dashboards need restraint. A useful dashboard does not show every field; it guides attention toward the comparisons that matter.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
