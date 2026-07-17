---
title: "Tool: RNN Time-Series Forecasting Gradio UI"
date: 2025-01-29
status: "Prototype"
field: "Tool"
tools:
  - Python
  - PyTorch
  - Gradio
  - pandas
  - scikit-learn
  - Matplotlib
  - RNN
  - LSTM
  - GRU
github:
demo:
kaggle:
excerpt: "A Gradio interface for training PyTorch RNN, LSTM, or GRU forecasting models on uploaded time-series CSV files, then returning MAE, downloadable model weights, prediction plots, and future forecasts."
---

## The Short Version

This project turns a PyTorch time-series forecasting notebook into an interactive Gradio tool. A user uploads a CSV, chooses the time-series column, selects an RNN family model, configures the training settings, and gets back a trained model file, MAE, actual-vs-predicted plots, and a table of future predictions.

It came out of earlier RNN experiments on historical price data, then grew into a reusable interface for testing recurrent models on different one-dimensional time series.

## Problem

Notebook forecasting experiments are useful, but they are easy to trap inside one dataset and one hard-coded configuration. I wanted a small interface that could make the workflow more reusable:

- upload a new time-series CSV,
- choose the column to forecast,
- optionally scale the values,
- configure lags, batch size, hidden size, layers, epochs, and learning rate,
- choose between RNN, LSTM, and GRU,
- train the model,
- export the trained weights,
- visualize how predictions compare with actual values,
- generate a short future forecast.

## Approach

**Dataset wrapper** - A custom `TimeSeriesDataset` converts a one-dimensional series into supervised windows: a sequence of previous values as input and the next values as the target. The default setup uses a lag window and predicts multiple future steps.

**Model options** - The tool implements three PyTorch sequence models with the same interface:

- vanilla `nn.RNN`,
- `nn.LSTM`,
- `nn.GRU`.

Each model passes the final recurrent output through a linear layer to produce the forecast horizon.

**Training loop** - The app trains with Adam and `L1Loss`, then reports the final MAE-like loss value. The training loop keeps the model/device handling explicit, including optional GPU usage.

**Forecasting workflow** - After training, the tool:

- predicts across the available historical data,
- extracts actual aligned values,
- optionally inverse-transforms scaled predictions,
- predicts future values from the most recent sequence,
- returns both charts and a future-prediction table.

**Gradio interface** - The UI exposes the workflow through text inputs and file upload fields, then returns:

- MAE,
- downloadable `.pth` model weights,
- actual-vs-predicted plot,
- future predictions table,
- future forecast plot.

## Result

The result is a small forecasting workbench rather than a single static notebook. It can train an RNN-style model from a CSV and immediately produce artifacts that are easier to inspect or reuse.

The original experiment used historical price data with a 7-step forecast horizon. Later versions generalized the workflow so the same interface could test RNN, LSTM, and GRU configurations on another uploaded time series.

## What I Learned

This project made the difference between "model code" and "usable modeling workflow" much more obvious. Even a simple forecasting app needs clean boundaries:

- data loading and sorting,
- scaling and inverse-scaling,
- window generation,
- model selection,
- training,
- inference,
- plotting,
- artifact export.

It also showed why recurrent models are sensitive to configuration. Sequence length, hidden size, number of layers, scaling, and forecast horizon all change the behavior of the model. Putting those controls into a UI made experimentation faster and made the assumptions more visible.

## Links

- Related LinkedIn note: [Deep learning, PyTorch, and TensorFlow](https://www.linkedin.com/posts/muhammad-abdulsalam-b127641b9_deeplearning-pytorch-tensorflow-activity-7241510203697844224-dp93)
