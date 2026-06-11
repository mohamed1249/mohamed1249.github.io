---
title: "ML: Alumni Data Analysis and Donor Clustering"
date: 2022-10-25
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - Scikit-learn
  - K-Means
  - StandardScaler
  - Silhouette Score
  - Plotly
  - Seaborn
  - Excel
github: "https://github.com/mohamed1249/Alumni-Data-Analysis-and-Clustering-Project"
demo:
kaggle:
excerpt: "A client-style alumni advancement analysis that cleans a large Excel dataset, answers faculty and regional targeting questions, clusters likely donors with K-Means, and exports ranked donor lists for outreach."
---

## The Short Version

This project analyzes a large alumni dataset to help an advancement team decide who to target for future fundraising. It combines data cleaning, exploratory analysis, donor scoring, K-Means clustering, and Excel deliverables that separate likely donors from less promising outreach targets.

The work answers practical questions: which faculties are strongest in Western Canada, whether alumni still appear sympathetic to giving, whether staff allocation by faculty makes sense, which cities should be prioritized, and which alumni look most likely to donate.

## Problem

The client needed a data-backed way to plan alumni outreach. The questions were not only descriptive; they were operational:

- Which graduation faculties are most common among alumni in Western Canada?
- Which alumni are still likely to be sympathetic to giving?
- Do staff allocations by faculty make sense compared with donation activity?
- Which cities should be targeted first?
- Which individual alumni are most likely to make a donation?

## Approach

**Data cleaning** - The original Excel dataset contains nearly one million rows and 30 columns. The notebook profiles null values, unique values, invalid columns, and messy fields before preparing the analysis dataset. It cleans fields such as gift years, largest gift, bequest gift score, student involvement, and affinity score.

**Exploratory analysis** - The analysis covers preferred language, gender distribution, past travel behavior, volunteering, faculty of graduation, province, city, year of graduation, lifetime giving, first and last gift years, events attended, affinity score, capacity score, bequest gift score, and recent click activity.

**Western Canada targeting** - The project filters Western Canada to British Columbia, Alberta, Saskatchewan, and Manitoba, then identifies the most common faculties for those alumni. General Arts appears as the strongest faculty signal in that regional subset.

**Donor clustering** - Because there was no labeled donor target column, the project uses unsupervised learning. K-Means clustering groups alumni into potential donor and non-donor segments. Elbow and silhouette checks support the two-cluster setup, matching the business need for a clear sympathetic vs. not-sympathetic split.

**Donor score and ranking** - A custom donor score combines bequest gift score, capacity score, and affinity score after normalization. The score is used to rank alumni and identify the top cities with the strongest average donor potential.

## Result

The project produced analysis visuals, clustering outputs, and Excel files for direct client use, including:

- Western Canada sympathetic and not-sympathetic alumni lists.
- Full-dataset sympathetic and not-sympathetic alumni lists.
- A ranked list of the alumni most likely to donate.
- A top-25 donor target file.
- City and faculty targeting charts.

The final clustering found about 7.6% of Western Canada alumni as potential donors, and about 26% of the full cleaned alumni dataset as potential donors.

## What I Learned

This project made the value of unsupervised learning very concrete. There was no clean label saying "this person will donate," but the data still contained signals: lifetime giving, largest gift, event attendance, affinity, capacity, bequest score, and recent engagement.

It also showed that analytics work often ends in a spreadsheet, not a model file. The real deliverable was not just a notebook; it was a ranked, interpretable list that a team could use for outreach decisions.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
