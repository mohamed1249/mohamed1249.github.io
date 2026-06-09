---
title: "Observational Data vs. Experimental Data"
date: 2025-05-30
status: "Completed"
field: "Stats"
tools:
  - Python
  - NumPy
  - Pandas
  - SciPy
  - Scikit-learn
  - Matplotlib
  - Seaborn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/observational-data-vs-experimental-data"
excerpt: "A simulation-driven reference notebook covering the full distinction between observational and experimental data — confounding, Simpson's Paradox, A/B test vs. observational bias, and statistical power — each illustrated with live-generated synthetic data."
---

## The Short Version

A structured reference notebook that teaches the difference between observational and experimental data not through definitions but through five working simulations. Each example generates synthetic data that demonstrates a real statistical phenomenon — confounding, Simpson's Paradox, selection bias in A/B tests, and statistical power curves — then visualises and quantifies the effect. This is conceptual infrastructure for the rest of the portfolio: every project that involves causal reasoning, experimental design, or A/B testing connects back to the distinctions established here.

## Problem

Most ML practitioners know that correlation doesn't imply causation — but fewer can demonstrate *exactly how* confounding inflates correlations, *why* Simpson's Paradox reverses aggregate findings, or *how much* selection bias overestimates A/B test lift. This notebook makes all three concrete with runnable code and numbers.

## Approach

Five simulations, each self-contained:

**Example 1 — Confounding (education → income & health):**
Synthetic data where education drives both income and health score. Raw income-health correlation: ~0.6. After computing the partial correlation (removing the education signal from both variables via residualisation), the relationship collapses — revealing that the apparent income-health association is almost entirely explained by education. Four-panel visualisation: raw scatter, education→income, education→health, and income vs. health stratified by education level.

**Example 2 — Randomised Controlled Trial (drug → blood pressure):**
500 treatment / 500 control patients randomly assigned. Randomisation check: age and baseline BP are near-identical between groups — the expected result. Treatment effect estimated by two-sample t-test. Visualisation: overlapping histograms confirming group balance, boxplot showing the 5-point BP reduction. Demonstrates exactly *why* randomisation enables causal inference: all confounders are balanced by design.

**Example 3 — Simpson's Paradox (kidney stone treatment):**
Hospital A treats mostly small stones (87 cases) with a few large (13 cases); Hospital B reverses that ratio. Hospital A has a *lower overall success rate* than Hospital B — but a *higher success rate for both small and large stones separately*. The paradox arises because Hospital A's case mix is skewed toward the easier cases (small stones), but its proportion is small enough that the harder-case majority from Hospital B dominates the aggregate. Side-by-side bar charts make the reversal immediately visible.

**Example 4 — A/B test vs. observational analysis (website conversion):**
The same true treatment effect (2 percentage point lift) is estimated two ways: via a properly randomised A/B test (correct estimate), and via observational analysis where high-engagement users are more likely to both see the new design and convert (biased estimate). The observational analysis overestimates the lift because engagement is an unmeasured confounder. Logistic regression controlling for engagement partially recovers the true effect. Four-panel visualisation: A/B results, biased observational results, engagement distribution showing selection bias, engagement vs. conversion scatter.

**Example 5 — Power analysis:**
Statistical power curves plotted for small (d=0.2), medium (d=0.5), and large (d=0.8) effect sizes across sample sizes from 50 to 1,000 per group. Annotated with the approximate sample sizes needed to reach 80% power for each effect size. The key message: detecting small effects experimentally requires hundreds of subjects per group — which is why observational studies (with large natural samples) often find associations that underpowered experiments miss.

## Result

Each simulation produces concrete numbers that make the abstract concrete:

- Raw income-health r=0.6 collapses toward 0 after controlling for education.
- Randomised groups differ by <0.1 years in age and <0.1 mmHg in baseline BP — confirming randomisation worked.
- Hospital A outperforms Hospital B for *both* stone sizes but loses in aggregate — a clean Simpson's Paradox demonstration.
- Observational A/B analysis overestimates lift because engaged users self-select into the new design — quantified by comparing raw vs. regression-adjusted estimates.
- Small effects (d=0.2) require ~800 subjects per group for 80% power; large effects (d=0.8) need only ~50.

## What I Learned

Building these simulations forced precise thinking about causal mechanisms — you can't simulate confounding without specifying exactly which variable causes what, in what direction, and with what magnitude. The Simpson's Paradox demonstration in particular required getting the case-mix proportions right: the paradox only appears when the confounder (stone size) is distributed differently across groups. The A/B test vs. observational simulation is the most practically relevant: it makes the cost of self-selection bias tangible and visible, not just described. Together, these five examples form a mental toolkit that applies directly to every project in this portfolio that touches experimental design, metric selection, or causal reasoning.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
