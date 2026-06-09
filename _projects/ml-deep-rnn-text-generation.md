---
title: "AI: Deep RNN (LSTM) Text Generation on Moby Dick"
date: 2022-11-01
status: "Completed"
field: "AI"
tools:
  - Python
  - TensorFlow / Keras
  - spaCy
  - NumPy
  - Pandas
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-deep-rnn-for-text-generation"
excerpt: "Training a three-layer stacked LSTM on the first quarter of Moby Dick to build a character-level next-word text generator — the first sequence modeling project in the portfolio."
---

## The Short Version

A from-scratch LSTM text generation model trained on *Moby Dick*, built as a first exploration of sequence modeling with Keras. The pipeline covers spaCy tokenization, sliding-window sequence construction, Keras `Tokenizer` vocabulary encoding, a three-layer stacked LSTM with Dropout, early stopping, and a `generate()` inference function that autoregressively predicts one word at a time from a seed phrase. The generated text is honestly poor — and the notebook says so directly — but the architecture and pipeline are correct.

## Problem

Can a deep LSTM learn the statistical patterns of Melville's prose well enough to generate plausible text continuations? And what does it take to build a complete next-word generation pipeline from raw text to model to inference — from scratch?

## Approach

**Data** — *Moby Dick* loaded as a raw string. To fit within GPU RAM, only the first 25% of the text was used (`str_txt.split()[:int(len * 0.25)]`). An explicit note invites readers to remove the slice if they have more resources.

**Tokenization** — spaCy `en_core_web_sm` with `parser`, `tagger`, and `NER` disabled (only the tokenizer component needed). Tokens were lowercased and punctuation/whitespace characters filtered out. Disabling unused components is explicitly explained in the notebook — a good practice that reduces processing time significantly on long texts.

**Sequence construction** — A sliding window of `train_len = 21` tokens (20 context + 1 target) was applied across the full token list, producing one training sequence per token position. Each sequence's first 20 tokens become `X`; the 21st becomes `y`.

**Vocabulary encoding** — Keras `Tokenizer` fit on all sequences, converting words to integer indices. Target `y` was one-hot encoded with `to_categorical` across the full vocabulary (`voc_size + 1` classes).

**Architecture** — A Keras `Sequential` model:
```
Embedding(voc_size+1, seq_len, input_length=seq_len)
→ LSTM(seq_len×4, return_sequences=True) → Dropout(0.1)
→ LSTM(seq_len×9, return_sequences=True) → Dropout(0.2)
→ LSTM(seq_len×2)                        → Dropout(0.05)
→ Dense(voc_size+1, activation='softmax')
```
The three-stage LSTM funnel (4× → 9× → 2× sequence length units) progressively distills the sequence representation before the classification head. `return_sequences=True` on the first two LSTM layers passes the full hidden state sequence to the next layer.

**Training** — `categorical_crossentropy`, Adam, batch size 64, up to 1,000 epochs with `EarlyStopping(monitor='loss', patience=5, min_delta=0.001, restore_best_weights=True)`. Both the model (`.h5`) and tokenizer (`.pkl`) were saved after training.

**Inference** — A `generate(seed_txt, num_gen_wrds)` function autoregressively generates text: encode the seed → pad to `seq_len` → forward pass → `argmax` → decode index to word → append to input → repeat. Each predicted word becomes part of the next input context.

## Result

The model trained to convergence under early stopping. Generated text on two seed phrases — one from the novel itself, one a personal introduction — produced grammatically weak but structurally plausible word sequences. The notebook closes honestly: *"Of course, you can see how bad the generated text is, but that would be a lot better if we could train the model on more data."*

Quality limitations are attributable to three factors: only 25% of the novel was used for training; Moby Dick's archaic 19th-century vocabulary is a harder distribution to model; and a greedy `argmax` decoding strategy (vs. temperature sampling) produces repetitive outputs.

## What I Learned

The sliding-window sequence construction is the conceptual foundation of all sequence-to-sequence learning — the idea that "context window → next token" is the universal training signal for language models, from this LSTM up to modern transformers. Building it manually with a list comprehension before reaching for any abstraction made that foundation concrete. The `return_sequences=True` flag is the LSTM detail that most confuses beginners: the first two layers need it to pass the full hidden state sequence forward; the final LSTM layer doesn't, because only the last hidden state feeds the classification head. The honest quality assessment at the end is also worth keeping — the architecture is correct, the data is just insufficient. Knowing *why* the output is poor (training size, decoding strategy, domain vocabulary) is more useful than pretending the results were better than they were.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
