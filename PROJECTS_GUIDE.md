# Adding a Project

Each project is one Markdown file inside `_projects`.

## Fast Steps

1. Copy `templates/project.md`.
2. Paste it into `_projects`.
3. Rename it like this:

```text
2026-05-27-my-project-name.md
```

4. Edit the title, date, field, tools, links, excerpt, and sections.
5. Commit and push. The project will appear automatically on `/ai-data/`.

## Project Fields

Use one of these values so the project appears in the right tab:

```yaml
field: "AI"
field: "ML"
field: "EDA"
field: "Stats"
field: "Tool"
```

- `AI`: LLMs, RAG, agents, generative systems, NLP, computer vision, and projects that feel like intelligent systems.
- `ML`: prediction models, recommenders, clustering, forecasting, model comparison, and classic machine learning notebooks.
- `EDA`: exploratory analysis, wrangling, visualization, and data storytelling without a strong modeling focus.
- `Stats`: A/B testing, experimental design, causal thinking, and statistical study notes.
- `Tool`: apps, packages, scripts, scrapers, utilities, and things built to help people work.

## Good Project File Names

Use lowercase words and hyphens:

```text
2026-05-27-customer-churn-model.md
2026-06-02-llm-study-assistant.md
2026-06-14-sales-dashboard.md
```

## Minimum Project Template

```markdown
---
title: "Customer Churn Prediction"
date: 2026-05-27
status: "Completed"
field: "ML"
tools:
  - Python
  - Pandas
  - Scikit-learn
github: "https://github.com/..."
demo:
kaggle:
excerpt: "A machine learning model that predicts customer churn and explains the strongest risk signals."
---

## The Short Version

I built...

## Problem

...

## Approach

...

## Result

...

## What I Learned

...
```
