---
title: "AI: Azure OpenAI Document Processing and RAG System"
date: 2024-06-19
status: "Prototype"
field: "AI"
tools:
  - Python
  - Azure OpenAI
  - OpenAI API
  - GPT-4
  - Pinecone
  - Sentence Transformers
  - Gradio
  - PyPDF2
  - JSON
  - RAG
github: "https://github.com/mohamed1249/Azure-OpenAI"
demo:
kaggle:
excerpt: "An Azure OpenAI prototype for document summarization, tagging, classification, and RAG-style question answering over service workflow and vehicle-license data."
---

## The Short Version

This project explores Azure OpenAI as a practical engine for document understanding and retrieval-augmented generation. It combines PDF extraction, prompt-based document processing, vector search, and a small Gradio interface for asking questions over structured service data.

The work sits between document AI and applied RAG: taking messy source material, preparing it for an LLM, retrieving the right context, and using Azure OpenAI to generate useful responses.

## Problem

Many business documents and service workflows are not easy to use directly. The information may live inside PDFs, JSON exports, JavaScript chunks, or internal process documents. A normal search can find words, but it does not always explain the workflow or answer a user's functional question.

The project focused on a few practical tasks:

- summarize long PDF documents,
- generate document tags,
- classify documents with a short label,
- retrieve relevant service-context chunks,
- answer questions over a vehicle-license renewal workflow,
- generate RCC-style functional scripts from source files.

## Approach

**PDF processing** - The document-processing scripts use `PyPDF2` to extract page-level text from a PDF, with a token-style limit so only a manageable portion of the document is sent to the model.

**Azure OpenAI prompts** - Separate scripts handle summarization, tagging, and classification using Azure OpenAI chat completions. Each task uses a focused system prompt so the same document extraction function can support different outputs.

**RAG pipeline** - The RAG prototype embeds service and workflow chunks with `all-MiniLM-L6-v2`, stores them in Pinecone, retrieves the most relevant metadata for a user query, and builds an augmented prompt from the retrieved context plus recent chat history.

**Gradio interface** - The UI exposes a simple chatbot-style interface for asking questions over the prepared knowledge base. The response is streamed back gradually so it feels more interactive.

**RCC assistant** - A second interface reads source files and asks Azure OpenAI to generate RCC-style scripts, turning code artifacts into a more guided functional-assistant workflow.

## Result

The project produced a working set of AI prototypes:

- document summarization over a telecommunications PDF,
- document tagging and classification examples,
- a Pinecone-backed RAG bot for a vehicle-license renewal service,
- a Gradio chat interface for functional Q&A,
- an RCC assistant for generating scripts from source files.

As a portfolio project, this shows the applied engineering side of LLM work: document ingestion, prompt design, embeddings, vector search, chat history trimming, UI wrapping, and integration with Azure-hosted OpenAI deployments.

## What I Learned

This project made the difference between "calling an LLM" and "building an LLM system" much clearer. The useful part is not only the model response; it is the pipeline around it: extraction, chunking, retrieval, context control, task-specific prompting, and interface design.

It also highlighted a serious production lesson: API keys and service credentials should never be hard-coded in scripts or committed to a repository. A cleaner version would use environment variables, secret management, pinned dependencies, safer error handling, source citations, and tests for retrieval quality.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
