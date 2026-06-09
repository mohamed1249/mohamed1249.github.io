---
title: "AI: IMDB Sentiment Classification with Fine-Tuned BERT"
date: 2024-07-14
status: "Completed"
field: "AI"
tools:
  - Python
  - PyTorch
  - Hugging Face Transformers
  - Scikit-learn
  - Pandas
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-imdb-review-classification-using-bert"
excerpt: "Fine-tuning bert-base-uncased on 10,000 IMDB movie reviews for binary sentiment classification — including a custom Dataset class, a linear warmup scheduler, and a reusable inference function."
---

## The Short Version

A full fine-tuning pipeline for `bert-base-uncased` on IMDB sentiment classification, built entirely from scratch in PyTorch using Hugging Face Transformers. The project goes beyond calling a high-level API — it implements a custom `Dataset`, a `BERTClassifier` module that wraps the pretrained encoder with a dropout + linear classification head, and separate `train()` and `evaluate()` functions that handle the full training loop. A `predict_sentiment()` inference function completes the end-to-end pipeline.

## Problem

Can a pretrained BERT model be fine-tuned on a sample of IMDB reviews to reliably classify sentiment as positive or negative? And what does it take to build that pipeline correctly from first principles — without relying on Hugging Face's high-level `Trainer` API?

## Approach

**Data** — The full 50,000-review IMDB dataset was loaded and capped at 10,000 reviews for training efficiency. Token length statistics were computed upfront (`count_tokens`) to inform the `max_length` choice — reviews can be very long, so truncation at 256 tokens was set as a deliberate tradeoff between context coverage and memory.

**Custom `TextClassificationDataset`** — A hand-written `torch.utils.data.Dataset` that tokenizes each review on the fly using `BertTokenizer`, with padding to `max_length` and truncation. Each `__getitem__` returns a dictionary of `input_ids`, `attention_mask`, and `label` tensors — the exact format BERT expects.

**`BERTClassifier` module** — A `nn.Module` wrapping `BertModel.from_pretrained('bert-base-uncased')` with:
- A `Dropout(0.1)` layer applied to the `[CLS]` pooled output.
- A `nn.Linear(768, 2)` classification head mapping from BERT's hidden size to binary logits.

This architecture follows the standard approach for fine-tuning BERT on sequence classification tasks.

**Training configuration:**
- Optimizer: `AdamW` (lr=2e-5) — the standard choice for fine-tuning transformers, with built-in weight decay.
- Scheduler: `get_linear_schedule_with_warmup` with no warmup steps — learning rate decays linearly from 2e-5 to 0 over the total training steps.
- 1 epoch (sufficient for BERT fine-tuning on this scale), batch size 64, `max_length` 256.

**Modular training loop** — Separate `train()` and `evaluate()` functions:
- `train()` handles the forward pass, CrossEntropyLoss, backpropagation, optimizer step, and scheduler step per batch.
- `evaluate()` runs inference under `torch.no_grad()`, collects predictions and labels, and returns both accuracy and a full `classification_report` (precision, recall, F1 per class).

**Model persistence** — The trained weights are saved to `bert_classifier.pth` with `torch.save`.

**Inference** — A `predict_sentiment()` function handles single-text prediction end-to-end: tokenize → move to device → forward pass → argmax → decode to "positive"/"negative". Tested on three example sentences including a clearly positive review, a clearly negative one, and a minimal three-word negative ("Worst movie of the year.") — all correctly classified.

## Result

The model trained for one epoch on 8,000 reviews and evaluated on 2,000 held-out reviews. The `classification_report` output confirms strong performance on both classes — BERT's pretrained language representations transfer effectively to sentiment classification even with minimal fine-tuning (1 epoch, 10K samples). The three qualitative inference tests at the end all produced correct predictions, including the terse "Worst movie of the year." — a case where a bag-of-words model would also succeed, but which confirms the pipeline is end-to-end functional.

## What I Learned

Building this pipeline without the Hugging Face `Trainer` API makes every design decision explicit: why `attention_mask` is passed alongside `input_ids` (to tell BERT which tokens are padding and which are real), why the `[CLS]` pooled output is used rather than the full sequence output (it's BERT's aggregate sentence representation), and why `AdamW` with linear decay is the standard recipe for transformer fine-tuning. The `max_length=256` truncation tradeoff is also worth understanding: BERT-base has a hard limit of 512 tokens, and IMDB reviews often exceed it — truncating to 256 saves memory but discards the second half of long reviews, which can carry sentiment signals. Increasing to 512 would be the first experiment in a next iteration, alongside training for 2–4 epochs as the standard recommendation for BERT fine-tuning.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
