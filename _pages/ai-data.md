---
title: "AI + Data Science"
permalink: /ai-data/
classes: wide
---

<p class="page-intro">
This is the research-lab side of the archive: models, datasets, notebooks, experiments, and the discipline of turning noise into a useful signal.
</p>

## Workbench

| Area | What Belongs Here |
| --- | --- |
| AI Engineering | LLM experiments, agents, prompt systems, model evaluation, automation, and applied AI tools. |
| Data Science | Analysis notebooks, dashboards, statistical thinking, visualization, and storytelling with data. |
| Machine Learning | Classification, regression, NLP, recommendation systems, and learning notes. |
| Experiments | Half-built prototypes, questions worth testing, and small technical discoveries. |

## Featured Projects

{% assign projects = site.projects | sort: "date" | reverse %}
{% if projects.size > 0 %}
  <div class="project-list">
  {% for project in projects %}
    <article class="project-card">
      <p class="project-card__meta">
        {% if project.date %}{{ project.date | date: "%Y" }}{% endif %}
        {% if project.status %}<span>{{ project.status }}</span>{% endif %}
      </p>
      <h3><a href="{{ project.url | relative_url }}">{{ project.title }}</a></h3>
      {% if project.excerpt %}
        <p>{{ project.excerpt | markdownify | strip_html }}</p>
      {% endif %}
      {% if project.tools %}
        <p class="project-card__tools">{{ project.tools | join: " . " }}</p>
      {% endif %}
    </article>
  {% endfor %}
  </div>
{% else %}
  <p>No projects are published yet. When you add files to <code>_projects</code>, they will appear here automatically.</p>
{% endif %}

## Technical Notes

Use this section for things you are learning in public: ideas about evaluation, data cleaning, model behavior, visualization, or the messy parts of building real systems.
