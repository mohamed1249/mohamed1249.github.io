---
title: "AI: ASL Recognition with TinyVGG & Custom CNN (100% Accuracy)"
date: 2024-01-27
status: "Completed"
field: "AI"
tools:
  - Python
  - PyTorch
  - torchvision
  - torchinfo
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-asl-tinyvgg-custom-cnn-classifiers-100-acc"
excerpt: "Two PyTorch CNNs trained to classify 29 American Sign Language hand gestures from 87,000 images — TinyVGG achieves 96.5% accuracy; a deeper custom CNN reaches 100%."
---

## The Short Version

A computer vision project that trains two convolutional neural networks from scratch to recognize all 29 classes of the ASL alphabet (26 letters + space, delete, nothing) from 87,000 images. The first model follows the TinyVGG architecture for a clean baseline. The second is a deeper custom CNN built using a flexible layer-specification system — and achieves perfect 100% test accuracy in 10 epochs. The project is aimed at real-world assistive technology: bridging communication for the deaf and hard-of-hearing community.

## Problem

Can a relatively small CNN trained from scratch — without transfer learning — reliably classify ASL hand gestures across all 29 classes? And how much does architectural depth matter when the baseline already reaches 96.5%?

## Approach

**Dataset** — The [ASL Alphabet dataset](https://www.kaggle.com/datasets/grassknoted/asl-alphabet) contains 87,000 200×200 RGB images across 29 classes (3,000 per class for training, 1 per class for testing). The test images were unorganized flat files and were reorganized into per-class subdirectories programmatically before loading.

**Preprocessing** — Training images were augmented with random horizontal flipping and `TrivialAugmentWide` (a randomized policy-based augmentation strategy). Test images were resized and normalized only. An 80/20 split of the training set was used for train/validation, giving ~69,600 training and ~17,400 validation images. DataLoaders used batch size 64 with all available CPU workers.

**Model 1 — TinyVGG:** Two convolutional blocks (Conv → ReLU → Conv → ReLU → MaxPool), each with 10 hidden channels, feeding into a flattened linear classifier. Lightweight and interpretable — a deliberate starting point. Trained for 10 epochs with Adam (lr=0.001) and CrossEntropyLoss.

**Model 2 — Custom CNN:** A deeper architecture specified via a human-readable layer list (`cnn_layers`) passed into a general-purpose `CNN` class that dynamically constructs a `nn.Sequential` stack. The architecture is: two conv blocks with 32 channels → two conv blocks with 64 channels → MaxPool after each pair → Flatten → Linear(64×50×50, 128) → ReLU → Linear(128, 29). The `CNN` class supports Conv2d, ReLU, MaxPool2d, Dropout, BatchNorm2d, Tanh, Sigmoid, LeakyReLU, Flatten, and Linear — making it reusable for any future architecture without writing a new class. Same training config as TinyVGG.

**Infrastructure** — A clean modular training loop was built with separate `train_step()`, `val_step()`, and `train()` functions. Loss curves (train + val, loss + accuracy) were plotted after each run. A `pred_and_plot_image()` function handles single-image inference end-to-end: load → normalize → unsqueeze → forward pass → softmax → argmax → plot with predicted class and confidence.

## Result

| Model | Test Accuracy |
|---|---|
| TinyVGG (10 hidden units, 2 conv blocks) | **96.5%** |
| Custom CNN (32→64 channels, 4 conv blocks) | **100%** |

Both models were trained for exactly 10 epochs from scratch with identical optimizer settings. The custom CNN achieved perfect classification on all 29 held-out test images. Single-image predictions on letters M, A, and the `space` gesture were all correct with high confidence.

## What I Learned

The `CNN` class built around a layer-spec list is the most reusable piece of code in this project — defining a new architecture becomes a matter of editing a Python list rather than writing a new class. The jump from 96.5% to 100% came entirely from adding depth and width (10 → 32/64 channels, 2 → 4 conv blocks), with no changes to the training setup — a clear signal that the TinyVGG bottleneck was representational capacity rather than overfitting or optimization. The `TrivialAugmentWide` augmentation strategy is also worth noting: it applies a randomly sampled augmentation policy per batch rather than a fixed set of transforms, which meaningfully reduces the risk of the model overfitting to a specific augmentation pattern — important at 87,000 images where the same sign can look very similar across samples.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
