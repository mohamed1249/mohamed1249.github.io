---
title: "EDA: YouTube Trending Videos Tableau Dashboard"
date: 2022-09-22
status: "Completed"
field: "EDA"
tools:
  - Tableau
  - Python
  - Pandas
  - CSV
  - Data Visualization
  - Dashboard Design
github: "https://github.com/mohamed1249/Tableau-Dashboards"
demo: "https://public.tableau.com/views/udacity_16636782555880/Dashboard1?:language=en-GB&:display_count=n&:origin=viz_share_link"
kaggle:
excerpt: "An early Tableau dashboard analyzing YouTube trending videos by views, tags, categories, channels, states, likes, dislikes, and comments."
---

## The Short Version

This is an early Tableau dashboard built from cleaned YouTube trending-video data. It explores what kinds of videos attract the most attention, how engagement metrics move together, and how viewing patterns vary across categories, tags, channels, and US states.

It feels like an early version of a question I still like asking: what does attention look like when it becomes data?

## Problem

YouTube data is messy because popularity is not one number. A video can have views, likes, dislikes, comments, tags, category labels, channel names, locations, and dates.

The goal was to build a small dashboard that could make those signals easier to read:

- Which tags and categories collect the most views?
- Which channels dominate the dataset?
- How do likes, dislikes, comments, and views relate?
- Do states show different top categories or channels?
- How do top categories change over time?

## Approach

**Data preparation** - The project uses a cleaned YouTube dataset with 22,524 rows, plus a transposed tags table with 310,728 tag rows and a category-name lookup file.

**Tag analysis** - Tags are reshaped so Tableau can rank them by views and connect them to categories and engagement metrics.

**Dashboard views** - The workbook includes views for top tags, top channels, a US state map, engagement scatter plots, and time-based category trends.

**Design choice** - The dashboard uses a red and light-gray visual direction to echo YouTube's interface while keeping the charts simple.

## Result

The analysis found that Music and Entertainment were among the strongest recurring categories across views, tags, channels, and state-level patterns.

The dashboard also shows a positive relationship between views and engagement: highly viewed videos often also have more likes, dislikes, and comments. Music videos tended to show strong likes and comments, while Entertainment also drew heavy attention with a different engagement balance.

## What I Learned

This project helped me practice turning a broad dataset into a dashboard narrative. The useful part was not only making charts, but deciding which relationships belonged together: tags with categories, states with channels, and engagement metrics with views.

It also taught me the value of visual restraint. A dashboard about a noisy platform like YouTube becomes easier to read when the design keeps the focus on a few strong comparisons.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
{% if page.demo %}- [Tableau Public]({{ page.demo }}){% endif %}
