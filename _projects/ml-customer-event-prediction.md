---
title: "ML: Customer Event Prediction for Subscription Retention"
date: 2023-06-06
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - TensorFlow
  - Keras
  - LSTM
  - Scikit-learn
  - ELI5
  - MAna
  - Plotly
  - Seaborn
github: "https://github.com/mohamed1249/Customer-Event-Prediction"
demo:
kaggle:
excerpt: "A client-style machine learning project for a food-box subscription business, predicting each customer's next subscription event and timing from historical event logs using a prepared tabular sequence pipeline and TensorFlow LSTM model."
---

## The Short Version

This project started as a churn-prediction task for a food-box subscription service: help the business see which customers might cancel, pause, reactivate, or change their subscription before it happens. The final local workflow moved beyond a simple yes/no churn flag and predicted the next customer event plus its expected date.

The project includes data preparation notebooks, encoded modeling datasets, saved preprocessing artifacts, an LSTM model, and a results file comparing real vs. predicted events.

## Problem

Subscription businesses do not only need to know whether a customer might leave. They need timing and context: who is likely to cancel, pause, reactivate, fail payment, create a new subscription, or change an existing one, and when that event may happen.

The original business goal was to support retention planning through future cancellation forecasts, weekly or monthly views, and drill-downs to individual customers for proactive communication.

## Approach

**Raw event data** - The project begins with a Boerschappen customer event export containing about 119,000 rows and 26 columns. The data captures customer IDs, event names, event timestamps, box information, delivery dates, delivery windows, order status, subscription status, and related box metadata.

**Data preparation** - The preparation notebook transforms the raw event history into a modeling table of about 52,000 rows with 19 columns. It creates event-count features, encodes categorical fields, prepares current and next box information, and calculates timing signals such as `time_since_last_event`.

**Modeling target** - Instead of treating churn as only a binary label, the model predicts a combined target representing the next customer event and timing. The target is label-encoded so the neural network can learn a multi-class prediction task.

**Model** - The main model is a TensorFlow/Keras LSTM stack with dropout layers. The workflow balances the training data, splits it into training and validation sets, reshapes the feature matrix for LSTM input, trains the model, and decodes predictions back into human-readable event and date outputs.

**Interpretability and checks** - The notebook includes permutation-importance style analysis, confusion-matrix visualizations, and separate accuracy checks for event prediction and date prediction.

## Result

The final results table contains 810 validation predictions comparing:

- Real event vs. predicted event.
- Real date vs. predicted date.
- Customer-level predicted subscription behavior.

On the saved validation results, the model reached:

| Metric | Score |
|---|---:|
| Event accuracy | 85.9% |
| Date accuracy | 62.7% |
| Exact event + date match | 60.7% |

The strongest outcome is the event prediction itself: the model often identifies the correct type of customer action even when the exact date is harder to predict. That makes the project useful as an early-warning retention signal rather than a perfect calendar oracle.

## What I Learned

Customer churn is rarely one clean label. In real subscription data, cancellation sits inside a wider event system: failed payments, pauses, reactivations, subscription changes, new orders, and delivery timing all shape what the customer does next.

The project also made the tradeoff clear between business usefulness and technical certainty. Predicting exact dates is difficult, but predicting a likely next customer action can still give a business valuable time to intervene.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
