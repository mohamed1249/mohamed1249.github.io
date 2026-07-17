---
title: "EDA: Video Game Sales — High-Level Visualizations"
excerpt: "A high-level exploratory analysis of video game releases and sales across platforms, genres, publishers, years, and regions, with reusable visualization functions."
date: 2022-03-20
status: "Completed"
field: "EDA"
tools:
  - Python
  - Pandas
  - Plotly
  - Seaborn
  - Matplotlib
  - pandas-profiling
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/eda-video-game-sales-high-level-visualizations"
---

## The Short Version

A full exploratory analysis of a video game sales dataset covering thousands of titles across platforms, genres, publishers, and global regions. The notebook is split into two halves — release counts and sales figures — and uses an extensive set of reusable visualization functions to answer over 20 specific questions about the industry's history.

## Problem

How did the video game industry evolve over time? Which platforms, genres, and publishers dominated both in number of releases and in actual sales? And do those two rankings tell the same story — or do the best-selling games come from a different slice of the market than the most prolific publishers?

## Approach

The dataset (sourced from Kaggle) is ranked by global sales and includes regional breakdowns for North America, Europe, Japan, and other markets. A `pandas-profiling` report was generated first for a broad overview, then two parallel EDA tracks were built:

**Releases analysis** — counting game releases by year, platform, genre, and publisher, using bar charts, grouped histograms, line charts, and animated bar charts to track trends over time.

**Sales analysis** — aggregating global and regional sales by the same dimensions, identifying the top-selling game per region, and tracing how sales shifted across publishers, platforms, and genres year by year.

A library of reusable helper functions (`most_frequent`, `yearly_most`, `sales_area`, `how_sales_go_animation`, etc.) kept the code DRY and made it easy to ask the same structural question across different dimensions. Both analysis sections close with a sunburst chart summarizing the top 5 publishers × top 4 platforms × top 3 genres — one by release count, one by global sales.

## Result

Key findings from the analysis:

- **Peak release years** were 2006–2011, after which the number of new titles declined sharply.
- **Nintendo** was the standout publisher both in release volume and global sales; the PS2 and DS were the most prolific platforms by release count.
- **Action** was the most released genre; **Sports** and **Shooter** titles drove disproportionately high sales relative to their release counts.
- **North America** consistently outsold all other regions combined, though the gap narrowed over time.
- **Wii Sports** was the top-selling game in NA, EU, and globally; Japan showed a distinctly different top-seller, highlighting regional taste differences.
- The sunburst charts revealed that the publishers dominating release counts aren't always the same ones dominating sales — Electronic Arts, for instance, ranks higher by sales than by sheer volume.

## What I Learned

Building a reusable function library upfront — rather than writing one-off chart calls — made the analysis dramatically faster and more consistent. It also made it easier to notice when the same question (e.g., "how does this break down by year?") gave meaningfully different answers depending on whether the metric was releases or revenue. The two sunburst summaries at the end were a useful forcing function: they required committing to a single view of "who matters most" and exposed how differently the industry looks depending on what you're measuring.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
