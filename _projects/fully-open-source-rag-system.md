---
title: "AI: Fully Open-Source RAG System"
date: 2024-11-10
status: "Completed"
tools:
  - Python
  - LangChain
  - Ollama
  - Llama 3
  - Sentence Transformers
  - FAISS
  - Streamlit
  - PyPDFLoader
github: "https://github.com/mohamed1249/Fully-Open-Source-RAG-System"
demo:
kaggle:
excerpt: "A fully open-source Retrieval-Augmented Generation system that lets users upload a PDF, embeds its content with Sentence Transformers, retrieves relevant chunks with FAISS, and answers questions through a local Ollama/Llama 3 workflow."
---

## The Short Version

This project is a local Retrieval-Augmented Generation system for asking questions about PDF documents. A user uploads a PDF, the app splits it into chunks, embeds the text, retrieves relevant context, and sends that context to a local LLM for question answering.

The important idea is that the whole pipeline is open-source: document loading, embeddings, vector search, app interface, and language model access can all run without depending on a closed hosted model API.

## Problem

PDFs often contain useful information, but searching through them manually is slow. A normal keyword search can find matching words, but it does not always answer a user’s actual question.

The goal was to build a simple RAG workflow that can:

- accept a PDF upload,
- break the document into searchable chunks,
- represent those chunks as embeddings,
- retrieve the most relevant context for a question,
- generate an answer grounded in the uploaded document.

## Approach

**PDF loading** - The Streamlit app accepts a PDF file and loads it with LangChain’s `PyPDFLoader`.

**Chunking** - The document is split into smaller text chunks with `CharacterTextSplitter`, using chunk overlap to preserve context across boundaries.

**Embeddings** - Text chunks are embedded with the `all-MiniLM-L6-v2` Sentence Transformers model.

**Vector search** - The embeddings are stored in a FAISS index for fast similarity search. The app also uses a small `VectorMap` helper to retrieve the most relevant chunks for a user question.

**Local LLM answering** - The retrieved context is passed into a LangChain retrieval QA prompt and answered with `ChatOllama`, using Llama 3 locally.

**Interface** - Streamlit provides the upload field, question input, and answer output, turning the notebook workflow into a small usable app.

## Result

The finished prototype lets a user upload a PDF and ask document-specific questions through a browser interface. The system retrieves relevant sections from the document, sends them as context to the LLM, and returns an answer based on the uploaded file.

The project demonstrates the main moving parts of modern RAG systems: loaders, chunking, embeddings, vector search, prompt composition, and local model inference.

## What I Learned

This project helped connect the abstract idea of RAG to a real working pipeline. The quality of the answer depends on every stage before the LLM: how the PDF is parsed, how the chunks are created, how embeddings represent the text, and whether retrieval finds the right context.

It also made the open-source tradeoff clear. Running locally gives more control and privacy, but it also means handling model setup, dependencies, performance, and retrieval quality yourself.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
