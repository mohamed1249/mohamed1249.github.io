---
title: "ML: Lyric Embeddings for Music Recommendations"
date: 2025-09-30
status: "Completed"
tools:
  - Python
  - Sentence Transformers (all-mpnet-base-v2)
  - Pandas
  - Scikit-learn
  - WordCloud
  - Matplotlib
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-lyric-embeddings-for-music-recommendations"
excerpt: "A lyrics-only music recommender using all-mpnet-base-v2 sentence embeddings and cosine similarity — comparing whole-lyric vs. 4-segment embeddings across 7 test songs from a personal playlist."
---

## The Short Version

A content-based music recommender that uses only song lyrics — no audio features, no play counts, no genre tags. Two embedding strategies are compared on the same dataset: one that encodes each song as a single vector, and one that splits each song into four equal segments and concatenates them into a 4× richer super-embedding. Tested on 7 hand-picked songs from a personal playlist spanning heartbreak ballads, theatrical rock, pop anthems, and rap, then rated and analysed per song. The segmented approach wins 8/10 vs. 6.75/10.

## Problem

If movies can be recommended by their plot summaries, why can't songs be recommended by their lyrics? The working hypothesis: lyrics carry the emotional DNA of a song — theme, mood, narrative arc — and a model that understands lyrical meaning should find more coherent recommendations than one matching by audio features or genre alone. The secondary question: does splitting a song into segments preserve emotional shifts that get lost when the whole lyric is compressed into a single vector?

## Approach

**Dataset** — Two sources merged: `spotify_millsongdata.csv` (broad artist range, older catalog) and a folder of per-artist CSVs with more recent songs. After renaming columns to a common schema, both were concatenated into `df_big` — a single lyrics table indexed by `"artist - song"`.

**Segmentation** — A `split_text()` function divides each song's lyric into 4 equal word-count parts, producing `part1`–`part4` columns. This preserves the verse/chorus/bridge structure rather than averaging across it.

**Two embedding versions using `all-mpnet-base-v2`** (chosen for its strong semantic similarity performance over shorter sentence-transformer models):

- **Version 1 — Whole lyrics:** Each song's full text encoded into a single 768-dim vector. Embeddings concatenated into a song × features matrix and saved as `.pkl`.
- **Version 2 — Segmented lyrics:** Each of the 4 parts encoded separately into 768-dim vectors, then horizontally concatenated into a 3,072-dim super-embedding per song.

Both versions: duplicate index entries dropped (same song from both sources), cosine similarity matrix computed with `sklearn.metrics.pairwise.cosine_similarity`, wrapped in a `get_top_similar_songs()` function that excludes the query song and returns the top-N.

**Test set** — 7 personal playlist picks chosen to stress-test different lyrical styles:
- Adele — All I Ask, Someone Like You (heartbreak)
- Rihanna — Stay (minimal lyrics, high emotion)
- Taylor Swift — the 1 (indie-folk), Shake It Off (pop anthem)
- Queen — Bohemian Rhapsody (theatrical chaos)
- Nicki Minaj — LLC (dense rap bars)

## Result

**Version 1 (Whole lyrics) — 6.75/10**

| Song | Top result quality | Notes |
|---|---|---|
| Adele — All I Ask | ✅ Strong | Ed Sheeran, Ariana Grande — correct heartbreak cluster |
| Adele — Someone Like You | ⚠️ Mixed | Ed Sheeran cover correct, then Santana and Gloria Estefan |
| Rihanna — Stay | ⚠️ Mixed | Kygo edit and Dua Lipa good; Michael Bolton — no |
| Taylor Swift — the 1 | ✅ Strong | Folklore/Evermore aesthetic, John Denver acoustic bonus |
| Taylor Swift — Shake It Off | ⚠️ Lazy | Mariah Carey correct; Drake and Usher loose |
| Queen — Bohemian Rhapsody | 🎖 Creative | Weird Al's *Bohemian Polka* — perfect deep cut |
| Nicki Minaj — LLC | ✅ Strong | Chun-Li and Drake collabs correct |

**Version 2 (Segmented) — 8/10**

All seven songs improved or held steady. Notable gains:
- *All I Ask*: Sam Smith and Ellie Goulding replace weaker picks — more cohesive heartbreak cluster.
- *LLC*: Model locked onto Nicki's own catalog and Cardi B's *Get Up 10* — textbook flex-rap matching.
- *Bohemian Rhapsody*: Linkin Park and Sia replace Ozzy — more tonally consistent.
- *the 1*: Folklore/Evermore adjacency tightened further.
- *Stay*: Still the weakest — Rascal Flatts and Paul McCartney remain odd picks.

**Failure pattern:** Both versions struggle when the lyrical theme is generic ("love", "heartbreak") rather than stylistically distinctive. *Someone Like You* and *Stay* both fall into this trap — the model surfaces "any sad love song" rather than the specific emotional register of each track.

## What I Learned

The segmented embedding design is the most creative technical decision in this project — the insight that a song's narrative arc gets compressed out of existence when the whole lyric is averaged into one vector is correct, and the 4-chunk concatenation approach is a principled fix. The 4× dimension increase doesn't cause a curse-of-dimensionality problem here because cosine similarity is scale-invariant. `all-mpnet-base-v2` was the right model choice over lighter sentence transformers — its stronger contextual understanding is exactly what separates *"these songs both mention rain"* from *"these songs carry the same kind of quiet devastation."* The failure mode on generic heartbreak songs points to the fundamental limit of any unsupervised text similarity system: when thousands of songs describe the same feeling, cosine distance alone can't distinguish between them without some grounding in listener preference data — which is exactly the signal that collaborative filtering would add.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
