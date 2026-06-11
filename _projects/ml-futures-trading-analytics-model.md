---
title: "ML: Futures Trading Analytics Model"
excerpt: "A market microstructure experiment on ES futures block data, testing whether volume-profile and breakout features could predict directional price movement."
date: 2023-09-26
field: "ML"
tags:
  - Machine Learning
  - Financial Analytics
  - Time Series
  - XGBoost
  - Feature Engineering
tools:
  - Python
  - Pandas
  - Scikit-learn
  - XGBoost
  - KNN
  - Decision Tree
  - Random Forest
  - SVC
  - LSTM
links:
  - label: "GitHub Repository"
    url: "https://github.com/mohamed1249/Futures-Trading-Analytics-Model"
---

This project explored whether short-term structure inside ES futures blocks could reveal a predictive edge. The goal was not to build a flashy trading bot, but to test a very specific question: after a breakout bar closes, can the surrounding price, volume, and profile behavior help predict whether price reaches an 8-tick target before moving 12 ticks against the setup?

The work turned raw futures exports into a modeling dataset. I processed bar and tick-level market data, engineered features around buy and sell volume, block totals, high-volume price levels, breakout behavior, divergence, and candle direction, then labeled breakout cases according to the target/adverse-move logic.

## What I Built

- Prepared ES futures bar, block, and tick data for modeling.
- Engineered a feature set from volume-profile behavior, breakout candles, price levels, and block-level buy/sell pressure.
- Labeled thousands of breakout cases based on whether the market reached an 8-tick move before a 12-tick adverse move.
- Compared several machine learning approaches, including KNN, XGBoost, Decision Tree, Random Forest, SVC, and LSTM models.
- Tested feature-selection and feature-combination variants to see which groups carried the clearest signal.

## Result

The project found a modest but measurable signal, with the strongest clear result coming from an XGBoost model after feature selection:

- Accuracy around 58%
- Precision around 62%
- F1 score around 57%
- ROC AUC around 0.58

That result is interesting, but not enough by itself to claim a production trading edge. A real trading system would still need walk-forward validation, transaction-cost modeling, slippage assumptions, out-of-sample testing across different market regimes, and profitability simulation.

For me, the value of this project was in turning noisy market microstructure into a disciplined modeling problem: define the event, engineer the market context, label the outcome, test the signal, and stay honest about what the numbers actually say.

## Links

{% include project-links.html links=page.links %}
