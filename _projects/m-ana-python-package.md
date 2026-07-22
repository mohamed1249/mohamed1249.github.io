---
title: "Library: M-Ana Python Toolkit"
date: 2026-07-04
status: "Stable / v1.0.0"
field: "Tool"
tools:
  - Python
  - Pandas
  - NumPy
  - Scikit-learn
  - SciPy
  - FAISS
  - Pinecone
  - PostgreSQL
  - SQLAlchemy
  - PyTorch
  - TensorFlow
  - Statsmodels
  - MkDocs
  - GitHub Actions
github: "https://github.com/mohamed1249/M-Ana"
demo:
kaggle:
excerpt: "A stable, tested Python toolkit that turns recurring data, AI, modeling, experimentation, forecasting, recommendation, retrieval, and database workflows into reusable package APIs."
---

## The Short Version

M-Ana is the Python toolkit I wish I had when I started moving between analysis notebooks, machine-learning experiments, AI prototypes, and production data work. It packages the pieces I kept rebuilding: loading and cleaning data, visualizing patterns, evaluating models, forecasting time series, testing hypotheses, retrieving evidence, ranking recommendations, and moving data through databases.

What began as a personal collection of helpers is now a stable `1.x` library with nine public domains, a small core installation, optional feature groups, semantic versioning, automated tests, documentation, and a repeatable release workflow.

## Problem

Technical projects often spend too much time recreating the same foundation:

- normalizing messy inputs before the real analysis can begin,
- rebuilding plots and evaluation reports for every model,
- creating time-series features without leaking future information,
- wiring document chunking, retrieval, citations, and generation together,
- implementing recommendation baselines before testing better ideas,
- moving DataFrames into databases safely and efficiently,
- keeping heavy libraries installed even when a project does not use them.

M-Ana turns those repeated decisions into reusable, documented interfaces so each new project can start closer to its actual problem.

## The Toolkit

| Domain | What M-Ana provides |
| --- | --- |
| `data` | Multi-format loading, validation, outlier handling, cleaning reports, and a chainable `DataCleaner` workflow. |
| `analysis` | Exploratory plots, statistical visualizations, animated charts, and compact dashboard helpers. |
| `modeling` | Classification, regression, and clustering evaluation; model persistence and registry tools; automated experiments; and PyTorch training utilities. |
| `nlp` | Text normalization, tokenization, chunking, vectorization, sentiment analysis, topic modeling, embeddings, semantic search, and leakage-safe text classifiers. |
| `rag` | Citation-ready ingestion, stable document IDs, FAISS search, rank fusion, grounded prompts, provider-neutral generation, Pinecone integration, and retrieval metrics. |
| `recommend` | Popularity, content-based, item-KNN, and hybrid recommenders with temporal holdouts and ranking evaluation. |
| `timeseries` | Forecasting, anomaly detection, decomposition, panel features, split-before-scale workflows, seasonality diagnostics, and forecast evaluation. |
| `stata` | Hypothesis tests, effect sizes, confidence intervals, Bayesian A/B testing, power analysis, multiple-testing corrections, and experiment visualizations. |
| `database` | SQL helpers and connectors for PostgreSQL, MySQL, SQLite, and MongoDB, with PostgreSQL-first bulk transfer, upsert, schema, index, and query-plan tooling. |

## Engineering Decisions

**A small core with optional capabilities** - The default install keeps only the broadly useful scientific Python stack. Heavier systems such as TensorFlow, PyTorch, Prophet, Pinecone, sentence transformers, and database drivers live in optional dependency groups, so a project installs only what it needs.

**Leakage-aware APIs** - Time-series splitting and scaling happen chronologically, grouped lag features stay within each entity, NLP pipelines learn vocabularies and encodings from training data, and recommender evaluation uses per-user temporal holdouts. These boundaries are part of the API rather than reminders left to each notebook.

**Provider-neutral retrieval** - The RAG layer separates ingestion, indexing, retrieval, ranking, prompt construction, and generation. It can use local semantic indexes, FAISS, Pinecone, or OpenAI-compatible providers without coupling the whole pipeline to one vendor.

**Evidence and evaluation** - Retrieved chunks retain stable IDs and source metadata, grounded prompts expose citations, and retrieval can be measured with precision, recall, MRR, average precision, and NDCG. Recommendation models follow the same ranked-output contract and can be compared with ranking, coverage, and diversity metrics.

**Database workflows beyond basic reads and writes** - The current database layer treats PostgreSQL as a first-class analytical system: environment-based connection URLs, schema-aware batch upserts, fast `COPY` transfers, DataFrame-to-DDL generation, catalog inspection, safe index creation, query plans, and maintenance diagnostics. SQLite remains available for portable notebooks and local stores.

## Package Quality

M-Ana is structured as an installable Python package rather than a source folder copied between projects:

- `pyproject.toml` is the single source for package metadata and dependency groups,
- the public version is defined once and released under semantic versioning,
- MkDocs organizes quick starts, API references, tutorials, and module notebooks,
- GitHub Actions checks supported Python versions and builds release artifacts,
- the release workflow runs linting, tests, package builds, and distribution checks,
- 95 automated tests currently pass across data, visualization, modeling, NLP, RAG, recommendation, statistics, time series, and database workflows.

The package supports Python 3.9 through 3.13 and ships under the MIT License.

## Result

M-Ana now provides a shared technical foundation for projects that would otherwise look unrelated. A data-cleaning notebook, a cited document assistant, a hybrid recommender, a forecasting experiment, and a PostgreSQL feature pipeline can reuse the same package conventions and evaluation discipline.

That is the main result of the project: not one model or dashboard, but a growing system for making future work faster, more consistent, and easier to trust.

## What I Learned

Building a library changes the standard of completion. A function that works once in a notebook is only the beginning; a public API also needs names that make sense, safe defaults, stable return types, dependency boundaries, documentation, tests, and a migration story when behavior changes.

The project also taught me to reduce scope deliberately. M-Ana became stronger when modules were organized around coherent workflows, duplicate implementations were removed, and optional integrations were kept outside the core package. Reusability is not about collecting the most functions. It is about making the right paths clear and dependable.

## Links

- [Source code and documentation]({{ page.github }})
- [v1.0.0 release](https://github.com/mohamed1249/M-Ana/releases/tag/v1.0.0)
