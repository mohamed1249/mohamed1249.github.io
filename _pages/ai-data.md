---
title: "Data Science + AI"
permalink: /ai-data/
classes: wide
---

<p class="page-intro">
This is the data-science side of the archive: datasets, notebooks, models, dashboards, experiments, and the discipline of turning noise into a useful signal.
</p>

## Workbench

| Area | What Belongs Here |
| --- | --- |
| Data Science | Analysis notebooks, dashboards, statistical thinking, visualization, and storytelling with data. |
| Machine Learning | Classification, regression, NLP, recommendation systems, and learning notes. |
| AI Engineering | LLM experiments, agents, prompt systems, model evaluation, automation, and applied AI tools. |
| Experiments | Half-built prototypes, questions worth testing, and small technical discoveries. |

## Featured Projects

{% assign projects = site.projects | sort: "date" | reverse %}
{% assign ai_projects = projects | where: "field", "AI" %}
{% assign ml_projects = projects | where: "field", "ML" %}
{% assign eda_projects = projects | where: "field", "EDA" %}
{% assign stats_projects = projects | where: "field", "Stats" %}
{% assign tool_projects = projects | where: "field", "Tool" %}
{% if projects.size > 0 %}
  <div class="project-tabs">
    <input type="radio" id="projects-all" name="project-tabs" checked>
    <input type="radio" id="projects-ai" name="project-tabs">
    <input type="radio" id="projects-ml" name="project-tabs">
    <input type="radio" id="projects-eda" name="project-tabs">
    <input type="radio" id="projects-stats" name="project-tabs">
    <input type="radio" id="projects-tool" name="project-tabs">

    <div class="project-tab-nav" aria-label="Project categories">
      <label for="projects-all">All <span>{{ projects.size }}</span></label>
      <label for="projects-ai">AI <span>{{ ai_projects.size }}</span></label>
      <label for="projects-ml">ML <span>{{ ml_projects.size }}</span></label>
      <label for="projects-eda">EDA <span>{{ eda_projects.size }}</span></label>
      <label for="projects-stats">Stats <span>{{ stats_projects.size }}</span></label>
      <label for="projects-tool">Tools <span>{{ tool_projects.size }}</span></label>
    </div>

    <div class="project-tab-panels">
      <section class="project-panel project-panel--all">
        <div class="project-list">
        {% for project in projects %}
          <article class="project-card">
            <p class="project-card__meta">
              {% if project.date %}{{ project.date | date: "%Y" }}{% endif %}
              {% if project.status %}<span>{{ project.status }}</span>{% endif %}
              {% if project.field %}<span>{{ project.field }}</span>{% endif %}
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
      </section>

      <section class="project-panel project-panel--ai">
        <div class="project-list">
        {% for project in ai_projects %}
          <article class="project-card">
            <p class="project-card__meta">
              {% if project.date %}{{ project.date | date: "%Y" }}{% endif %}
              {% if project.status %}<span>{{ project.status }}</span>{% endif %}
              <span>AI</span>
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
      </section>

      <section class="project-panel project-panel--ml">
        <div class="project-list">
        {% for project in ml_projects %}
          <article class="project-card">
            <p class="project-card__meta">
              {% if project.date %}{{ project.date | date: "%Y" }}{% endif %}
              {% if project.status %}<span>{{ project.status }}</span>{% endif %}
              <span>ML</span>
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
      </section>

      <section class="project-panel project-panel--eda">
        <div class="project-list">
        {% for project in eda_projects %}
          <article class="project-card">
            <p class="project-card__meta">
              {% if project.date %}{{ project.date | date: "%Y" }}{% endif %}
              {% if project.status %}<span>{{ project.status }}</span>{% endif %}
              <span>EDA</span>
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
      </section>

      <section class="project-panel project-panel--stats">
        <div class="project-list">
        {% for project in stats_projects %}
          <article class="project-card">
            <p class="project-card__meta">
              {% if project.date %}{{ project.date | date: "%Y" }}{% endif %}
              {% if project.status %}<span>{{ project.status }}</span>{% endif %}
              <span>Stats</span>
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
      </section>

      <section class="project-panel project-panel--tool">
        <div class="project-list">
        {% for project in tool_projects %}
          <article class="project-card">
            <p class="project-card__meta">
              {% if project.date %}{{ project.date | date: "%Y" }}{% endif %}
              {% if project.status %}<span>{{ project.status }}</span>{% endif %}
              <span>Tool</span>
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
      </section>
    </div>
  </div>
{% else %}
  <p>Projects are being curated.</p>
{% endif %}

## Research Notes

The projects here are not only outputs. They are traces of questions: how to evaluate models honestly, how to clean data without losing meaning, how to turn notebooks into systems, and how to make technical work readable without flattening it.
