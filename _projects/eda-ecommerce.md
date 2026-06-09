---
title: "EDA: E-Commerce — eBay, Flipkart & Pakistan Market Analysis"
date: 2021-11-28
status: "Completed"
field: "EDA"
tools:
  - Python
  - Pandas
  - Matplotlib
  - Seaborn
github:
demo:
kaggle: "https://www.kaggle.com/code/ma12492002/ml-clustering-mall-customer-segmentation-data"
excerpt: "A cross-platform analysis of eBay, Flipkart, and Pakistan's e-commerce market to measure the impact of digital commerce on sales volume, pricing, customer satisfaction, and seasonal trends."
---

## The Short Version

A research-style EDA spanning three e-commerce datasets — eBay (UK orders + product listings), Flipkart (India), and a Pakistan-wide e-commerce dataset. Rather than a single-dataset exploration, this project asks comparative questions across platforms and markets: how much do they sell, at what prices, how are customers rating products, and does festival season actually drive sales?

## Problem

Can data from real e-commerce platforms prove the business case for digital commerce? The notebook frames itself around a central thesis: digital marketing and e-commerce dramatically amplify a company's sales reach — and sets out to support it with concrete statistics on volume, geography, pricing, and customer sentiment across multiple platforms and countries.

## Approach

Four datasets were loaded and independently cleaned — a substantial preprocessing effort given messy price strings (currency symbols, commas), inconsistent date formats, and string-encoded numeric fields. Key analytical threads included:

**Geographic reach:** Drilling from country → county → city to find where eBay orders concentrated (spoiler: London dominated at every level, including a data-cleaning catch where "london" and "London" were being counted separately).

**Pricing:** Raw means were wildly skewed by outliers in both eBay and Flipkart datasets. A consistent IQR-based trimming strategy (cutting above the 75th percentile) was applied to surface more representative average prices — ~$98 for eBay, ~913 INR for Flipkart, ~845 PKR for Pakistan. Flipkart's average discount rate came out at ~52%.

**Seasonality vs. festivals:** Pakistan's 2017 sales data was combined with a scraped public holiday calendar (fetched live via `pd.read_html`) to test whether festival days drive above-average sales. The answer was mostly no — only two of the five peak sales months overlapped with the five most festival-heavy months.

**Customer sentiment:** Feedback and ratings were bucketed into positive/neutral/negative across eBay (order feedback) and both eBay and Flipkart (product ratings). eBay came out at ~95.6% positive; Flipkart's numbers were lower but described as "more realistic."

**Brand vs. sales:** Top-rated brands on eBay (Apple, Samsung, Sony) aligned closely with top-selling brands. On Flipkart, the two best-selling brands (Allure Auto, Voylla) had no ratings data and below-average ratings respectively — suggesting brand loyalty plays out differently across markets.

**Volume benchmarks:** Daily sales rates were computed for all three platforms — ~156/day for eBay, ~157/day for Flipkart, ~741/day across Pakistan's market. November consistently emerged as the top month across both Pakistan years in the dataset.

## Result

- An eBay branch reaches 19 countries from a single storefront, with London as the dominant buyer city.
- ~95.6% positive feedback on eBay orders; Flipkart product ratings skewed positive but less extremely so.
- Festival seasons in Pakistan do **not** reliably predict high sales — only April and August appeared in both the top-selling months and the top festival months.
- Strong brand names (Apple, Samsung) correlate with both high ratings and high sales volume on eBay; the correlation is weaker on Flipkart.
- November is the single strongest sales month in Pakistan's market across multiple years.

## What I Learned

Outliers in price data are the rule, not the exception, in real e-commerce datasets — and the choice of how to handle them (trim vs. transform vs. cap) meaningfully changes the headline number. Scraping live reference data mid-notebook (the Pakistani holiday calendar) to cross-validate an assumption was a useful pattern: rather than guessing whether festivals drive sales, the data was brought in to test it directly. The cross-platform structure also surfaced how easy it is to over-generalize from one market — the brand-loyalty finding on eBay simply didn't hold on Flipkart.

## Links

{% if page.kaggle %}- [Kaggle]({{ page.kaggle }}){% endif %}
