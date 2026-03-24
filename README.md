# Dubai Real Estate Lakehouse — Daily Pulse Pipeline

A production-grade, end-to-end Data Engineering project built on Azure Databricks, 
implementing the Medallion Architecture to analyze the Dubai property market.

---

## Project Overview

This project builds an automated **"Daily Pulse"** data platform that ingests, cleans, 
and aggregates millions of dollars in Dubai real estate transactions every morning — 
entirely in the cloud.

The pipeline tracks property transactions across Dubai, delivering clean, analytics-ready 
data to a live Databricks dashboard showing market trends, top investment areas, and 
supply statistics.

---

## Architecture
```
Raw Source (Dubai Pulse API)
         ↓
   🥉 Bronze Layer       → Raw CSV ingestion (transactions, units, projects)
         ↓
   🥈 Silver Layer       → Cleaned, joined, and enriched Delta tables
         ↓
   🥇 Gold Layer         → Aggregated KPI tables ready for analytics
         ↓
   📊 Power BI Bridge    → Registered managed tables for dashboards
```

**Cloud Stack:**
- **Azure Data Lake Gen2** — Storage (Bronze / Silver / Gold containers)
- **Azure Databricks** — PySpark transformations & orchestration
- **Delta Lake** — ACID transactions, time travel, schema enforcement
- **Databricks Workflows** — Automated daily scheduling
- **GitHub** — Version control & CI/CD

---

##  Repository Structure
```
dubai-real-estate-project/
│
├── 1_ingest_bronze_snapshot.ipynb        # Ingest raw transactions into Bronze
├── 2_ingest_dimensions_units_projects.ipynb  # Ingest units & projects dimensions
├── 3_silver_transformation.ipynb         # Clean, join & enrich data into Silver
├── 4_gold_transformation.ipynb           # Aggregate KPIs into Gold layer
└── 5_powerbi_bridge.ipynb                # Register Gold tables for BI layer
```

---

##  Pipeline Stages

### 1️ Bronze — Raw Ingestion
- Ingests raw transaction CSV from Dubai Pulse government open data
- Ingests dimension tables: Units and Projects
- Stores raw files as `current.csv` in landing zones
- Archives previous files with date-stamped filenames
- Retention policy: auto-deletes archive files older than 14 days

### 2️ Silver — Transformation
- Casts and cleans all data types (dates, integers, doubles)
- Joins transactions with project dimension data
- Enriches with calculated fields:
  - `price_per_sqft` — price divided by area
  - `is_offplan_flag` — identifies off-plan vs ready properties
- Writes output as Delta format for ACID compliance

### 3️ Gold — Aggregation
- **Sales Overview** — monthly sales trends (total AED, transaction count, avg price/sqft)
- **Area Stats** — top performing areas by total sales volume
- **Supply Stats** — inventory breakdown by bedroom type and project

### 4️ Power BI Bridge
- Registers Gold Delta tables as managed Databricks tables
- Enables direct SQL querying for dashboards and BI tools

---

## ⚙️ Automated Orchestration

The full pipeline runs automatically via **Databricks Workflows**:
```
1_ingest_transactions
        ↓
2_ingest_dimensions
        ↓
3_transform_silver
        ↓
4_transform_gold
        ↓
5_refresh_bridge
```

**Schedule:** Daily at 06:00 AM

---

## Key KPIs Tracked

| KPI | Description |
|-----|-------------|
| Monthly Sales Volume | Total AED transacted per month |
| Transaction Count | Number of deals closed per month |
| Avg Price per SqFt | Market price benchmark over time |
| Top Areas by Volume | Highest value investment locations |
| Supply by Bedroom | Inventory breakdown across projects |

---

## How to Run

1. Clone this repository into your Databricks Git Folder
2. Set up Azure Data Lake Gen2 with Bronze, Silver, and Gold containers
3. Upload source CSV files to the Bronze landing zones
4. Add your Azure Storage credentials to each notebook
5. Run notebooks in order (1 → 5) or trigger the Databricks Workflow job

>  **Security Note:** Azure Storage keys have been removed from all notebooks 
> before committing. Add your own credentials locally before running.

---

##  Security

- All Azure Storage keys are removed before every Git commit
- Credentials are stored as placeholder strings (`REMOVED_FOR_SECURITY`)
- For production use, secrets should be managed via **Azure Key Vault** or 
  **Databricks Secret Scopes**

---

##  Data Sources

| Dataset | Source | Format |
|---------|--------|--------|
| DLD Transactions | Dubai Pulse (dubaipulse.gov.ae) | CSV |
| DLD Units | Dubai Pulse (dubaipulse.gov.ae) | CSV |
| DLD Projects | Dubai Pulse (dubaipulse.gov.ae) | CSV |

---

##  Tech Stack

| Tool | Purpose |
|------|---------|
| Azure Databricks | Compute & orchestration |
| Apache Spark (PySpark) | Distributed data processing |
| Delta Lake | Storage format with ACID support |
| Azure Data Lake Gen2 | Cloud storage |
| GitHub | Version control |
| Databricks Workflows | Pipeline scheduling |

---

## 👤Author

**Sameer Ahmed**  
Data Engineering Project — Dubai Real Estate Lakehouse  
Built on Azure Databricks | Medallion Architecture
