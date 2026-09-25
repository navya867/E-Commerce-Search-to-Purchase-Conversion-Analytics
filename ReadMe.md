# E-Commerce Search-to-Purchase Conversion Analysis

## Executive Summary

E-commerce businesses need to understand where users drop off between search and purchase. Using **36M+ browsing events and 819K+ search interactions**, I analyzed the customer journey from **search → click → product detail → add to cart → purchase** using SQL, Python, and Power BI. The analysis identified major funnel drop-offs and differences in conversion behavior across search-result volume, product attributes, time, and session search frequency.

## Business Problem

Search engagement does not necessarily translate into purchases. The objective was to determine **where users were dropping out of the search-to-purchase journey and which user/product segments showed different conversion behavior**.

Key questions included:

* Where are the largest funnel drop-offs?
* Does search-result volume affect engagement?
* How does conversion vary across price buckets and product categories?
* How does search frequency relate to purchase behavior?

## Methodology

1. **Python/Pandas** — validated, cleaned, timestamped, and normalized the raw search and browsing data.
2. **MySQL** — built the search-to-purchase funnel using session, product, and timestamp-level event matching.
3. **SQL Analysis** — segmented funnel performance by result count, price bucket, category, time, and session search frequency.
4. **Power BI** — created dashboard-ready analytical datasets to monitor search engagement, funnel drop-offs, and conversion behavior.

## Skills

**SQL/MySQL:** Joins, CTEs, temporary tables, aggregations, CASE statements, date/time analysis, funnel analysis, segmentation

**Python:** Pandas, data cleaning, data validation, transformation, exploratory analysis

**Power BI:** Data modeling, KPI development, funnel analysis, data visualization, dashboarding

## Results & Business Insights

* **10.01%** of clicked products reached a product-detail event, followed by **12.55% detail-to-add** and **32.71% add-to-purchase** conversion.
* Search-result volume showed **non-linear conversion behavior**, with no consistent improvement as the number of returned products increased.
* Product **price buckets and categories showed different funnel patterns**, highlighting variation in downstream purchase behavior across product segments.
* Purchase-containing sessions increased from **0.11% for single-search sessions to 1.13% for sessions with 11+ searches**, indicating different observed behavior among search-intensive sessions.
* The Power BI dashboard provides visibility into **search engagement, funnel leakage, product segments, and user search behavior** for further business investigation.

> **Note:** These findings represent observed associations in the dataset and are not treated as causal relationships.

## Next Steps

1. Analyze **search-result position/rank** to understand how product placement affects clicks.
2. Analyze **search-query characteristics** and their relationship with conversion.
3. Measure the **time between funnel stages** to identify delays in the customer journey.
4. Develop predictive analysis to identify **high-intent sessions**.

## Dataset

**Coveo SIGIR E-commerce Dataset**

[Dataset & Documentation](https://github.com/coveooss/SIGIR-ecom-data-challenge?utm_source=chatgpt.com)
