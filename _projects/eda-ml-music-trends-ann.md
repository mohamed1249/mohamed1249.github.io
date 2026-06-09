---
title: "EDA + ML: Music Trends Analysis & ANN Trendiness Prediction"
date: 2024-11-17
status: "Completed"
field: "ML"
tools:
  - Python
  - PyTorch
  - Pandas
  - Scikit-learn
  - Plotly
  - Seaborn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/eda-ml-music-trends-ann-predictive-modeling"
excerpt: "A Spotify 2024 streaming dataset EDA with a hand-engineered TikTok trendiness score, followed by a PyTorch ANN classifier with early stopping and gradient clipping that predicts a song's trendiness category from cross-platform streaming metrics."
---

## The Short Version

An end-to-end pipeline on the Most Streamed Spotify Songs 2024 dataset — cleaning messy comma-formatted numeric columns, engineering a TikTok-based trendiness score from three normalized engagement metrics, then training a PyTorch feedforward ANN to classify songs into four trendiness categories. The model architecture uses the same layer-spec-driven `ANN` class pattern from the ASL CNN project, extended here with Dropout regularization, Adam with weight decay, early stopping, and gradient clipping.

## Problem

Can a song's cross-platform streaming footprint — Spotify, YouTube, Apple Music, Deezer, Pandora, SoundCloud, Shazam, TikTok — predict how "trendy" it is on TikTok specifically? And since TikTok trendiness isn't a column in any dataset, can it be engineered from raw engagement data into a classification target?

## Approach

**Cleaning** — The dataset arrived with all numeric metrics stored as comma-formatted strings (e.g., `"1,234,567"`). Twelve columns required `.str.replace(',', '').astype(float)` conversion. Six low-signal columns were dropped (`ISRC`, `TIDAL Popularity`, `AirPlay Spins`, `SiriusXM Spins`, `YouTube Playlist Reach`, `Pandora Track Stations`). Release date was parsed to datetime, then decomposed into `month`, `year`, `year_day`, and a `days since release` counter.

**EDA** — Three time-series line charts tracked monthly release volume, average Track Score, and explicit track count over time. TikTok columns (`Posts`, `Likes`, `Views`) were each explored with box plots before being used to build the target variable.

**Trendiness score engineering** — Each TikTok metric was min-max normalized to [0, 100], then summed into a single `trending_score`. This composite was bucketed into four ordinal classes:

| Class | Label | Threshold |
|---|---|---|
| 0 | Very Low | score ≤ 0.18 |
| 1 | Not really | score ≤ 0.83 |
| 2 | Good | score ≤ 3 |
| 3 | Trendy | score > 3 |

Distribution visualized as a pie chart; top 20 trendy tracks and artists plotted as bar charts. The TikTok columns were then dropped from the feature set before modeling to prevent target leakage.

**Preprocessing for modeling** — Remaining null values filled with -1; `Artist` label-encoded; feature matrix converted to float32 PyTorch tensors; `StandardScaler` fit on training split and applied to both splits (correctly — no leakage).

**ANN architecture** — Built with the same reusable layer-spec `ANN` class, this time specifying:
`Linear(17→64) → ReLU → Dropout(0.4) → Linear(64→64) → ReLU → Dropout(0.4) → ReLU → Linear(64→32) → ReLU → Dropout(0.2) → Linear(32→32) → ReLU → Dropout(0.2) → Linear(32→16) → ReLU → Linear(16→4)`

**Training** — `CrossEntropyLoss`, Adam (lr=0.0005, weight_decay=1e-4). A full `train_model()` function with:
- Per-epoch tracking of train/test loss and accuracy.
- **Early stopping** with `patience=50` — halts training when test loss fails to improve for 50 consecutive epochs.
- **Gradient clipping** (`max_norm=1.0`) to prevent exploding gradients.
- Progress printed every 100 epochs.

**Evaluation** — Loss and accuracy curves plotted side-by-side (train vs. test). Confusion matrix generated with `sns.heatmap` via a dedicated `ANN_confusion_matrix()` function.

## Result

The model converged under early stopping before the 1,000-epoch ceiling. Loss and accuracy curves showed healthy convergence without significant overfitting — the aggressive Dropout (0.4 in early layers) and L2 weight decay kept the train/test gap narrow. The confusion matrix confirmed strongest performance on the majority class (`Very Low` and `Not really`) with reasonable separation on the smaller `Trendy` class — expected given the class imbalance inherent in a long-tail trendiness distribution.

Top 20 trendy tracks were dominated by viral TikTok-era songs; top artist by total trending score reflected the playlist-spanning artists with consistent multi-video presence rather than single-hit performers.

## What I Learned

Engineering the trendiness target from raw TikTok metrics — rather than using a pre-existing label — is the most domain-specific decision in this project. The min-max normalization before summing is important: raw TikTok Views dwarf Posts by orders of magnitude, so summing without normalization would effectively reduce the score to Views alone. The threshold choices for the four buckets are heuristic rather than data-driven — a quantile-based split would be more principled, but the chosen thresholds produce interpretable class sizes. The `train_model()` function's early stopping + gradient clipping combination is the most production-ready training loop in the portfolio — patience-based stopping prevents wasted compute and overfitting, while gradient clipping handles the instability that can arise with deeper networks on imbalanced multi-class targets. The L2 weight decay in Adam (`weight_decay=1e-4`) adds a second regularization signal on top of Dropout — belt-and-suspenders regularization for a relatively small dataset.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
