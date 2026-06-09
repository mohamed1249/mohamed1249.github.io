---
title: "EDA: IMDB / TMDb — Deep Movie Analysis"
date: 2024-05-01
status: "Completed"
field: "EDA"
tools:
  - Python
  - Pandas
  - Plotly
  - Seaborn
  - Matplotlib
  - WordCloud
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/eda-imdb-deep-analysis"
excerpt: "A comprehensive deep-dive into ~5,000 TMDb movies — systematically profiling budget, revenue, profit, runtime, genres, keywords, cast, directors, production companies, and release timing across every major performance metric."
---

## The Short Version

The most ambitious EDA in my portfolio. This notebook analyzes the full TMDb 5,000 movie dataset across every dimension that matters to three audiences simultaneously: viewers looking for what to watch, producers trying to maximize profit, and data enthusiasts who just want to understand how the film industry works. Eleven feature categories. Seven performance metrics. A purpose-built library of reusable visualization functions. And a narrative framing device — three named character personas — that makes the findings readable rather than just comprehensive.

## Problem

What separates a blockbuster from a flop? Is it the genre, the cast, the director, the budget level, the release timing, or the production country? And depending on whether you care about profit, popularity, or critical rating, do those answers change? This project sets out to answer all of those questions from a single dataset — exhaustively.

## Approach

**Data Engineering** — The raw TMDb data arrives as two CSVs (movies + credits) that were merged on `id`. The most demanding part was parsing the heavily nested JSON-like string columns (`genres`, `keywords`, `cast`, `crew`, `production_companies`, `production_countries`). Two custom parsers were built:

- `clean_column()` — strips structural characters and splits into Python lists.
- `super_clean_column()` — a more powerful recursive parser that accepts a `key` and `master_key` to extract specific named fields from nested JSON strings (e.g., pulling only Director entries from the `crew` column).

A `profit` column was engineered (`revenue - budget`), and `release_date` was decomposed into year, season, month, day-of-month, and day-name for temporal analysis. Movies with zero budget, near-zero revenue, fewer than 10 votes, or outside the 60–250 minute runtime range were filtered as noise.

**Visualization infrastructure** — Rather than writing one-off charts, a full reusable function library was built:

- `dist()` — distribution histogram with mean, median, and mode lines overlaid.
- `through_time()` — time-series scatter with a 365-day rolling average trend.
- `tops_in_decades()` — bar chart of the highest-value movie per decade.
- `rel()` — Seaborn regression plot between two numeric columns.
- `RepetitionDF()` — counts item frequency across columns of lists (genres, cast, etc.).
- `most_by()` — averages a numeric metric across all items in a list column (e.g., average profit per genre).
- `percentages()` / `super_percentages()` — Plotly sunburst charts mapping co-occurrence between two list columns, optionally weighted by a numeric metric.
- `multi_through_time()` — stacked area chart showing how list-column items evolved year by year.
- `WC()` — WordCloud generator that can optionally weight words by a numeric metric (e.g., most common words in the top-200 profit movies).
- `mini_most_by()` — compact bar chart for flat categorical columns.

**Narrative structure** — Each finding is voiced by one of three named personas: Viona (the viewer), Richard (the producer), and Nuna (the curious reader). This separates analytical observations by their practical relevance and keeps a 200+ chart notebook readable.

**Coverage** — Every feature was profiled against every key metric (budget, revenue, profit, popularity, vote average, vote count, runtime):

Genres · Keywords · Original Language · Original Title · Overview · Tagline · Production Companies · Production Countries · Cast · Directors · Release Year · Season · Month · Day of Month · Day of Week

## Result

Selected findings across the three audience perspectives:

**For producers (Richard):**
- ~24% of released movies lose money. The median profit is only ~$2M despite an average of ~$80M — extreme right skew driven by blockbusters.
- Movie profits grew ~200% from 2006 to 2016; budgets are rising ~$8M per decade.
- **Low-budget path:** Comedy, Thriller, and Drama require the least capital and still generate strong average profits (~$73M for Comedy at ~$37M budget).
- **High-budget path:** Animation, Adventure, and Family generate the highest average revenues and profits when budget isn't a constraint.
- Comic/superhero keywords (`based on comic`, `marvel comic`, `superhero`) dominate both top-budget and top-profit word clouds — the clearest keyword signal in the dataset.
- For any given genre, the `super_percentages` charts map which specific keywords, countries, cast members, and directors correlate with the highest average profit in that genre.

**For viewers (Viona):**
- A movie with vote count > 5,000 and vote average > 6.5 is a statistically very safe watch.
- Popularity scores have nearly doubled every five years since 2011 — newer movies trend higher by design of the scoring system, so skew toward recent releases for that metric.
- For pure quality (vote average), Documentary, History, and War genres outperform despite low popularity. For crowd-pleasing watchability, Animation, Adventure, Science Fiction, and Fantasy dominate.
- Vote average has been *declining* by ~0.1 per decade on average — newer isn't always better rated.

**For the curious (Nuna):**
- Drama is the most-released genre of all time by a large margin, but the genres people actually love most are the ones that stimulate imagination — Animation, Sci-Fi, Adventure, Fantasy.
- The USA produced ~75% of all movies in the dataset; the UK is a distant second.
- The most frequent words in movie titles: *Man*, *World*, *Day*, *Love*, *Last*.
- Pirates of the Caribbean held the highest budget record through 2016; Avatar and Titanic held revenue and profit records.
- The most common day of release is Friday; summer is the most prolific season.

## What I Learned

Building the full function library before starting the analysis — especially `RepetitionDF`, `most_by`, and `super_percentages` — was the single highest-leverage decision in the project. Without it, analyzing cast, keywords, genres, and directors (all list-valued columns) against seven different metrics would have required hundreds of one-off loops. With it, each new question was a one-liner. The sunburst charts for genre × keyword × profit were the most practically useful output: they compress what would be a lookup table spanning thousands of combinations into a navigable visual that a producer or viewer can actually use. The three-persona narrative structure also proved worth the upfront design cost — it forces every finding to be connected to a real use case rather than just reported.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
