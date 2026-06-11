---
title: "AI: PDF Chatbot with Streamlit and OpenAI"
date: 2024-07-14
status: "Prototype"
field: "AI"
tools:
  - Python
  - Streamlit
  - OpenAI
  - GPT-3.5
  - LlamaIndex
  - LangChain
  - PyPDF2
  - JSON
github: "https://github.com/mohamed1249/PDF-Chatbot-with-Streamlit-and-OpenAI"
demo:
kaggle:
excerpt: "An early RAG-style PDF chatbot that extracts pages from a document, builds a vector index, and lets users ask questions through a Streamlit chat interface powered by OpenAI."
---

## The Short Version

This project is an early PDF chatbot prototype built with Streamlit and OpenAI. It takes a PDF document, extracts its pages into structured JSON, builds a searchable vector index, and provides a chat interface where users can ask questions about the document.

It sits nicely before my later open-source RAG work: this was one of the projects where I started connecting document parsing, vector search, prompt context, and conversational interfaces into one working AI app.

## Problem

Long PDFs are difficult to explore manually. A user may not want to search for exact keywords; they may want to ask a direct question and receive an answer grounded in the document.

The goal was to build a simple flow that could:

- extract text from a PDF,
- keep page-level structure,
- turn the extracted text into searchable data,
- build an index for retrieval,
- let a user ask questions through a chat interface.

## Approach

**PDF extraction** - The project uses `PyPDF2` to read the PDF page by page and save extracted text into JSON records with page numbers and the source book name.

**Index generation** - The extracted document data is loaded with the older `gpt_index` / LlamaIndex-style workflow and saved as a local index file.

**OpenAI answering** - The index is connected to an OpenAI chat model through LangChain, using a controlled temperature and token settings for answer generation.

**Streamlit chat UI** - The app uses Streamlit chat components to show user and assistant messages, preserve recent chat history in session state, and provide a simple browser-based interface.

**Algorithm-focused demo** - The local project uses an algorithms textbook as the source document, making the chatbot feel like a small study assistant for technical material.

## Result

The result is a working prototype for asking natural-language questions over a PDF-backed knowledge source. It demonstrates the core pieces of a document chatbot:

- document extraction,
- JSON preparation,
- vector-index construction,
- chat state,
- prompt composition,
- OpenAI-backed responses.

As a portfolio project, its value is not that it is the most modern RAG implementation, but that it shows the foundations: turning a static document into an interactive AI conversation.

## What I Learned

This project helped me understand that a chatbot is only as useful as the retrieval and document-processing layer beneath it. Extracted text quality, chunk size, prompt context, and session history all shape the final answer.

It also exposed an important engineering lesson: API keys and secrets should never live directly in source files. A cleaner production version would use environment variables or a secret manager, add file upload support, improve chunking and citations, and update the older `gpt_index` workflow to modern LlamaIndex or LangChain patterns.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
