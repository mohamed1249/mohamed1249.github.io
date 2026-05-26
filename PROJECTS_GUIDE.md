# Adding a Project

Each project is one Markdown file inside `_projects`.

## Fast Steps

1. Copy `templates/project.md`.
2. Paste it into `_projects`.
3. Rename it like this:

```text
2026-05-27-my-project-name.md
```

4. Edit the title, date, tools, links, excerpt, and sections.
5. Commit and push. The project will appear automatically on `/ai-data/`.

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
