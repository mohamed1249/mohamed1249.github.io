---
title: "Tool: Dockerized PyTorch CNN Classifier API"
date: 2024-06-22
status: "Prototype"
field: "Tool"
tools:
  - Python
  - PyTorch
  - torchvision
  - Flask
  - Docker
  - Pillow
  - CNN
github: "https://github.com/mohamed1249/PyTorch-CNN--Dockerized-"
demo:
kaggle:
excerpt: "A Dockerized Flask API that serves a trained PyTorch CNN for image classification, turning a notebook-trained model into a local inference service."
---

## The Short Version

This project takes a trained PyTorch convolutional neural network and wraps it in a small Flask API, then packages the service with Docker. It is connected to my earlier PyTorch insect-classification work, but the focus here is deployment: moving from a trained model file to an endpoint that can accept an image and return a predicted class.

It is a practical bridge between model experimentation and usable machine-learning software.

## Problem

Training a model in a notebook is only the first part of the work. To make the model useful, it needs a repeatable way to run inference:

- load the trained model,
- preprocess incoming images the same way training images were prepared,
- run prediction without gradients,
- decode the output class,
- expose the prediction through an API,
- package the environment so it can run on another machine.

## Approach

**CNN model** - The API recreates the same PyTorch CNN architecture used during training: two convolution and pooling blocks followed by fully connected layers and a log-softmax output.

**Model loading** - The trained weights are loaded from `model_state_dict.pt`, then the network is switched to evaluation mode before serving predictions.

**Image preprocessing** - Uploaded images are opened with Pillow, resized to `224 x 224`, converted to tensors, and normalized with ImageNet-style mean and standard deviation values.

**Flask endpoint** - The app exposes a `/classify` route that accepts an uploaded image file, runs the model under `torch.no_grad()`, and returns the decoded class as JSON.

**Docker packaging** - The Dockerfile builds from a Python 3.9 slim image, installs PyTorch, torchvision, Flask, and Pillow, exposes the Flask port, and starts the API inside the container.

## Result

The result is a small image-classification inference service:

- trained PyTorch weights saved locally,
- Flask API for image upload,
- preprocessing pipeline for inference,
- JSON prediction response,
- Dockerfile for containerized execution.

As a portfolio project, it shows the deployment side of machine learning: not only building a CNN, but wrapping it in a service that another program can call.

## What I Learned

This project helped connect deep-learning notebooks with backend engineering. Serving a model forces a different kind of thinking: inputs need validation, preprocessing must match training, model files must load reliably, and class labels need to stay consistent between training and inference.

The next improvements would be to add a `.dockerignore`, pin dependency versions, keep only the required model and class-label files in the Docker build context, add a health-check route, and store labels in an explicit file instead of relying on folder order.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
