---
title: "AI: LLM-Powered Product Recommendation System (davinci-instruct-beta)"
date: 2023-09-20
status: "Completed"
field: "AI"
tools:
  - Python
  - OpenAI API (davinci-instruct-beta)
  - LlamaIndex
  - LangChain
  - Pandas
  - Scikit-learn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-fine-tuning-openai-s-davinci-instruct-beta"
excerpt: "Three prompt engineering experiments testing whether OpenAI's davinci-instruct-beta can act as a product recommendation system on real e-commerce data from Bobcat in a Box — with an honest verdict: it can't, and here's exactly why."
---

## The Short Version

A real-world LLM experiment on proprietary data from Bobcat in a Box — a subscription box company operating across 42 countries. The goal was to test whether `davinci-instruct-beta` could predict customer product preferences (liked/disliked) by ingesting structured customer history as natural language context. Three prompt engineering strategies were designed, tested against 20–100 held-out samples, evaluated with classification metrics, and ultimately rejected. The notebook concludes with a direct verdict: for this task, the model doesn't work reliably — and documents exactly why each attempt failed.

## Problem

Can a large language model serve as a zero-shot or retrieval-augmented recommendation engine? Instead of training a traditional classifier, could you simply describe a customer's history and preferences in natural language — their past liked/disliked products, their keyword interests, their country — and ask the model whether they'd like a new product?

## Approach

**Dataset** — Nine tables from the Bobcat in a Box production database (Oct 2023), joined across 9 keys into a single customer-package record:
- `tbl_package` (267K rows): which SKU was sent, arrival and liked status.
- `tbl_keyword` + `tbl_keywordmapping`: product keyword vocabulary with synonym resolution (e.g., "horse" → "horses").
- `tbl_productkeyword`: SKU → keyword set mapping.
- `tbl_reqkeyword`: per-subscriber keyword interests.
- `tbl_similarSKU`: SKU-to-SKU similarity graph.
- `tbl_previewarchive` (2M rows): per-subscriber liked/disliked SKU sets from veto previews.
- `tbl_address`: subscriber country.

Each table was individually cleaned, aggregated into per-entity dictionaries (keywords per SKU, interests per subscriber, liked/disliked SKU sets per subscriber), then sequentially merged onto the package table. Memory was carefully managed — intermediate DataFrames were `del`-ed after each merge. The keyword synonym mapping was resolved into an `equals` column before joining.

After joining and filtering (removing feedback-free packages and non-delivery statuses), the dataset was balanced 1:1 (liked vs. disliked) and split: 80% train, 1,000-sample test set.

**LlamaIndex + davinci-instruct-beta** — Training data was serialized to JSON and indexed with `GPTVectorStoreIndex` via LlamaIndex, using `davinci-instruct-beta` as the LLM predictor. The index was persisted to disk and reloaded for inference. Test queries were submitted via `query_engine.query()`.

**Three prompt engineering experiments:**

*Test 1 — Natural language context + binary question:*
Training format: `"We've sent product {sku} to a customer in {country} and they liked/didn't like it, that product has keywords: {...}, the customer is interested in: {...}..."`
Test query: same context without the feedback + `"Do you think the customer would like that product?"`
**Conclusion:** Model always predicted "Yes." The model was positively biased by the question framing.

*Test 2 — Same context + probability question:*
Test query changed to: `"From 0% to 100%, give an exact rate of how much do you think the customer would like product {sku}?"`
Response parsing extracted the `%` token.
**Conclusion:** Model returned unrealistic or inconsistent percentages. Changed output format didn't resolve the underlying bias.

*Test 3 — Structured dict context + binary question:*
Training format changed from prose to key-value dictionary: `{"product ID": "...", "Customer country": "...", "Did the customer like the product?": "Yes/No", "sent product's keywords": [...], ...}`
Test query reverted to binary question. Response filtered to `Y`/`N` first character. Evaluated with `accuracy_score`, `precision_score`, `recall_score`, `F1`. Confusion matrix plotted.
**Conclusion:** Best result of the three — the model showed some signal and returned parseable Y/N responses for ~100 samples. But accuracy and F1 were not reliable enough for a production recommendation system.

## Result

| Test | Format | Conclusion |
|---|---|---|
| Test 1 | Prose context + binary question | Always predicted "Yes" — positive bias |
| Test 2 | Prose context + probability rate | Unrealistic/inconsistent rates |
| Test 3 | Dict context + binary question | **Best result** — some signal, but unreliable |

Final verdict (notebook's own words): *"I guess we can say that OpenAI's davinci-instruct-beta doesn't have what it takes to work as a recommendation system."*

The core failure mode: the model is a next-token predictor with extensive pretraining toward helpfulness — it defaults to agreeable, positive responses when asked whether a customer would "like" something, regardless of the retrieved context. A recommendation task requires a calibrated probability estimate over binary preferences, which is not what a generative LLM naturally produces without explicit fine-tuning for that output distribution.

## What I Learned

This is the most novel experimental design in the portfolio — using a vector-indexed LLM as a recommendation engine on real proprietary data is not a standard Kaggle approach. The multi-table join pipeline alone (nine sources, synonym resolution, per-entity set aggregation, 2M-row preview processing) was a substantial data engineering effort independent of the modeling. The three-test structure is the right way to document a negative result: each test changed exactly one variable (prompt format vs. output format vs. data serialization format), isolated the failure mode, and motivated the next design. The honest final conclusion — the model doesn't work for this task — is more valuable than a result that was massaged into looking successful. Understanding *why* it failed (generative bias toward helpfulness, no calibrated probability distribution) is a more useful takeaway than any accuracy number this approach could have produced.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
