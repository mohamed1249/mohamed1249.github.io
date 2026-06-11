---
title: "EDA: Chinook Music Market Tableau Dashboard"
date: 2022-09-13
status: "Completed"
field: "EDA"
tools:
  - Tableau
  - SQL
  - SQLite
  - Excel
  - Data Visualization
  - Dashboard Design
github: "https://github.com/mohamed1249/Tableau-Dashboards"
demo:
kaggle:
excerpt: "An early Tableau dashboard built on the Chinook music database, exploring sales by country, genre, artist, and revenue."
---

## The Short Version

This is one of my early Tableau dashboards, built on the Chinook music-store database. It explores a simple but useful business question: where does the music market perform best, and which artists or genres drive the strongest sales?

The project is beginner-level compared with my later work, but it shows the foundation: taking relational music-sales data and turning it into readable visual questions.

## Problem

The Chinook database contains tracks, albums, artists, genres, customers, invoices, and invoice lines. The challenge was to turn those tables into a dashboard that answers a few direct business questions:

- Which countries generate the most sales?
- Which genres sell the most?
- Which artists are strongest inside each genre?
- Which artists generate the most revenue?

## Approach

**Data source** - The dashboard uses the Chinook SQLite database and a prepared track workbook. The database includes 3,503 tracks, 412 invoices, 59 customers, and 25 genres.

**Dashboard structure** - The Tableau workbook includes separate views for genre sales, most-selling country, most-selling artist by genre, most-selling genre by artist, and top-gaining artist.

**Visual design** - The dashboard combines a map, bar charts, and artist/genre views so a viewer can move from broad market geography into more specific music-performance patterns.

## Result

The final Tableau dashboard highlights:

- sales volume by country,
- genre performance over time,
- top artists by quantity and revenue,
- relationships between artists and their strongest genres.

The clearest pattern is that Rock, Metal, Latin, Pop, and Alternative/Punk dominate much of the dashboard, with artists like Iron Maiden, U2, Metallica, and other recurring names appearing across the sales views.

## What I Learned

This project helped me understand how dashboard design depends on choosing a small number of questions. The dataset had many tables, but the dashboard became readable only after focusing on country, genre, artist, quantity, and revenue.

It was an early step in learning Tableau as a storytelling tool, not just a charting interface.

## Links

{% if page.github %}- [GitHub]({{ page.github }}){% endif %}
{% if page.demo %}- [Tableau Public]({{ page.demo }}){% endif %}
