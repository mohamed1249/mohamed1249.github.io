---
title: "ML: Goodreads Book Search Engine"
date: 2026-05-30
status: "Completed"
tools:
  - Python
  - Pandas
  - Scikit-learn
  - TF-IDF
  - Cosine Similarity
  - Regex
  - Seaborn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-goodreads-search-engine"
excerpt: "A lightweight machine learning search engine for Goodreads books that cleans book titles, vectorizes them with TF-IDF, retrieves similar titles with cosine similarity, and ranks results by a custom popularity score."
---

## The Short Version

This project builds a simple but useful search engine for Goodreads book data. A user can type a book-related query, and the notebook returns the most similar titles from the dataset, sorted by a popularity score so the results are not only textually close but also more likely to be relevant to readers.

## Problem

Book datasets are large, messy, and not always easy to browse by exact title. A strict string match can miss useful results because users may type partial names, lowercase words, or slightly different title forms.

The goal was to build a small search system that can:

- Clean and normalize book titles.
- Compare a user query against thousands of titles.
- Return the closest matches.
- Rank those matches with a popularity-aware signal.

## Approach

**Data loading** - The project uses the Goodreads books dataset from Kaggle and loads it with Pandas.

**Popularity score** - Before search, the notebook creates a custom `popularity` column:

```python
data['popularity'] = (data.average_rating**8 * data.ratings_count) / 10**5
```

This gives more weight to books with strong average ratings and a meaningful number of ratings. The project also points readers to the earlier Goodreads EDA notebook for more background on this feature.

**Title cleaning** - Titles are prepared for search by:

- Removing special characters with regex.
- Lowercasing all text.
- Normalizing repeated whitespace.
- Dropping empty cleaned titles.

**Vectorization** - The cleaned titles are transformed with `TfidfVectorizer`, turning each title into a numerical representation based on term importance.

**Search function** - The user query is cleaned, vectorized, compared against all title vectors with `cosine_similarity`, and the top 10 closest matches are selected.

**Ranking** - After retrieving similar titles, the results are sorted by the custom `popularity` score. This makes the output feel more useful than pure text similarity alone.

## Result

The final notebook includes a reusable `search(query, vectorizer)` function that returns a ranked table of matching Goodreads books. For example, a query like `angel` produces a set of similar book titles, ordered by the popularity score.

The project is intentionally lightweight: it does not need a deep learning model, external API, or complicated infrastructure. It shows how far clean preprocessing, TF-IDF, cosine similarity, and thoughtful ranking can go for a practical information retrieval task.

## What I Learned

Search quality is not only about finding similar text. Ranking matters. A result can be technically similar to the query but still feel weak to a user if it is obscure, noisy, or untrusted. Adding a popularity signal gives the search output a more human sense of usefulness.

I also learned that small preprocessing decisions matter. Removing special characters, lowercasing titles, and normalizing whitespace make the vectorizer's job much cleaner and reduce avoidable mismatch between a user query and the dataset.

## Links

{% if page.kaggle %}- [Kaggle Notebook]({{ page.kaggle }}){% endif %}
