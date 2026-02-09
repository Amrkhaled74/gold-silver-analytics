# 🌍 Geopolitical Risk & Precious Metals Data Warehouse

## 📌 Project Overview

This project is a complete **end-to-end Business Intelligence (BI) and Data Warehousing solution** designed to analyze the relationship between **geopolitical risk** and **precious metal market behavior** (Gold & Silver).

The system integrates historical **Geopolitical Risk Data (GPRD)** with **financial market prices**, processes them through a structured **ETL pipeline built in Informatica PowerCenter**, models the data using a **Star Schema**, and prepares it for **analytics dashboards and decision support systems**.

The main objective is to enable data-driven insights into how geopolitical instability influences safe-haven assets such as gold and silver.

---

## 🧠 Business & Analytical Objectives

* Analyze correlation between geopolitical risk and precious metal prices
* Identify **safe-haven behavior** during high-risk periods
* Track volatility patterns in gold and silver markets
* Detect historical market reactions to geopolitical events
* Enable fast analytical queries for BI dashboards

---

## 🗂️ Project Structure

```text
├── etl/                    # Informatica PowerCenter mappings, workflows, transformations
├── design_star_schema/     # Data warehouse schema, ER diagrams, SQL DDL
└── dashboard/              # BI dashboards, visualizations, analytical reports
```

Each layer reflects a standard enterprise data architecture:

* **ETL Layer** → Data ingestion and transformation
* **Warehouse Layer** → Dimensional modeling (Star Schema)
* **Analytics Layer** → BI dashboards and reporting

---

## 🔄 ETL Process (Informatica PowerCenter)

**Folder:** `etl/`

The ETL layer is responsible for extracting raw data from flat files (CSV), transforming it into structured business entities, and loading it into the Data Warehouse.

### 🔹 Data Sources

* `Gold_Spot_Price_Daily`
* `Silver_Spot_Price_Daily`
* `Geopolitical_Risk_Index`

---

### 1️⃣ Date Dimension Load (`DIM_DATE`)

**Source:** `Gold_Spot_Price_Daily`

**Purpose:** Build a centralized time dimension for analytical consistency.

**Transformations:**

* `EXPTRANS (Expression)` → Extracts: Year, Month, Day, Quarter, Half_Year
* `SEQTRANS (Sequence Generator)` → Generates surrogate key `DATE_ID`

**Target:** `DIM_DATE`

---

### 2️⃣ Geopolitical Risk Dimension Load (`DIM_GEOPOLITICALRISK`)

**Source:** `Geopolitical_Risk_Index`

**Purpose:** Standardize and classify geopolitical risk types.

**Transformations:**

* `SRTTRANS (Sorter)` → Sorting by Risk Category & Source
* `EXPTRANS (Expression)` → Risk classification:

  * `GPRD` → General Risk Index
  * `GPRD_ACT` → Actual geopolitical acts
  * `GPRD_THREAT` → Threat-based risks

**Target:** `DIM_GEOPOLITICALRISK`

---

### 3️⃣ Event Dimension Load (`DIM_EVENT`) — *Advanced Logic*

**Sources:**

* `Gold_Spot_Price`
* `Silver_Spot_Price`
* `Geopolitical_Risk_Index`

**Purpose:** Detect meaningful market events and quantify their financial impact.

**Transformations:**

* `JNRTRANS (Joiner)` → Join datasets by Date
* `EXPTRANS (Expression)` → Calculate:

  * `IMPACT_ON_GOLD`
  * `IMPACT_ON_SILVER`
* `FILTER` → Load only **significant geopolitical events**

**Target:** `DIM_EVENT`

---

## ⭐ Star Schema Design

**Folder:** `design_star_schema/`

The warehouse uses a **classic Star Schema** optimized for:

* High-performance analytical queries
* BI tools
* Aggregations
* Time-series analysis

### 🧩 Fact Table

**Table:** `FACT_MARKET`

**Foreign Keys:**

* `DATE_ID`
* `EVENT_ID`
* `RISK_ID`
* `ASSET_ID`

**Measures:**

**Gold Metrics:**

* `GOLD_OPEN`
* `GOLD_CLOSE`
* `GOLD_HIGH`
* `GOLD_LOW`
* `GOLD_CHANGE`

**Silver Metrics:**

* `SILVER_OPEN`
* `SILVER_CLOSE`
* `SILVER_HIGH`
* `SILVER_LOW`
* `SILVER_CHANGE`

**Risk Metrics:**

* `GPRD`
* `GPRD_ACT`
* `GPRD_THREAT`

---

### 🗃️ Dimension Tables

| Table Name             | Description                 | Key Attributes                               |
| ---------------------- | --------------------------- | -------------------------------------------- |
| `DIM_DATE`             | Time intelligence dimension | Year, Month, Quarter, Half_Year              |
| `DIM_GEOPOLITICALRISK` | Risk classification         | Risk_Type, Risk_Source                       |
| `DIM_EVENT`            | Market events & impacts     | Event_Type, Impact_on_Gold, Impact_on_Silver |
| `DIM_ASSET`            | Asset lookup                | Asset_Type                                   |

---

## 📊 Dashboard & Analytics Layer

**Folder:** `dashboard/`

The dashboard layer connects directly to the Star Schema and enables advanced analytics.

### 🔍 Analytical Use Cases

* 📈 **Price Correlation Analysis**
  Relationship between **GPRD levels** and gold/silver price movements

* 🛡️ **Safe-Haven Behavior Detection**
  Identify periods where gold/silver rise during geopolitical instability

* ⚡ **Event Impact Analysis**
  Measure market reaction to different event types

* 🌊 **Volatility Monitoring**
  Track `GOLD_CHANGE` and `SILVER_CHANGE` over time

---

## 🏗️ Architecture Summary

```text
Raw Data Sources
      ↓
ETL (Informatica PowerCenter)
      ↓
Staging Layer
      ↓
Star Schema Data Warehouse
      ↓
BI Dashboards
      ↓
Business Insights & Decision Support
```

---

## 🎯 Project Value

This project demonstrates:

* Enterprise-grade ETL design
* Dimensional modeling expertise
* Business Intelligence architecture
* Analytical thinking
* Financial + geopolitical domain integration
* Data engineering best practices

---

## 🚀 Future Enhancements

* Real-time streaming ingestion (Kafka)
* Predictive modeling (ML risk forecasting)
* Sentiment analysis integration (news & social media)
* API-based live market feeds
* Automated anomaly detection

---

📌 **This repository represents a full BI lifecycle:**
Data Engineering → Data Modeling → Data Warehousing → Analytics → Business Intelligence
