---
title: "AI: Diagnosify Arabic Medical Symptom Assistant"
date: 2025-03-31
status: "Prototype"
field: "AI"
tools:
  - Python
  - Flask
  - Streamlit
  - XGBoost
  - Scikit-learn
  - Pandas
  - SQLite
  - Fuzzy Matching
  - OpenAI
  - Pinecone
  - RAG
github: "https://github.com/mohamed1249/Diagnosify-Arabic-Medical-Symptom-Assistant"
demo:
kaggle:
excerpt: "An Arabic medical-assistant prototype that combines symptom-based disease prediction, conversational symptom extraction, and a RAG-style medical knowledge layer."
---

## The Short Version

Diagnosify is an Arabic medical-assistant prototype built around symptom understanding and disease prediction. It started as a graduation-project build I developed and guided, then grew into a small system with machine-learning diagnosis, a chatbot-style API, symptom storage, and an experimental RAG layer for medical question answering.

The important idea is not to replace a doctor. The project is a learning and assistance tool: it shows how structured symptom data, Arabic user input, classification models, and retrieval-based AI can work together in a careful health-tech prototype.

## Problem

Medical symptom checkers are difficult because users do not always describe symptoms in clean, model-ready language. They may write informally, in Arabic, with spelling variation, missing symptoms, or vague descriptions.

The project explored how to build a system that can:

- understand Arabic symptom input,
- match user text to known symptoms,
- predict a likely disease class from symptom vectors,
- keep track of unknown or newly entered symptoms,
- provide cautious medical guidance with clear safety limits,
- support a future knowledge-based chat layer.

## Approach

**Arabic symptom dataset** - The project uses a translated SymbiPredict-style symptom dataset with 4,961 records, 132 symptom columns, and 42 disease labels. The data was prepared into disease-specific datasets so each condition could be tested against its relevant symptoms.

**Disease prediction** - The core model uses XGBoost for symptom-based classification. Symptoms are represented as binary features, and disease names are label-encoded for model training and prediction.

**Disease-specific models** - One version of the project trains separate XGBoost classifiers per disease, allowing the Streamlit interface to ask only about symptoms relevant to the selected condition.

**Arabic chatbot API** - A Flask API accepts Arabic symptom descriptions, extracts possible symptoms with fuzzy matching, builds the model input vector, and returns a predicted condition with a cautionary medical disclaimer.

**Unknown symptom logging** - The API uses SQLite to store symptoms that are not already recognized, making the prototype feel like a system that can learn from gaps in the vocabulary over time.

**RAG medical assistant layer** - A later branch experiments with OpenAI embeddings, Pinecone vector search, and a fixed safety-focused medical prompt. The assistant retrieves relevant medical context before generating a response, while reminding the user that it is not a licensed medical professional.

## Result

The result is a multi-part prototype for Arabic medical assistance:

- a translated Arabic symptom dataset,
- XGBoost disease prediction,
- a Streamlit symptom-checking interface,
- a Flask chatbot API,
- fuzzy symptom matching,
- SQLite logging for new symptoms,
- an experimental OpenAI/Pinecone RAG layer.

As a portfolio project, I like it because it is not only model training. It touches the whole shape of an applied AI product: data preparation, Arabic language handling, model serving, user input ambiguity, interface design, and responsible AI boundaries.

## What I Learned

This project made the human side of AI more visible. A medical assistant is not just a classifier; it has to be cautious, understandable, and honest about uncertainty. It also has to handle language the way people actually use it, not only the way a clean dataset stores it.

Technically, it strengthened my work with Arabic data, fuzzy matching, XGBoost classification, Flask APIs, Streamlit interfaces, and retrieval-augmented generation. It also highlighted where a production health application would need much more work: clinical validation, doctor review, stronger evaluation, privacy controls, secure deployment, and careful safety testing.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
