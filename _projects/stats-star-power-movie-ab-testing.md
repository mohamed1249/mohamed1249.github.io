---
title: "Stats: Star Power and Movie Success"
date: 2025-01-22
status: "Completed"
field: "Stats"
tools:
  - Python
  - Pandas
  - NumPy
  - SciPy
  - Seaborn
  - Plotly
  - Scikit-learn
  - XGBoost
  - Bayesian Testing
  - A/B Testing
github: "https://github.com/mohamed1249/Star-Power-Movie-AB-Testing"
demo:
kaggle:
excerpt: "A statistical and machine-learning study on TMDB movie data, testing whether star actors actually improve revenue, popularity, audience engagement, and profitability."
---

## The Short Version

This project asks a deceptively simple question: do star actors actually make movies more successful?

Using the TMDB 5000 movies and credits datasets, I built a full analysis around "star power" and tested its relationship with movie outcomes such as budget, revenue, popularity, runtime, audience rating, vote count, absolute profit, and profit percentage.

What makes the project important to me is that it does not stop at one method. It uses statistical testing, Bayesian reasoning, correlation analysis, partial correlation, and machine-learning models to look at the same question from several angles.

## Problem

In film, hiring a famous actor is expensive. The assumption is that stars bring attention, revenue, and prestige. But that assumption has tradeoffs:

- Do star-led movies earn more revenue?
- Do they receive more audience attention?
- Are they rated better?
- Do they actually make more profit?
- Or do higher actor and production costs reduce profit margins?

The project treats this as a decision-making problem for producers, investors, and analysts: when is star power commercially meaningful, and when is it just expensive?

## Approach

**Data preparation** - I joined the TMDB movies and credits datasets, parsed cast information from JSON-like fields, and built a clean movie-level dataset with financial, popularity, rating, runtime, and cast features.

**Feature engineering** - I calculated profit and profit percentage, then built actor-based features:

- `has_a_star`: whether a movie includes at least one top actor,
- `count_of_stars`: how many star actors appear in the movie,
- actor indicator features for model-based importance analysis.

**Star definition** - I focused on the top-billed actors, aggregated their movie popularity, vote counts, and movie counts, then selected the top 150 actors as "stars" for the main comparison.

**Traditional A/B testing** - Movies were split into two groups: with star actors and without star actors. I compared the groups using t-tests and Mann-Whitney U tests across budget, popularity, revenue, runtime, vote average, vote count, profit, and profit percentage.

**Bayesian A/B testing** - I used Bayesian group comparison to estimate posterior distributions, probabilities that one group outperforms the other, credible intervals, and effect sizes.

**Correlation and partial correlation** - I tested whether star presence still mattered after controlling for other variables such as budget, revenue, and profit percentage.

**Machine learning** - I tested how predictive star-related features are by training regression models, including Linear Regression, Gaussian Process Regression, and XGBoost.

## Result

The analysis found a clear but nuanced pattern.

Movies with star actors had much higher average:

- budget,
- popularity,
- revenue,
- vote count,
- absolute profit.

The traditional tests showed statistically significant differences across most metrics. The Bayesian results told the same story probabilistically: star-led movies were overwhelmingly more likely to outperform non-star movies on revenue, popularity, vote count, and profit.

But the most interesting result is the exception: **profit percentage**. Star-led movies earned more money in absolute terms, but they did not clearly produce better profit margins. In some analyses, the relationship between star power and profit percentage was weak or negative after accounting for revenue or profit.

That makes the business conclusion much more honest:

- Stars help maximize attention, scale, revenue, and audience engagement.
- Stars do not automatically make a movie more cost-efficient.
- If the goal is blockbuster reach, star power matters.
- If the goal is margin efficiency, smaller non-star productions may compete surprisingly well.

## Modeling Insight

The machine-learning section confirmed that star power is useful, but not sufficient.

Star count alone explained only a modest share of variance in outcomes. It was strongest for popularity, vote count, budget, and revenue, but weak for ratings, runtime, and profit percentage.

XGBoost with actor-level features improved some predictions, especially commercial metrics, and highlighted franchise-associated actors as important predictors. Still, the models showed that casting alone cannot explain movie success. Genre, marketing, franchise status, release timing, budget, distribution, and production quality all matter too.

## What I Learned

This project taught me how much better an analysis becomes when one question is examined through multiple lenses. A/B testing gives a statistical difference, Bayesian testing gives a probability and uncertainty, partial correlation checks confounding, and machine learning tests predictive usefulness.

The final answer is not a slogan like "stars are worth it" or "stars are overrated." It is more useful than that: stars are powerful for scale and attention, but expensive, and the margin story is much more complicated.

That is why I like this project. It feels like real data thinking.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
