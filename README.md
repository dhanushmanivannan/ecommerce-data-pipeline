# 🛒 E-Commerce Sales Analytics Pipeline

> End-to-end Data Engineering pipeline built with **PySpark**, 
> **Databricks**, and **Delta Lake** using Medallion Architecture.

---

## 📌 Business Problem

E-commerce businesses generate millions of order records daily but 
struggle to answer basic questions like:
- What are our top-selling products this month?
- Which customers are at risk of churning?
- What is our revenue breakdown by country?

This pipeline transforms raw transactional data into business-ready 
KPI tables that answer these questions reliably at scale.

---

## 🏗️ Architecture

```
Raw CSV (UCI Online Retail II)
        ↓
🥉 Bronze Layer  — Raw ingestion, metadata columns added
        ↓
🥈 Silver Layer  — Cleaned, validated, typed, deduplicated
        ↓
🥇 Gold Layer    — Business KPIs, aggregations, RFM scores
        ↓
📊 Analytics     — Executive dashboard, customer segments
```

---

## 📦 Dataset

| Property | Detail |
|---|---|
| Source | UCI Online Retail II |
| Records | ~1 million transactions |
| Period | 2009–2011 |
| Domain | UK-based online retail |
| Link | [Kaggle](https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci) |

---

## 🔧 Tech Stack

| Tool | Purpose |
|---|---|
| Databricks Free Edition | Compute + Notebook environment |
| PySpark | Data processing and transformations |
| Delta Lake | ACID-compliant storage layer |
| Unity Catalog | Table management (catalog.schema.table) |
| Python | Scripting and automation |

---

## 📁 Project Structure

```
ecommerce-data-pipeline/
├── notebooks/
│   ├── 01_bronze_ingestion.ipynb
│   ├── 02_silver_cleaning.ipynb
│   ├── 03_gold_aggregations.ipynb
│   ├── 04_data_quality_checks.ipynb
│   └── 05_analytics_kpis.ipynb
├── architecture/
│   └── architecture_diagram.png
├── sample_data/
│   └── sample_100_rows.csv
└── README.md
```

---

## 🥉 Bronze Layer
- Reads raw data from Unity Catalog managed table
- Adds `_ingestion_timestamp` and `_source_table` metadata columns
- Fixes invalid column names (spaces → underscores)
- Writes to `bronze_online_retail` Delta table

## 🥈 Silver Layer
- Renames all columns to snake_case
- Casts data types (string → timestamp, float → double)
- Removes nulls in critical columns
- Removes duplicate invoice+stock combinations
- Filters cancellations (Invoice starting with C)
- Filters negative quantities and zero prices
- Adds derived column `total_amount = quantity × unit_price`
- Writes to `silver_online_retail` Delta table

## 🥇 Gold Layer
| Table | Description |
|---|---|
| `gold_revenue_by_country` | Revenue, orders, customers per country |
| `gold_top_products` | Top 50 products ranked by revenue |
| `gold_monthly_trend` | Monthly revenue and order counts |
| `gold_customer_rfm` | Recency, Frequency, Monetary per customer |
| `gold_executive_kpis` | Top-line business metrics |
| `gold_customer_segments` | RFM-based customer tiers |
| `gold_monthly_growth` | Month-on-month revenue growth % |
| `gold_hourly_pattern` | Orders and revenue by hour of day |
| `gold_ytd_revenue` | Cumulative year-to-date revenue |
| `gold_day_of_week_pattern` | Sales pattern by day of week |

---

## ✅ Data Quality Checks

Automated quality checks run after every pipeline execution:
- Row count validation (Bronze vs Silver)
- Null checks on all critical columns
- Duplicate detection
- Business rule validation (quantities, prices, cancellations)
- Date range validation
- Gold table row count verification
- Results saved to `data_quality_report` table

---

## 🚀 How to Run

1. Sign up at [community.cloud.databricks.com](https://community.cloud.databricks.com)
2. Upload the UCI Online Retail II dataset as a managed table
3. Run notebooks in order: `01 → 02 → 03 → 04 → 05`
4. Query Gold tables in Databricks SQL

---

## 💡 Key PySpark Concepts Demonstrated

- Medallion Architecture (Bronze / Silver / Gold)
- Delta Lake ACID transactions
- Window functions (`dense_rank`, `lag`, running totals)
- RFM customer segmentation
- Data quality assertions
- Unity Catalog table management
- Partitioned writes for query optimization
- Lazy evaluation and caching

---

## 📊 Key Business Insights Generated

- 🇬🇧 United Kingdom drives ~85% of total revenue
- 🏆 Top product generates 3x revenue of second-placed product
- 🕙 Peak order hours are 10 AM–12 PM
- 🔄 Repeat purchase rate indicates customer loyalty health
- 📈 Month-on-month revenue growth trends visible across 2 years

---

## 🔮 Future Enhancements

- [ ] Azure Data Lake Gen2 for scalable cloud storage
- [ ] Azure Data Factory for pipeline orchestration
- [ ] Databricks Workflows for automated scheduling
- [ ] Delta Live Tables for declarative pipeline definition
- [ ] Real-time streaming with Kafka + Structured Streaming
- [ ] Power BI dashboard connected to Gold layer

---

## 👨‍💻 Author

**Dhanush Manivannan**  
Data Engineer | PySpark | Databricks | Azure  
[LinkedIn](https://linkedin.com/in/dhanush-manivannan-88b470190) | 
[GitHub](https://github.com/dhanushmanivannan)
