---
title: "EDA: Traditional vs. Bayesian A/B Testing"
date: 2025-05-03
status: "Completed"
field: "Stats"
tools:
  - Python
  - Pandas
  - SciPy
  - NumPy
  - Matplotlib
  - Seaborn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/eda-traditional-vs-bayesian-a-b-testing"
excerpt: "A side-by-side implementation of Traditional and Bayesian A/B testing on the Cookie Cats mobile game dataset — two retention metrics, two statistical paradigms, one decision."
---

## The Short Version

A methodological comparison of two approaches to A/B testing applied to the Cookie Cats mobile game experiment — where a gate was moved from level 30 to level 40. Both approaches test the same question (does the gate position affect 1-day and 7-day player retention?) but answer it differently: Traditional testing via t-test and chi-square gives a p-value and a binary reject/fail-to-reject decision; Bayesian testing via a Beta-distribution bandit model gives a posterior probability distribution over retention rates for each variant.

## Problem

When running an A/B test, which statistical paradigm gives you a better answer — and a more useful one? The Traditional approach is widely understood but gives only a p-value threshold decision. The Bayesian approach quantifies the full probability that one variant is better than the other, and updates continuously as evidence accumulates. Both were implemented from scratch and applied to the same experiment.

## Approach

**Dataset** — Cookie Cats (`cookie_cats.csv`): 90,189 players split between `gate_30` (N=44,700) and `gate_40` (N=45,489), with binary 1-day and 7-day retention outcomes.

**Traditional A/B Testing (`traditional_ab_test()`):**
- Means, standard deviations, and two-sample t-test (`scipy.stats.ttest_ind`) for both retention metrics.
- A chi-square contingency test (`chi2_contingency`) comparing retained vs. churned user counts between variants as a proportion test.
- Decision rule: p < 0.05 → statistically significant; recommend the higher-mean variant.
- A `recommend_variant()` function encodes this logic cleanly.

**Bayesian A/B Testing (`VersionBandit` + `bayesian_bandit_experiment()`):**
- A `VersionBandit` class models each game version as a Beta-distribution bandit with a uniform prior (α=1, β=1). `sample_posterior()` draws from `Beta(α + cumulative_reward, β + pulls - cumulative_reward)`; `update(reward)` increments pulls and rewards.
- A Monte Carlo simulation runs 10,000 rounds: at each step, both bandits are sampled, the higher-scoring version is selected, and its bandit is updated with a reward of 1. This is Thompson Sampling — a standard Bayesian bandit algorithm.
- After simulation, the posterior reward distributions for both variants are plotted as overlapping histograms — showing not just which version is better but by how much and with what certainty.
- Run separately for `retention_1` and `retention_7`.

## Result

**1-day retention:**

| Version | Retained | Total | Rate |
|---|---|---|---|
| gate_30 | 20,034 | 44,700 | 44.82% |
| gate_40 | 20,119 | 45,489 | 44.23% |

Chi-square p-value: not significant at α=0.05 → "Insufficient evidence to choose between variants."

**7-day retention:**

| Version | Retained | Total | Rate |
|---|---|---|---|
| gate_30 | 8,502 | 44,700 | 19.02% |
| gate_40 | 8,279 | 45,489 | 18.20% |

Chi-square p-value: significant (p < 0.05) → "Confidently choose gate_30."

The Bayesian posterior plots for both metrics showed the gate_30 distribution shifted slightly rightward — consistent with the Traditional result but expressed as a probability mass over possible retention rates rather than a binary threshold decision.

**Recommendation: keep the gate at level 30.** On the metric that matters most (7-day retention, which captures longer-term engagement), gate_30 wins significantly by both paradigms.

## What I Learned

The most important conceptual takeaway is what each paradigm actually outputs. A p-value answers: "If there were no real difference, how likely is this data?" — a confusingly indirect statement. A Bayesian posterior answers: "Given this data, what is the probability distribution over the true retention rate?" — directly interpretable. The Thompson Sampling implementation makes the Bayesian advantage tangible: rather than waiting for a fixed sample size, it continuously allocates more simulated traffic to the better-performing variant while still exploring the other. The 1-day vs. 7-day result divergence is also worth noting: the two metrics tell different stories about the same experiment, and the choice of primary metric would have determined the decision if only one had been measured. This is why pre-registering your primary metric before running an A/B test is standard practice.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
