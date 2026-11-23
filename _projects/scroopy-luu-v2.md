---
title: "Tool: Scroopy Luu v2 Web and PDF Collection"
date: 2026-11-23
status: "Early local-use release"
field: "Tool"
tools:
  - Python
  - Flask
  - Streamlit
  - MongoDB
  - HTTP
  - Automated testing
github: "https://github.com/mohamed1249/scroopy-luu-v2"
excerpt: "A local Flask and Streamlit collection system with bounded crawling, webpage and PDF extraction, traceable MongoDB persistence, safe refresh behavior, image collection, and recovery reports."
---

## The Short Version

Scroopy Luu v2 develops my earlier Streamlit scraper into a local Flask API and Streamlit application for collecting webpage and PDF text for AI data workflows. The emphasis is on source traceability, persistent pulls, controlled downloads, and retaining useful saved content when refresh attempts fail.

## Approach

- Breadth-first crawling supports selectable levels, page limits, and deduplication of repeated links and URL fragments. Extraction offers full-page text or optional main content, with linked PDF text kept in page order.
- A stable pull ID identifies URLs and settings; individual run IDs identify collection attempts. MongoDB stores pulls, runs, unique nonempty texts, source metadata, content hashes, and ordered text ranges.
- Successful refreshes merge updated documents. Failed, empty, or unvisited pages retain earlier content and the last successful run identity. Refreshes are not atomic snapshots; run IDs matter when checking freshness. Locks prevent concurrent refreshes of the same pull.
- Optional image references retain URLs, alt text, captions, and page order. Downloads use hash-named files to deduplicate identical image bytes, while MongoDB keeps metadata and relative paths.
- Per-file and per-run byte limits, public-destination checks, DNS-pinned HTTP transport, owned run workspaces, and compressed recovery files address collection and persistence failures.
- Streamlit supplies previews and a downloadable JSON run report. A launcher checks MongoDB and starts the local services.

## Engineering Evidence

The repository documents offline regression and Streamlit tests that mock websites and MongoDB, a local transport verification script, and optional database integration checks. These are documented test facilities; this portfolio entry does not assert a test count, measured throughput, or a test run performed during this update.

## Result and Limits

The result is an early local-use release with a documented storage contract, API settings, recovery operations, and dataset roadmap. It demonstrates progression from one-off text scraping toward a persistent collection workflow with explicit identity, provenance, failure handling, and resource controls.

The application does not render JavaScript, perform OCR, preserve raw HTML, or reconstruct visual layouts. Dataset export, saved-pull browsing, chunking, embeddings, and training remain future work. The API has no authentication and is intended for localhost. Crawler pacing, robots.txt handling, global concurrency quotas, and automatic stale-lock recovery are not implemented. No production adoption, business outcome, or ready-made training dataset is claimed.

## Links

- [Repository and documentation](https://github.com/mohamed1249/scroopy-luu-v2)
- [Earlier Scroopy Luu project](/projects/scroopy-luu/)
