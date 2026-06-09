---
title: "ReMo: NLP Movie Recommender System"
date: 2025-08-31
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - Scikit-learn
  - Sentence Transformers (all-MiniLM-L6-v2)
  - NumPy
  - Pickle
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/remo-nlp-recommender-movie-system"
excerpt: "A hybrid movie recommender system that iterates through four progressively better approaches — metadata cosine similarity, collaborative filtering, NLP embeddings, and a final system combining all three — documented with honest per-attempt ratings and a distinct voice."
---

## The Short Version

ReMo (REcommender-MOvie) is the biggest unsupervised ML project in this portfolio — a hybrid recommender system built across four approaches on The Movies Dataset. Each approach is implemented, tested on real movies (Spider-Man, Harry Potter, Toy Story, Guardians of the Galaxy), rated honestly when it fails, and iterated on. The final `recommender_prime` function combines NLP semantic embeddings, metadata cosine similarity, and collaborative filtering co-watch counts into a single composite-scored recommendation engine that actually works.

## Problem

Movie recommendation is a solved problem at Netflix scale — but building one from scratch is an entirely different exercise. The challenge was: can three fundamentally different signals (what a movie *is like* by metadata, what it's *about* by text, and who *watches it together* by behavior) be combined into a recommender that's meaningfully better than any signal alone?

## Approach

The project is structured as four progressive attempts, each building on the failures of the last.

**Attempt 1 — Metadata Cosine Similarity (5.25/10)**

Two CSVs (movie metadata and credits) were merged, with nested JSON columns unpacked: `production_companies`, `production_countries`, and `genres` were parsed from string-embedded lists; `cast` and `crew` were parsed with `eval()` to extract `actor_1`, `actor_2`, and `director`. `belongs_to_collection`, `release_date`, `budget`, and `vote_count` were cleaned and typed. Languages with fewer than 50 movies were filtered. All categorical columns were `LabelEncoder`-encoded; all numeric columns were `StandardScaler`-scaled. A full movie×movie cosine similarity matrix was computed on the resulting feature space.

Result: The Amazing Spider-Man test returned The Dark Knight Rises and Man of Steel — superhero adjacency, but no Tobey Maguire. Rated 5.25/10 — exceeded expectations but not actually useful.

**Attempt 2 — Collaborative Filtering via Co-watch Counts (0.5/10)**

The `ratings.csv` file (user IDs + movie ratings) was used to build a co-watch frequency dictionary: for each user with ≥2 ratings, every pair of movies in their watched list was counted. Movies were sorted by co-watch count and the top 10 stored per movie. Harry Potter (id 671) returned A Nightmare on Elm Street, Rain Man, Rope, and Men in Black II. The dataset's age (last updated ~2015) and sparse coverage for specific titles made this approach nearly useless. Rated 0.5/10 and documented with full sarcasm intact.

**Attempt 3 — Sentence Embedding Similarity (8/10 → 8.75/10 → 11/10)**

The `all-MiniLM-L6-v2` sentence transformer from Hugging Face was used to encode movie texts. Three sub-approaches were tested:

- **Overviews only:** Embeddings of movie plot summaries. Spider-Man test returned other Spider-Man films + Guardians. Some noise (Hook, Babysitters Beware). Rated 8/10.
- **Keywords only:** Embeddings of keyword strings (parsed from nested JSON with `eval()`). Spider-Man test returned X-Men, Logan, Hulk, The Punisher — sharper superhero clustering. Rated 8.75/10.
- **Overviews + Keywords combined:** Overview and keyword embeddings generated separately then `np.concatenate`d into a single 768-dim vector. Spider-Man test returned Spider-Man 1, 2, 3, Amazing Spider-Man 2, Logan, Superman, Guardians. Rated 11/10 — the first approach that felt genuinely correct.

**Attempt 4 — `recommender_prime`: Hybrid System (final)**

The NLP combined embedding serves as the base score (10 down to 1 for top-10 results). Metadata cosine similarity and co-watch collaborative filtering results are each checked: if a movie appears in those results *and* in the NLP top-10, its score gets +2 from each signal (maximum +4 boost). Final ranking by composite score.

Tested on:
- **Toy Story (862):** Toy Story 2, Toy Story 3, Big Hero 6, The Lego Movie as top picks.
- **Harry Potter and the Philosopher's Stone (671):** Top 6 results were the full Harry Potter series, followed by The Wizard of Oz, Narnia, and Cinderella.

## Result

Each successive approach produced measurably better recommendations, tracked by qualitative ratings at each step:

| Approach | Test Movie | Rating | Notes |
|---|---|---|---|
| Metadata cosine similarity | Spider-Man | 5.25/10 | Superhero adjacency, wrong franchise |
| Collaborative filtering | Harry Potter | 0.5/10 | Dataset too sparse and old |
| Overview embeddings | Spider-Man | 8/10 | Mostly correct, some noise |
| Keywords embeddings | Spider-Man | 8.75/10 | Sharper genre clustering |
| Overview + Keywords | Spider-Man | 11/10 | First genuinely correct results |
| **`recommender_prime`** | Toy Story, HP | **Final** | Sequels first, then thematically similar |

The hybrid system reliably surfaces sequels and direct franchise entries first, then broadens to thematically similar films — the behavior you'd want from any real recommender.

## What I Learned

The clearest lesson is that `all-MiniLM-L6-v2` over movie keywords is the single highest-signal input in this system — it captures genre and thematic similarity more precisely than raw metadata features (which collapse categorical nuance into integers) or overviews (which are more narrative than categorical). Concatenating two embedding spaces before computing cosine similarity is a deceptively simple operation that works better than either alone because it forces the similarity metric to jointly account for both narrative context and topical identity. The hybrid scoring in `recommender_prime` is deliberately lightweight — +2 boosts rather than a learned weight — because with no training signal, a simple additive heuristic is more honest than an overfit combination function. The co-watch approach's failure is also instructive: collaborative filtering requires dense, recent interaction data to work; a sparse 2015 dataset with millions of missing user-movie pairs produces random-walk recommendations. The signal isn't in the algorithm, it's in the data quality.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
