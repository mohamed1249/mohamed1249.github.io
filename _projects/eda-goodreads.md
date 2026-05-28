---
title: "EDA: Goodreads — What Makes a Perfect Book?"
date: 2022-10-10
status: "Completed"
tools:
  - Python
  - Pandas
  - Seaborn
  - Matplotlib
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/eda-goodreads"
excerpt: "21 questions answered across 11,000+ Goodreads books — from top authors and publishers to a custom popularity score that combines average rating and read count into a single fair ranking metric."
---

## The Short Version

An exploratory analysis of 11,000+ Goodreads books covering titles, authors, ratings, page counts, languages, publishers, and publication years. The notebook answers 21 structured questions for two audiences — readers trying to find what to pick up next, and publishers trying to understand what succeeds. The centerpiece is a hand-crafted composite popularity score that addresses a real flaw in using raw average ratings to rank books.

## Problem

What makes a book popular — and how do you measure it fairly? Raw average rating rewards obscure books with a handful of perfect scores. Pure read count rewards old blockbusters with millions of accumulated ratings. Neither alone gives a fair picture. The project asks 21 questions about books and authors, with the popularity ranking problem as the analytical core.

## Approach

**Cleaning** — The dataset had four issues addressed before analysis:
- `authors` column contained co-author entries separated by `/` (4,562 books affected) — only the first author was retained to avoid duplicate-identity problems in author-level aggregations.
- `publisher` column had the same `/` pattern — cleaned identically.
- `num_pages` had a leading whitespace in the column name — stripped via rename.
- Books with `num_pages ≤ 20` or `ratings_count ≤ 100` were filtered as noise (zero-page entries and unreviewed books).

**Custom popularity score** — A key insight: `average_rating` values cluster around 3.5–4.5, while `ratings_count` values range from hundreds to millions — they're on incomparable scales. Rather than normalizing, the score raises `average_rating` to the 8th power (bringing it to ~65,000 for a 4.0-rated book, competitive with rating counts) then multiplies by `ratings_count` and scales down by 10⁵:

`real_best_books = (average_rating⁸ × ratings_count) / 10⁵`

This single formula rewards books that are both widely read *and* highly rated — neither metric can dominate alone.

**21 questions answered**, grouped by theme:
- *Popularity:* Top 10 books, top 10 authors (by composite score and by raw read count separately), top 10 publishers.
- *Productivity:* Top 10 most prolific authors by book count.
- *Language:* Most common languages, average rating per language, longest books by language.
- *Size:* Top 10 longest books, top 10 authors with the longest average book length.
- *Engagement:* Top 10 most text-reviewed books and authors, correlations between page count/text reviews/ratings count and average rating.
- *Time:* Average rating trend by year, average read count by year, best-rated book per publication year.

## Result

Key findings:

- **Most popular books:** Harry Potter series dominates the composite score — high ratings *and* massive read counts.
- **Most popular author:** Stephenie Meyer tops the composite score (driven by Twilight's enormous ratings count).
- **Most prolific author** ≠ **most popular author** — the two rankings share almost no overlap, confirming that volume and quality are independent.
- **Most read book:** Twilight by a significant margin in raw ratings count.
- **Latin-language books** have the highest average ratings — a small but highly rated sample.
- **All books with 1M+ ratings** have an average rating above 3.5 — popularity and quality are correlated at the extremes.
- **All books over 1,500 pages** have average ratings above 4.0 — readers who commit to long books rate them highly.
- **Text reviews and ratings count** are strongly correlated (the one relationship the regression plots confirmed as positive and strong).
- **Average ratings are declining slightly** over publication years — newer books tend to be rated lower, possibly due to recency effects and incomplete accumulation.
- An outlier spike in average read count around 1952 traced back to a single book: *A Streetcar Named Desire* — flagged, investigated, and explained inline.
- **Most popular publisher:** Arthur A. Levine Books (home of the Harry Potter US editions).

## What I Learned

The composite scoring formula is the most creative analytical decision in this notebook — and the reasoning behind it (the scale mismatch between ratings and counts, solved by exponentiation rather than normalization) is the kind of problem that doesn't have a textbook answer. It required thinking about what the metric *means* rather than just what's available. The notebook also demonstrates a consistent habit of challenging the obvious answer: when `sort_values('average_rating').head(10)` produced an implausible list of obscure books, the response was to ask *why* and redesign the metric rather than accept a misleading result. The outlier investigation on the 1950s read-count spike is another example of the same instinct — a chart anomaly treated as a question to answer, not noise to ignore.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
