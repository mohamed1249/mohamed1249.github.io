---
title: "ML: Simple Neural Network with PyTorch"
date: 2024-01-22
status: "Completed"
field: "ML"
tools:
  - Python
  - PyTorch
  - Pandas
  - Scikit-learn
  - Matplotlib
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-simple-neural-network-with-pytorch"
excerpt: "A from-scratch PyTorch feedforward neural network for binary tabular classification — a focused exercise in building and training a custom nn.Module by hand."
---

## The Short Version

A clean, from-scratch implementation of a feedforward neural network in PyTorch for binary classification on the Kaggle Tabular Playground Series (November 2021). No high-level wrappers, no callbacks — just a custom `nn.Module`, a manual training loop, a loss curve, and a test accuracy evaluation. This sits in the portfolio as a foundational PyTorch reference: the point where the mechanics of building, training, and evaluating a neural network by hand were first put together cleanly.

## Problem

Binary classification on a 100-feature tabular dataset with 500,000+ rows. The objective was less about squeezing out accuracy and more about getting the PyTorch training pipeline right: GPU detection, tensor dtype handling, forward pass, backward pass, optimizer step, and evaluation without gradient tracking.

## Approach

**Architecture** — A custom `Model` class subclassing `nn.Module`, with four linear layers expanding then contracting the feature space:
`Linear(100 → 200) → ReLU → Linear(200 → 400) → ReLU → Linear(400 → 200) → ReLU → Linear(200 → 2)`

The widening-then-narrowing funnel shape (100 → 200 → 400 → 200 → 2) gives the network capacity to learn feature interactions before compressing to the 2-class output.

**Training** — `CrossEntropyLoss`, Adam (lr=0.01), 100 epochs. The training loop follows the standard PyTorch pattern: forward pass → loss → `zero_grad()` → `backward()` → `optimizer.step()`. Loss was appended each epoch and plotted as a curve after training.

**Evaluation** — Test predictions generated under `torch.no_grad()` by looping through each test sample, running a forward pass, and taking `argmax()` of the output logits. Final accuracy reported with `sklearn.metrics.accuracy_score`.

**GPU handling** — Device detection at the top (`cuda` if available, else `cpu`), with all tensors and the model moved to the selected device. Both `X` tensors cast to `FloatTensor` and `y` tensors to `LongTensor` — the correct dtypes for `CrossEntropyLoss` input and target respectively.

## Result

The model converged smoothly over 100 epochs with a clean decreasing loss curve. Test accuracy reflects a working binary classifier on the Tabular Playground benchmark. The focus of this notebook was correctness of implementation rather than leaderboard performance — no regularization, early stopping, or hyperparameter tuning was applied.

## What I Learned

This notebook established the mental model for all subsequent PyTorch work. The two dtype decisions — `FloatTensor` for features, `LongTensor` for labels — are easy to get wrong and cause confusing errors; getting them right here meant they were automatic in later projects. The `zero_grad()` → `backward()` → `step()` sequence and the `torch.no_grad()` evaluation block are the two patterns that appear in every training loop from the insect classifier through to BERT fine-tuning. Building them manually once, without a Trainer wrapper, made them second nature. Compared to later notebooks like the Music Trends ANN (which adds early stopping, gradient clipping, and weight decay) or the BERT classifier, this is deliberately minimal — its value is in being the first clean implementation, not the most complete one.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
