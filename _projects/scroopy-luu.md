---
title: "Tool: Scroopy-Luu Web Scraper"
date: 2025-03-05
status: "Completed"
tools:
  - Python
  - Streamlit
  - Requests
  - BeautifulSoup
  - Pandas
  - JSON
  - Pickle
  - CSV
github: "https://github.com/mohamed1249/Scroopy-Luu"
demo:
kaggle:
excerpt: "A Streamlit web-scraping tool that accepts multiple URLs, optionally follows sub-links, extracts page text, logs progress in the UI, and exports scraped content to JSON, Pickle, and CSV."
---

## The Short Version

Scroopy-Luu is a small web-scraping interface built with Streamlit. Instead of running scraping scripts manually, the user can paste one or more URLs into a web UI, choose whether to include sub-links, start the scraping job, watch logs in real time, and download the generated files.

## Problem

Scraping web pages often starts as a quick script, then becomes repetitive:

- paste a URL,
- fetch the page,
- extract readable text,
- clean duplicate lines,
- save the result,
- repeat the process for another URL,
- convert the output into a format that can be reused later.

The goal of Scroopy-Luu was to make that workflow more approachable through a simple interface and automatic multi-format exports.

## Approach

**Streamlit interface** - The app provides a wide-layout Streamlit UI where users can add or remove URL inputs, toggle sub-link scraping for each URL, start a scraping job, and clear logs.

**Scraping engine** - The scraper uses `requests` to fetch pages and `BeautifulSoup` to parse HTML. The main extraction function collects text from `div` elements, removes repeated lines, normalizes whitespace, and returns cleaned page content.

**Optional sub-link scraping** - For URLs marked with the sub-links option, the scraper collects links from the page and attempts to scrape each linked page as well. The output stores whether each item is the main page or a sub-page and keeps a link number for traceability.

**Export pipeline** - Scraped results are saved into structured folders:

- `JSONs` for readable structured output,
- `Pickles` for Python-native reuse,
- `CSVs` for tabular review and analysis.

**Download workflow** - After scraping, the Streamlit app groups generated files by format and creates download buttons for each result file.

## Result

The finished tool can process multiple URLs in one run, show timestamped progress logs, handle partial errors without losing successful outputs, and produce reusable data files for later analysis.

The local project already contains example outputs from documentation and data-heavy sites such as Google Cloud docs, n8n docs, Pinecone docs, MongoDB docs, HealthData.gov, MedlinePlus, and Wikipedia. That makes the project feel like a practical research/data-collection utility rather than just a scraper experiment.

## What I Learned

This project is a useful example of wrapping a script in a real interface. The scraping logic itself is only one part of the experience; the logs, multiple URL inputs, download buttons, format organization, and error reporting make the tool easier to use and easier to trust.

It also shows a common engineering tradeoff in web scraping: a simple general-purpose extractor can be useful quickly, but every website has different HTML structure. The next improvement would be stronger content selection, better relative-link handling, robots.txt awareness, and clearer controls for crawl depth.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
