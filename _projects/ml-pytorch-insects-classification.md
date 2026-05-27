---
title: "ML: PyTorch Insects Image Classification"
date: 2024-03-13
status: "Completed"
tools:
  - Python
  - PyTorch
  - torchvision
  - Scikit-learn
  - Pandas
  - Matplotlib
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-pytorch-insects-image-classification"
excerpt: "A binary insect image classifier built in PyTorch from scratch — featuring a custom Dataset class, manual shape-tracing to compute layer dimensions, and a full train/eval loop with loss curve visualization."
---

## The Short Version

A PyTorch CNN built from scratch to classify images of two insect species — the two most image-rich classes in a 224×224 RGB dataset of insects with scientific names. The notebook goes deeper into the mechanics of CNN construction than any prior project: the architecture's fully-connected layer dimensions are derived by manually tracing a tensor through the convolutional stack before writing the model class, and a custom `Dataset` handles label encoding from directory names.

## Problem

Given images of insects labeled by scientific name, can a lightweight two-layer CNN learn to distinguish between the two most common species? The secondary question is architectural: how do you correctly compute the input size of the first fully-connected layer after stacked convolutions and pooling?

## Approach

**Dataset selection** — Rather than using all classes in the dataset, the two species with the most images were programmatically selected (`pd.Series(dct).sort_values().head(2)`), their folders copied to a working directory, and renamed to integer-encoded labels (0, 1) to feed directly into the custom loader.

**Custom `ImageFolderWithLabels` Dataset** — A hand-written `torch.utils.data.Dataset` subclass that walks the directory tree, extracts the integer label from each image's parent folder name, opens images as RGB with PIL, and applies transforms. This is more explicit than using `torchvision.datasets.ImageFolder` — a deliberate learning choice.

**Transforms** — Resize to 224 → ToTensor → ImageNet-standard normalization (mean `[0.485, 0.456, 0.406]`, std `[0.229, 0.224, 0.225]`) → RandomCrop back to 224. The normalization constants are the ImageNet defaults, which generalize well even for domain-shifted datasets.

**Shape tracing** — Before writing the `ConvolutionalNetwork` class, a single sample tensor was manually passed through each layer step-by-step to confirm the output shape at every stage:
- Conv2d(3→6, k=3) + ReLU → [1, 6, 222, 222]
- MaxPool(2,2) → [1, 6, 111, 111]
- Conv2d(6→16, k=3) + ReLU → [1, 16, 109, 109]
- MaxPool(2,2) → [1, 16, 54, 54]

This gives `ns = 16 × 54 × 54 = 46,656` input features to the first linear layer — derived empirically, not guessed.

**Architecture** — `ConvolutionalNetwork`: two conv blocks (Conv → ReLU → MaxPool) feeding into three fully connected layers (46,656 → 256 → 256 → 2), with `log_softmax` output. Trained with CrossEntropyLoss, Adam (lr=0.1), for 10 epochs, batch size 2.

**Training loop** — A manual epoch loop tracking train and test correct predictions per epoch, with loss printed every 15 batches. Loss curves for both train and validation were plotted after training. A `decode_y()` function maps predicted integer indices back to scientific species names for interpretable single-image inference.

## Result

The model converged over 10 epochs and correctly classified held-out test images. Loss curves showed the expected decreasing trend for both train and validation sets. Single-image inference at the end of the notebook correctly identified the species label by decoding the argmax prediction back through the label dictionary.

The batch size of 2 and aggressive learning rate (0.1) made training noisy on a per-batch basis — but the model still generalized to the binary classification task, which is achievable given the visual distinctiveness between the two selected species.

## What I Learned

Manually tracing a tensor through each layer before writing the model class — rather than calculating the flattened size algebraically — is one of the most practically useful habits in CNN development. It catches shape mismatches before training and makes the architecture's spatial compression explicit and visible. The custom `Dataset` class was also more instructive than using `ImageFolder`: writing `__len__` and `__getitem__` yourself forces a clear mental model of what a DataLoader is actually doing per batch. In hindsight, lr=0.1 is too aggressive for Adam on image classification — later projects use 0.001 — and batch size 2 makes loss curves noisy; but those are the exact lessons that only become clear by running the experiment and watching the training curve spike.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
