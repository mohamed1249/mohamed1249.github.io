---
title: "AI: Doctor Intelligent Graduation Project"
date: 2024-09-06
status: "Completed"
field: "AI"
tools:
  - Python
  - Flask
  - LangChain
  - OpenAI GPT Models
  - Pinecone
  - Sentence Transformers
  - RAG
  - PHP
  - JavaScript
  - HTML/CSS
github: "https://github.com/mohamed1249/Graduation-Project"
demo:
kaggle:
excerpt: "A graduation capstone project for an AI-powered mental-health knowledge assistant, combining a classic educational web section, bilingual chat endpoints, GPT-based responses, RAG retrieval with Pinecone, and a PHP frontend."
---

## The Short Version

Doctor Intelligent is my graduation project: an AI-powered mental-health knowledge assistant built as both a web application and a set of evolving AI prototypes.

The project combines a classic informational section for users who prefer reading with an intelligent chat section for users who want to ask questions. Behind the interface, the system experiments with GPT-based chat, bilingual Arabic/English prompts, retrieval-augmented generation, vector search, and a Flask API connected to a PHP frontend.

## Problem

Mental-health and special-needs information can be difficult to navigate. People may want accessible explanations about topics such as ADHD, autism, anxiety, schizophrenia, OCD, and Down syndrome, but they may not know where to start or what to search for.

The project explored whether an AI assistant could make educational information easier to access through two modes:

- a classic reading experience with organized topics,
- an interactive question-answering experience powered by LLMs and retrieval.

This was designed as an educational support system, not as a diagnostic or clinical replacement.

## Approach

**Web application** - The frontend includes login/signup pages, a home page, a classic reading section, and a chat interface. The classic section organizes mental-health and special-needs topics into readable pages with images and external references.

**Multiple AI versions** - The repository preserves several development versions:

- `V1` experiments with direct GPT-3.5/GPT-4 chatbot behavior.
- `V2` introduces RAG and vector database retrieval.
- `V3` includes preprocessing and fine-tuning experiments.
- `V4` continues interface and assistant iteration.

**Backend API** - A Flask backend exposes multiple endpoints for different assistant versions and languages. The API receives questions, chat history, and model choice, then returns a generated response.

**Bilingual prompts** - The backend includes English and Arabic routes, making the project more relevant to Arabic-speaking users while still supporting English interactions.

**RAG retrieval** - Later versions connect to Pinecone as a vector database. User questions are embedded with Sentence Transformers, relevant document chunks are retrieved, and the retrieved context is added to the LLM prompt.

**Knowledge sources** - The project includes mental-health and special-needs PDFs, including DSM-style reference material and Arabic documents, which were used for preprocessing, retrieval, and fine-tuning experiments.

## Result

The final project demonstrates a full-stack AI assistant workflow:

- static educational content for common mental-health and developmental topics,
- a chat UI with GPT-3.5/GPT-4 selection,
- Flask endpoints for model interaction,
- chat-history handling,
- retrieval-augmented prompts,
- Pinecone vector search,
- bilingual Arabic/English assistant behavior,
- multiple experimental versions documenting the project's development path.

As a portfolio piece, it shows both product thinking and AI engineering: the work was not only about calling an LLM, but about designing a user-facing system, organizing knowledge, experimenting with retrieval, and connecting a frontend to backend AI services.

## What I Learned

This project taught me how quickly an AI prototype becomes a systems problem. The model is only one layer; the experience also depends on prompt design, retrieval quality, chat history limits, frontend state, API routing, and how clearly the app explains its role to users.

It also made the responsibility of health-related AI very clear. A system like this must be framed carefully as educational support, with stronger safety boundaries, citations, privacy controls, and professional escalation guidance before it could be treated as anything more serious.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
