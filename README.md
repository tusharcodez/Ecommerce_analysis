# 🤖 AI-Assisted E-commerce Data Analytics & Power BI Dashboard

An end-to-end **E-commerce Data Analytics project** combining **SQL, Databricks, Python, Pandas, Generative AI, and Power BI** to transform transactional data into actionable business insights.

The project focuses on **revenue analysis, customer behavior, transaction frequency, geographic performance, YoY growth, payment analysis, and Root Cause Analysis (RCA)**.

> **Project Status:** 🟢 Working Prototype — Databricks SQL + Python/Google Colab + Power BI

---
# 🚀 Google Colab The project can be run and demonstrated directly in Google Colab. [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/18VegDvLUmt-iGxR9qxkgXDeeFYt-r4-z?usp=sharing)
---

 ### 🔗 Databricks SQL Query

[View SQL Query in Databricks](https://dbc-41c04ce0-5cc4.cloud.databricks.com/editor/queries/3847591861351702?o=7474655575965593)



# 📊 Project Overview

This project demonstrates how an analyst can take raw e-commerce transaction data and convert it into a complete **business intelligence solution**.

The workflow covers:

```text
Raw Transaction Data
        ↓
Databricks SQL
        ↓
Data Validation & Transformation
        ↓
Business KPI Analysis
        ↓
Customer & Geographic Analysis
        ↓
Root Cause Analysis
        ↓
Python / Pandas Analysis
        ↓
Power BI Dashboard
        ↓
Business Recommendations
```

The project also incorporates **Generative AI** to assist with natural-language-to-SQL analysis and analytical interpretation.

---

# 🎯 Business Problem

The objective is to understand:

* Which geographic areas are performing well or poorly?
* Why is a particular Upzila underperforming?
* Is poor performance caused by transaction volume or transaction value?
* How frequently are customers transacting?
* Which customers are becoming inactive?
* Which products contribute most to revenue?
* How is revenue changing year-over-year?
* Which payment methods contribute to revenue?
* Where are the biggest business opportunities?

The analysis ultimately focuses on identifying **actionable business drivers rather than simply reporting KPIs**.

---

# 🎯 Project Objectives

### Business Objectives

* Analyze revenue and transaction performance.
* Identify underperforming geographic areas.
* Compare Upzila performance against benchmarks.
* Analyze customer transaction frequency.
* Identify customer inactivity and retention risks.
* Perform product contribution analysis.
* Analyze payment-method performance.
* Calculate YoY revenue growth.
* Perform Root Cause Analysis.
* Quantify potential revenue opportunities.
* Build an interactive Power BI dashboard.

### Technical Objectives

* Write analytical SQL queries using Databricks SQL.
* Use CTEs and window functions.
* Perform multi-table joins.
* Use `LAG()` for YoY analysis.
* Use `PERCENTILE_APPROX()` for median benchmarking.
* Perform customer-level aggregation.
* Transform and analyze data using Pandas.
* Build interactive Power BI visualizations.
* Use DAX for analytical KPIs.
* Apply Generative AI for analytical assistance.

---

# 🔎 Key Business Findings

## Geographic Performance

The analysis identified **Sylhet as the lowest-performing Upzila** based on revenue.

| Upzila     |      Revenue | Revenue / Customer | Performance vs Median |
| ---------- | -----------: | -----------------: | --------------------: |
| Dhaka      |     ₹4.08 Cr |             ₹4,435 |              +260.38% |
| Chittagong |     ₹1.98 Cr |             ₹2,150 |               +74.72% |
| Rajshahi   |     ₹1.21 Cr |             ₹1,316 |                +6.96% |
| Khulna     |     ₹1.13 Cr |             ₹1,231 |                 0.00% |
| Rangpur    |     ₹0.84 Cr |               ₹918 |               -25.48% |
| Barisal    |     ₹0.75 Cr |               ₹819 |               -33.52% |
| **Sylhet** | **₹0.55 Cr** |           **₹601** |           **-51.27%** |

---

## 🔴 Sylhet Root Cause

The analysis indicates that Sylhet's underperformance is primarily associated with:

1. **Low customer transaction frequency**
2. **High customer inactivity**
3. **Low revenue generated per customer**

Product mix and store-level performance did not indicate a single dominant operational issue.

---

## 👥 Customer Frequency

Sylhet customer frequency distribution:

| Frequency Segment | Customers |  Share |
| ----------------- | --------: | -----: |
| 1 Transaction     |       188 |  2.05% |
| 2–5 Transactions  |     4,429 | 48.32% |
| 6–10 Transactions |     4,255 | 46.42% |
| 11+ Transactions  |       294 |  3.21% |

Only **3.21% of Sylhet customers** belong to the high-frequency 11+ transaction segment.

---

## ⏳ Customer Recency

Sylhet customer activity:

| Customer Status        | Customers |      Share |
| ---------------------- | --------: | ---------: |
| Active ≤30 Days        |       573 |      6.25% |
| Inactive 31–90 Days    |     1,067 |     11.65% |
| Inactive 91–180 Days   |     1,437 |     15.68% |
| **Inactive 180+ Days** | **6,089** | **66.45%** |

This indicates a significant **customer retention/reactivation opportunity**.

> Customer inactivity is treated as a retention-risk indicator and not as confirmed churn.

---

# 🏗️ Data Architecture

The project follows a dimensional/star-schema-style structure.

```text
                    ┌─────────────────┐
                    │   customer_dim  │
                    └────────┬────────┘
                             │
                             │
┌──────────────┐      ┌──────▼───────┐      ┌──────────────┐
│  time_dim    │─────►│  fact_table  │◄─────│   item_dim   │
└──────────────┘      └──────┬───────┘      └──────────────┘
                             │
                             │
                    ┌────────▼────────┐
                    │   store_dim     │
                    └─────────────────┘
                             │
                    ┌────────▼────────┐
                    │    trans_dim    │
                    └─────────────────┘
```

---

# 🗄️ Database Schema

### Fact Table

`fact_table`

| Column        | Description                   |
| ------------- | ----------------------------- |
| payment_key   | Payment/transaction reference |
| coustomer_key | Customer identifier           |
| time_key      | Time dimension key            |
| item_key      | Product identifier            |
| store_key     | Store identifier              |
| quantity      | Quantity sold                 |
| unit          | Unit of measurement           |
| unit_price    | Price per unit                |
| total_price   | Transaction revenue           |

### Customer Dimension

`customer_dim`

```text
customer_key
name
contact_no
nid
```

### Item Dimension

`item_dim`

```text
item_key
item_name
desc
unit_price
man_country
supplier
unit
```

### Store Dimension

`store_dim`

```text
store_key
division
district
upzila
```

### Time Dimension

`time_dim`

```text
time_key
date
hour
day
week
month
quarter
year
month_name
month_number
```

### Transaction Dimension

`trans_dim`

```text
payment_key
trans_type
bank_name
```

---

# 🤖 AI-Assisted Analytics Workflow

Generative AI is used as an **analytical assistant** rather than as a replacement for SQL or business validation.

```text
Business Question
       ↓
Natural Language Prompt
       ↓
Generative AI
       ↓
SQL Generation
       ↓
SQL Validation
       ↓
Databricks SQL
       ↓
Query Results
       ↓
Pandas / AI-Assisted Interpretation
       ↓
Business Insight
```

Example business questions:

```text
"Which Upzila is underperforming?"

"Show YoY revenue growth by Upzila."

"Which customers have low transaction frequency?"

"Why is Sylhet underperforming?"

"What is the potential revenue opportunity?"
```

---

# 🧮 SQL Analysis

The project uses Databricks SQL for analytical processing.

### SQL techniques used

* CTEs
* Window Functions
* `LAG()`
* `CASE WHEN`
* `COUNT()`
* `COUNT(DISTINCT)`
* `SUM()`
* `AVG()`
* `PERCENTILE_APPROX()`
* `NULLIF()`
* Conditional Aggregation
* Multi-table JOINs
* Customer-level aggregation
* Geographic aggregation
* YoY analysis


---

# 📈 Year-over-Year Revenue Analysis

Revenue is aggregated by Upzila and year and previous-year revenue is calculated using a window function.

```sql
WITH yearly_revenue AS (
    SELECT
        s.upzila,
        t.year,
        SUM(f.total_price) AS revenue
    FROM fact_table f
    JOIN store_dim s
        ON f.store_key = s.store_key
    JOIN time_dim t
        ON f.time_key = t.time_key
    GROUP BY
        s.upzila,
        t.year
),
yoy_calculation AS (
    SELECT
        upzila,
        year,
        revenue,
        LAG(revenue) OVER (
            PARTITION BY upzila
            ORDER BY year
        ) AS previous_year_revenue
    FROM yearly_revenue
)
SELECT
    upzila,
    year,
    ROUND(revenue, 2) AS revenue,
    ROUND(previous_year_revenue, 2) AS previous_year_revenue,
    ROUND(
        (revenue - previous_year_revenue)
        * 100.0 /
        NULLIF(previous_year_revenue, 0),
        2
    ) AS yoy_growth_percentage
FROM yoy_calculation
ORDER BY upzila, year;
```




# 📍 Geographic Performance Analysis

Revenue was aggregated at the Upzila level and compared against the median benchmark.

The median benchmark was used instead of only relying on the mean because it provides a more robust comparison when geographic revenue is unevenly distributed.

### Key result

**Median revenue per customer:** ₹1,230.73

**Sylhet revenue per customer:** ₹601.38

**Gap:** ₹629.35 per customer

**Variance:** -51.27%

---

# 👥 Customer Analysis

Customer analysis includes:

### Transaction Frequency

Customers are segmented into:

```text
1 Transaction
2–5 Transactions
6–10 Transactions
11+ Transactions
```

### Customer Recency

Customers are classified based on days since their last transaction:

```text
0–30 Days
31–90 Days
91–180 Days
180+ Days
```

This enables identification of:

* High-frequency customers
* Low-frequency customers
* One-time customers
* Inactive customers
* Retention opportunities
* Reactivation opportunities


---

# 🔍 Root Cause Analysis — Sylhet

## Problem

Sylhet generates the lowest revenue among the analyzed Upzilas.

### Investigation Framework

```text
Revenue
   ↓
Customer Base
   ↓
Transaction Volume
   ↓
Transaction Value
   ↓
Customer Frequency
   ↓
Customer Recency
   ↓
Store Performance
   ↓
Product Mix
   ↓
Root Cause
```

---

## 1️⃣ Customer Base

Sylhet has approximately **9,166 customers**.

Therefore, customer count alone does not explain the revenue gap.

---

## 2️⃣ Transaction Value

Sylhet's average revenue per transaction is approximately:

**₹105.67**

This is broadly consistent with other Upzilas.

Therefore, transaction value is **not the primary driver**.

---

## 3️⃣ Customer Frequency

Only **3.21%** of Sylhet customers have 11+ transactions.

This indicates weak repeat transaction frequency compared with stronger-performing regions.

---

## 4️⃣ Customer Recency

Approximately **66.45%** of Sylhet customers have not transacted in more than 180 days.

This indicates a significant **customer inactivity and retention risk**.

---

## 5️⃣ Store Performance

Store-level analysis did not identify one store as the dominant cause.

The lowest-performing Sylhet store generated approximately:

**₹135K**

while the highest generated approximately:

**₹159K**

This suggests that the issue is relatively broad-based rather than isolated to a single store.

---

## 6️⃣ Product Mix

Product contribution analysis showed that Sylhet's product mix is broadly aligned with the overall business.

Examples of products with relatively stronger Sylhet representation include:

* Dixie PerfectTouch Paper Cups
* Monster Zero Ultra Variety
* San Pellegrino
* Red Bull Sugar Free
* Dunkin' Donuts Original Blend

However, product mix did not emerge as the primary root cause.

---

# 🎯 Final Root Cause

### 🔴 Primary Root Cause

> **Low customer transaction frequency combined with high customer inactivity.**

The analysis suggests that Sylhet's revenue gap is driven more by **customer engagement and repeat purchasing behavior** than by transaction value, product mix, or a single store.

---

# 💰 Estimated Revenue Opportunity

Current Sylhet revenue:

**₹55.12L**

Current revenue per customer:

**₹601**

Benchmark revenue per customer:

**₹1,231**

Estimated benchmark revenue:

**₹112.81L**

### Estimated Revenue Opportunity

# **₹57.69L**

This is a **benchmark-based opportunity estimate**, not a guaranteed revenue forecast.

---

# 💡 Business Recommendations

## 1. Customer Reactivation

Target customers who have been inactive for 180+ days.

Potential actions:

* Personalized offers
* Win-back campaigns
* Email/SMS campaigns
* Limited-time discounts

---

## 2. Increase Transaction Frequency

Target customers in the:

```text
2–5 Transactions
6–10 Transactions
```

segments and encourage repeat purchases.

---

## 3. Retention Monitoring

Create monthly monitoring for:

* 30-day activity
* 90-day activity
* 180-day inactivity
* Repeat transaction rate

---



## 4. Geographic Benchmarking

Monitor:

```text
Revenue
Revenue / Customer
Transactions
Customer Frequency
Customer Recency
YoY Growth
```

across all Upzilas.

---


---

# 📌 Power BI KPIs

The dashboard includes:

| KPI                   | Purpose                       |
| --------------------- | ----------------------------- |
| Total Revenue         | Overall business revenue      |
| Total Order           | Number of Order records       |
| AOV                   | Average Order value           |
| YoY Revenue Growth    | Annual performance            |
| Total Quantity        | Total Quantity Sold           |


> **Important:** The dataset does not contain a dedicated `order_id`. Therefore, the project uses **transaction-based metrics** rather than claiming true order-level metrics.

---

# 📊 Dashboard Visualizations

The Power BI dashboard includes:

* KPI Cards
* Revenue Trend
* YoY Growth
* Revenue by Upzila
* Revenue vs Transactions
* Customer Frequency
* Customer Recency
* Revenue per Customer
* Top Products
* Bottom Products
* Payment Method Analysis
* Geographic Performance
* Root Cause Analysis
* Business Opportunity Analysis

Interactive filters include:

* Year
* Month
* Division
* District
* Upzila
* Product
* Payment Type

---

# 🧰 Technology Stack

### Programming & Analytics

* Python
* Pandas
* NumPy
* Jupyter Notebook
* Google Colab

### SQL & Data Platform

* SQL
* Databricks SQL
* SQL Server
* CTEs
* Window Functions

### Business Intelligence

* Microsoft Power BI
* DAX
* Power Query

### AI / GenAI

* Generative AI
* Gemini
* LangChain
* Natural Language to SQL

### Other

* Regex
* Git
* GitHub

---

# ⚠️ Limitations

* The dataset does not contain a dedicated `order_id`.
* `payment_key` is treated as a payment/transaction reference and is **not assumed to be an order ID**.
* Therefore, transaction-level metrics are used instead of true order-level metrics.
* Revenue opportunity is a benchmark-based estimate.
* Customer inactivity is treated as a retention-risk indicator, not confirmed churn.
* Product analysis is based on available transactional data.
* The analysis is descriptive/diagnostic and does not establish causal relationships.

---

# 🚀 Future Improvements

### Data Engineering

* Build an automated ETL pipeline.
* Implement Bronze → Silver → Gold architecture.
* Add data quality checks.
* Automate Databricks workflows.

### Analytics

* Customer RFM segmentation.
* Customer Lifetime Value.
* Cohort analysis.
* Churn prediction.
* Demand forecasting.
* Product recommendation system.

### AI

* Automated natural-language analytics.
* AI-generated business summaries.
* Automated anomaly detection.
* AI-powered Root Cause Analysis.
* Conversational Power BI analytics.

### Cloud

Potential future integration with:

* Azure
* AWS
* GCP

---

# 🎓 Learning Outcomes

Through this project, I developed practical experience in:

* Business problem solving
* SQL analytics
* Databricks SQL
* Data modeling
* Dimensional analysis
* Customer segmentation
* Customer retention analysis
* Geographic performance analysis
* Root Cause Analysis
* YoY analysis
* Power BI dashboard development
* DAX
* Power Query
* Python/Pandas
* Generative AI
* Natural Language to SQL
* Business storytelling
* Data-driven recommendations

---

# 👨‍💻 Author

**Tushar Upadhyay**

Aspiring **Data Analyst / Business Analyst**

### Core Skills

```text
SQL | Python | Pandas | Power BI | DAX
Databricks | Excel | Data Analysis
Generative AI | Gemini | LangChain
ETL | Data Modeling | Root Cause Analysis
Business Intelligence | Data Visualization
```

---

# ⭐ Project Goal

The goal of this project is to demonstrate how **SQL, Python, Generative AI, Databricks and Power BI** can be combined to move from:

> **Raw Data → Analysis → Root Cause → Business Insight → Action**

rather than simply creating a dashboard.

---
