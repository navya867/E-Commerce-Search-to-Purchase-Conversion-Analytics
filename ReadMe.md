E-Commerce Search-to-Purchase Conversion Analytics
Analyzing Search Engagement, Funnel Drop-offs & Purchase Behavior

Project Overview

E-commerce platforms generate large volumes of search and browsing events, but high search activity does not necessarily translate into product purchases.

This project analyzes search-to-purchase behavior to understand:

How effectively users engage with search results
Where users drop off between search and purchase
Whether search-result volume is associated with different conversion behavior
How product price buckets and categories relate to funnel performance
How user search frequency differs across sessions
How search engagement varies by time of day

The analysis converts raw event-level data into reusable analytical datasets and business KPIs that can be used for dashboarding and further investigation.

**Business Objective**

The primary objective is to understand where and how users move through the e-commerce search journey:

Search → Click → Product Detail → Add to Cart → Purchase

The project focuses on identifying funnel leakage and behavioral patterns that can help e-commerce teams investigate opportunities around search experience, product discovery, and conversion.

**Dataset**

The project uses the Coveo SIGIR E-commerce Dataset, containing anonymized e-commerce search and browsing interactions.

**Dataset scale**
Dataset	Records	Purpose
Browsing events	36M+	Product and user interaction events
Search interactions	819K+	Search queries, returned products and clicks
Product catalog	66K+	Product attributes and price/category information

Main data sources
browsing_train.csv — browsing and product interaction events
search_train.csv — search interactions and clicked/returned products
sku_to_content.csv — product-level attributes

**Key Business Questions**
Search Engagement
How many searches result in a product click?
How does search-result volume relate to search engagement?
How does search activity vary by hour and day?
Conversion Funnel
How many clicked products reach product detail?
How many progress to add-to-cart?
How many ultimately reach purchase?
Where are the major funnel drop-offs?
Product Behavior
Does conversion behavior vary across price buckets?
How does funnel performance differ across product categories?
User Behavior
Does search frequency differ across sessions with and without purchases?
How does search frequency relate to downstream purchase behavior?
🛠️ Analytical Methodology
1. Data Validation

Before analysis, the raw datasets were profiled to understand:

Dataset size and structure
Data types
Missing-value patterns
Event types
Product-action relationships
Duplicate records
Search-result and click-list structure

Missing values were not removed blindly. Their meaning was investigated first because a missing search-result or click list can represent different tracking scenarios.

2. Data Preparation with Python

Python/Pandas was used for initial preparation and validation.

Key transformations
Converted Unix epoch timestamps into datetime fields
Created a unique search_id
Parsed returned-product and clicked-product lists
Normalized search-product relationships
Created clean analytical copies while preserving raw data
Validated relationships between search results and clicks

The normalized search data was then loaded into MySQL for analytical querying.

**SQL Data Model**

The analysis was structured around four main analytical tables:

                   
**Search-to-Purchase Funnel**

A chronological funnel was created by connecting search clicks to subsequent product events using:

session_id_hash
product_sku_hash
Event timestamps

The sequence was enforced as:

Search
   ↓
Product Click
   ↓
Product Detail
   ↓
Add to Cart
   ↓
Purchase

Only events occurring after the relevant previous stage were counted.

This prevents a later event from being incorrectly attributed to an earlier funnel stage.

**Key KPIs**

The analysis identified:

KPI	Result
Total searches	819,516
Searches with a click	179,495
Search click rate	29.77%
Clicked search-product records	256,809
Click → Detail	10.01%
Detail → Add	12.55%
Add → Purchase	32.71%
Click → Purchase	0.41%
Funnel
256,809
Clicked Products
      │
      ▼
25,706
Reached Product Detail
      │
      ▼
3,225
Added to Cart
      │
      ▼
1,055
Purchased

These are observed event relationships, not causal estimates.

**Analysis Performed**
1. Search Result Volume

Searches were grouped by the number of products returned:

1–5
6–10
11–15
16–20
21–25

This was used to examine whether the amount of available search results was associated with different engagement and downstream funnel behavior.

The analysis showed that conversion did not follow a simple monotonic pattern as the number of returned products increased.

2. Price Bucket Analysis

Products were analyzed across the available encoded price buckets.

The analysis examined:

Click volume
Product-detail views
Add-to-cart activity
Purchases
Stage-level conversion rates

The results showed differences in funnel behavior across price buckets, with middle price buckets showing stronger observed downstream purchase conversion than some lower and higher buckets.

Because the dataset provides an encoded price_bucket, these results are interpreted as relative price-group behavior rather than actual monetary price effects.

3. Product Category Analysis

Product categories were analyzed using the anonymized category hierarchy.

The dataset contained:

167 distinct categories among clicked products
Most clicked products belonged to depth-3 categories

Parent-level categories were used for the main comparison to avoid over-interpreting very small leaf categories.

Categories were compared using:

Click volume
Detail views
Add-to-cart
Purchases
Funnel conversion rates

A minimum-volume threshold was applied when comparing categories to reduce instability from very small samples.

4. Time-Based Analysis

Search behavior was analyzed by:

Hour of day
Day of week
Observation

Search engagement varied considerably by hour, while day-of-week differences were relatively small.

Hourly search click rates ranged from approximately:

8% → 24%

This analysis helps distinguish traffic volume patterns from conversion behavior, since high search activity does not necessarily mean higher downstream purchase conversion.

**5. Session Search Frequency**

Sessions were grouped based on number of searches:

1
2–3
4–5
6–10
11+
Observed pattern

Session-level purchase incidence increased with search frequency:

Search Frequency	Purchase-containing Sessions
1 search	0.11%
2–3 searches	0.29%
4–5 searches	0.65%
6–10 searches	1.04%
11+ searches	1.13%

This suggests that search-intensive sessions exhibit different purchase behavior.

However, this is an observational relationship and does not establish that performing more searches causes a purchase.

**Key Insights**
1. Search engagement does not equal purchase conversion

Although 29.77% of searches resulted in a click, only a smaller portion of clicked products progressed through the complete purchase funnel.

This highlights the importance of analyzing the complete customer journey rather than relying only on search CTR.

2. The largest funnel leakage occurs after product click

Out of 256K+ clicked products, 25K+ reached a product-detail event and 3K+ reached an add-to-cart event.

This makes the post-click journey an important area for further investigation.

3. Search-result volume has a non-linear relationship with conversion

The number of returned products showed different engagement patterns across ranges rather than a simple “more results = better conversion” relationship.

4. User search frequency is associated with purchase incidence

Sessions with more searches showed higher observed purchase incidence, although the relationship should not be interpreted causally.

5. Conversion behavior differs across product segments

Price buckets and product categories showed different funnel characteristics, suggesting that aggregate conversion rates can hide important product-level differences.

**Power BI Dashboard**

The final analytical tables were designed to support a Power BI dashboard covering:

Executive Overview
Total Searches
Search CTR
Clicked Products
Purchase Conversion
Funnel Drop-off
Search Analytics
Searches by hour
Search-result volume
Search click rate
Result-count segmentation
Conversion Funnel
Search
  ↓
Click
  ↓
Detail
  ↓
Add
  ↓
Purchase
Product Analysis
Price bucket performance
Category performance
Product-level funnel metrics
User Behavior
Searches per session
Purchase incidence by search frequency
Time-based behavior


**Data & Analytical Considerations**

Event attribution

Funnel events were attributed using:

Session + Product + Timestamp

with each downstream event required to occur after the preceding funnel stage.

Clicked products outside returned results

The normalized search data contains 23,711 clicked-product records that were not present in the recorded returned-product list.

These records were retained rather than silently discarded because they represent observed click activity. This should be considered when interpreting strict search-result CTR metrics.

Search-result ranking

Original result ranking was not retained in the normalized search_results table, so the project focuses on result volume and conversion behavior rather than rank-level analysis.

Causality

The project identifies associations and observed behavioral patterns. It does not claim that search-result volume, price, category, or search frequency directly causes changes in purchase behavior.

**Business Value**

This analysis provides a structured way for e-commerce teams to move beyond basic search-volume reporting and investigate:

Where users disengage after search
Which funnel stages have the largest drop-offs
How different product segments behave
How search behavior differs across user sessions
Which behavioral segments may warrant deeper experimentation

The resulting analytical tables can be reused for dashboard reporting, exploratory analysis, and future hypothesis testing.
