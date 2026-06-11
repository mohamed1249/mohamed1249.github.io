---
title: "Tool: Flask Web Scraper API"
date: 2024-06-17
status: "Completed"
field: "Tool"
tools:
  - Python
  - Flask
  - BeautifulSoup
  - Requests
  - PyPDF2
  - MongoDB
  - Google Drive API
  - Langflow
  - JSON
  - CSV
github: "https://github.com/mohamed1249/web-scraper-flask"
demo:
kaggle:
excerpt: "A Flask-based scraping and ingestion API that collects web pages, PDFs, and Google Drive documents, cleans them into structured JSON, and stores the results for downstream search or AI workflows."
---

## The Short Version

This project is a backend scraping and ingestion service built around Flask. It was designed to collect public web content, optionally download attached PDFs, extract PDF text, convert everything into structured JSON, and save the result into MongoDB for later use.

Unlike Scroopy-Luu, which is a Streamlit interface for interactive scraping, this project behaves more like an API layer: a system another application can call when it needs content collected, cleaned, and stored.

## Problem

For search, chatbot, and knowledge-base projects, the hard part often starts before the model: the data has to be collected, cleaned, structured, and traced back to sources.

The client-side need here was practical:

- scrape selected website pages,
- follow relevant sub-links when needed,
- download and validate PDFs,
- extract readable text from PDF pages,
- collect content from Google Drive folders,
- save outputs in a format that can feed later retrieval or AI workflows.

## Approach

**Flask API** - The project exposes API routes for scraping web URLs and processing Google Drive folders. Requests include options for downloading PDFs, scraping page content, and selecting the destination database.

**HTML scraping** - The scraper uses `requests` and `BeautifulSoup` to fetch pages, extract text content, remove duplicate lines, normalize whitespace, and preserve the source URL.

**PDF extraction** - Linked PDF files are downloaded, validated, and converted into page-level JSON records using `PyPDF2`. This makes long documents easier to search and reuse later.

**MongoDB storage** - Scraped pages and extracted PDF pages are saved into MongoDB collections, with update logic for files that already exist.

**Google Drive ingestion** - A second API route connects to Google Drive, walks through folders recursively, downloads PDF files, extracts their text, and stores the resulting JSON records.

**Langflow component** - The project also includes a custom Langflow web-scraper component, making the scraper easier to plug into visual AI/data workflows.

## Result

The local project contains a full scraping run with more than 1,600 JSON outputs across sources such as GGD West Brabant, Groeigids, and Thuisarts. It also includes CSV link lists for baby, toddler, health-question, and medical-information pages.

As a portfolio project, this shows the less glamorous but important part of AI work: building the ingestion layer that turns messy public content into structured data that can later support search, RAG, or analysis.

## What I Learned

This project made the data-engineering side of AI feel very concrete. A useful AI system is not only prompts and models; it also needs reliable source collection, text extraction, storage, and traceability.

It also showed the tradeoffs of general-purpose scraping. A simple scraper can move fast across many pages, but production scraping would need stronger content selection, rate limiting, robots.txt awareness, secure secret handling, better crawl-depth controls, and richer error reporting.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
