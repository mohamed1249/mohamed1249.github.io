---
title: "EDA: Students Performance in Exams"
excerpt: "An exploratory analysis of 1,000 student exam records, showing how demographics, lunch type, parental education, and test preparation relate to math, reading, and writing scores."
date: 2022-04-01
status: "Completed"
field: "EDA"
tools:
  - Python
  - Pandas
  - Plotly
  - Seaborn
  - Matplotlib
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/eda-students-performance-in-exams"
---

## The Short Version

An exploratory data analysis of 1,000 students' exam scores across math, reading, and writing. The goal was to understand how demographic and socioeconomic factors — gender, race/ethnicity, parental education, lunch type, and test prep — influence academic performance. It surfaces clear, data-backed patterns about what separates high-scoring students from low-scoring ones.

## Problem

Which student background factors most strongly predict exam scores? Do gender gaps exist across subjects? Does completing a test preparation course actually make a measurable difference? This project started from curiosity about whether simple demographic variables could explain meaningful variation in student outcomes.

## Approach

The dataset (sourced from Kaggle) contains 8 columns and no missing values — clean enough to jump straight into analysis. The approach used:

- **Pie charts and sunburst charts** (Plotly) to visualize the composition and intersections of categorical variables like race, gender, lunch type, and parental education.
- **Histograms and distribution plots** (Plotly) to compare score distributions by group, reporting both mean and mode per segment.
- **Grouped bar charts** to compare average scores across demographic categories.
- **A correlation heatmap** (Seaborn) to measure how math, reading, writing, and total scores relate to one another.
- A derived `total` score column (sum of all three subjects) was engineered for aggregate comparisons.

## Result

Several clear patterns emerged:

- **Gender:** Males outperform females in math; females outperform males in both reading and writing, and earn higher total scores overall.
- **Race/Ethnicity:** Group E achieves the highest average total score and is the only group where math scores exceed reading/writing averages.
- **Parental Education:** Students whose parents hold a master's degree score higher across all subjects.
- **Lunch Type:** Students on a standard lunch plan consistently outscore those on free/reduced lunch — a proxy for socioeconomic status.
- **Test Preparation:** Completing the test prep course is associated with higher scores across all three subjects.
- **Score Correlations:** Reading and writing scores are strongly correlated with each other and together drive total score more than math does.

## What I Learned

Socioeconomic signals (lunch type, parental education) appear at least as influential on outcomes as direct preparation factors (test prep course). Visualizing multi-variable interactions through sunburst charts made it easier to spot compound effects — e.g., how gender splits differ across race groups at each parental education level — that flat bar charts would have missed.

## Links

[Kaggle](https://www.kaggle.com/code/ma12492002/eda-students-performance-in-exams)
