---
title: "Mansoura Mobility: PostgreSQL Database and Analytics"
date: 2026-09-18
status: "Completed"
field: "EDA"
tools:
  - PostgreSQL
  - SQL
  - Python
  - Jupyter
  - Pandas
  - SQLAlchemy
github: "https://github.com/mohamed1249/mansoura-mobility-analytics"
demo:
kaggle:
excerpt: "A 13-table mobility database with reproducible synthetic data, business-rule checks, and a notebook investigation of marketplace performance."
---

## The Short Version

I designed and built a PostgreSQL database for a fictional ride-hailing marketplace around Mansoura and Talkha. The project connects database modeling with a Data Scientist's workflow: querying PostgreSQL from Python notebooks, checking the result grain, and investigating operational patterns.

## Database Design

The 13-table model covers shared accounts, passenger and driver roles, vehicles, zones, directional routes, offers, rides, payment attempts, refunds, ratings, and reports.

An offer can exist without a ride. Accepted offers link to rides, while payments and reports remain separate records. Accounts can hold both passenger and driver roles.

## Data and Workflow

A fixed-seed Python generator creates 180 days of synthetic activity, including 1,000 passengers, 100 drivers, 15 zones, and 14,000 offers. Validation runs before loading and against PostgreSQL afterward.

The notebooks cover connection setup, schema creation, data generation, constraint checks using rolled-back writes, and analysis. SQL queries use parameters and summarize related records before joining them to avoid multiplying rows.

## Marketplace Investigation

The analysis compares offer acceptance, ride completion, cancellations, rating coverage, and reports across pickup zones and time blocks. Python handles segment summaries and conditional formatting. A bounded SQL extract provides rides from El Mokhtalat during the evening for manual review.

This is an exploratory learning project using synthetic records, not evidence about real transport conditions or a validated campaign launch decision. It also documents business rules that are not fully enforced by the schema.

## Links

- [Source code, notebooks, and setup guide]({{ page.github }})
- [Database relationships]({{ page.github }}/blob/main/docs/03_conceptual_erd.md)
- [Marketplace investigation notebook]({{ page.github }}/blob/main/notebooks/04_data_scientist_database_case_study.ipynb)
