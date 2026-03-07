# Financial & Geopolitical Impact Data Pipeline

## Project Overview
This project is an **end-to-end ETL pipeline** designed to analyze the relationship between **Geopolitical Risk Indices (GPRD)** and the **price performance of Precious Metals (Gold & Silver)**.

The system processes historical market data and geopolitical risk indicators, transforms them using **Informatica PowerCenter**, and loads the results into a **Star Schema Data Warehouse** optimized for analytical reporting.

The pipeline enables analysts to evaluate how **political tensions, military threats, and global instability influence asset volatility and safe-haven investment behavior**.

---

## Table of Contents
1. [Project Structure](#-Project-Structure)
2. [ETL Process](#-ETL-Process)
3. [Star Schema Design](#-Star-Schema-Design) 
4. [Technical Challenges](#-Technical-Challenges)
5. [Use Cases](#-use-cases)
 
---

## Project Structure
The repository is organized into directories that represent the main stages of the data pipeline:

```text
├── etl/                   # Informatica mappings and ETL workflow logic
├── design_star_schema/    # Data modeling diagrams and SQL schema definitions
└── dashboard/             # BI dashboards and analytical reports
```

---

## ETL Process

Folder: **etl/**

The ETL pipeline is implemented using **Informatica PowerCenter**.  
Raw data from financial market datasets and geopolitical risk indices is ingested, transformed, and loaded into the Data Warehouse.

### Key Mappings & Logic

### 1. Date Dimension Load
Source: **Gold_Spot_Price_Daily**

Logic: Extracts unique dates from price history to build a complete calendar dimension.

Transformations:

- **Expression Transformation (EXPTRANS)**  
  Extracts `Year`, `Month`, `Day`, `Quarter`, and `Half_Year`.

- **Sequence Generator (SEQTRANS)**  
  Generates the surrogate primary key **DATE_ID**.

Target: **DIM_DATE**

---

### 2. Geopolitical Risk Dimension Load
Source: **Geopolitical_Risk_Index**

Logic: Standardizes and categorizes geopolitical risks.

Transformations:

- **Sorter (SRTTRANS)**  
  Sorts data by risk level and source.

- **Expression Transformation**  
  Classifies risks based on:
  - **GPRD** (General Risk Index)
  - **GPRD_ACT** (Actual Acts)
  - **GPRD_THREAT** (Threat Indicators)

Risk categories are classified into:

- **Low Risk**
- **Medium Risk**
- **High Risk**

Target: **DIM_GEOPOLITICALRISK**

---

### 3. Event Dimension Load (Complex Logic)
Source:  
- **Gold_Spot_Price**
- **Silver_Spot_Price**
- **Geopolitical_Risk_Index**

Logic: Identifies market events and evaluates their immediate impact on asset prices.

Transformations:

- **Joiner (JNRTRANS)**  
  Combines Gold and Silver price data with risk indices using the **date key**.

- **Expression Transformation**  
  Calculates:
  - **IMPACT_ON_GOLD**
  - **IMPACT_ON_SILVER**

  based on price changes during high-risk geopolitical periods.

- **Filter Transformation**  
  Ensures only significant events are loaded.

Target: **DIM_EVENT**

---

## Star Schema Design  

Folder: **design_star_schema/**

The Data Warehouse follows a **Star Schema architecture**, optimized for **analytical queries and BI dashboards**.

The model contains a **central fact table** storing daily market metrics, surrounded by descriptive dimension tables.

---

## Fact Table: FACT_ASSET_PRICE

This table contains **daily asset performance metrics**.

Foreign Keys:

- DATE_ID
- EVENT_ID
- RISK_ID
- ASSET_ID

Metrics:

### Gold Metrics
- GOLD_OPEN
- GOLD_CLOSE
- GOLD_HIGH
- GOLD_LOW
- GOLD_CHANGE

### Silver Metrics
- SILVER_OPEN
- SILVER_CLOSE
- SILVER_HIGH
- SILVER_LOW
- SILVER_CHANGE

### Risk Indicators
- GPRD
- GPRD_ACT
- GPRD_THREAT

---

## Dimension Tables
| Table Name | Description | Key Attributes |
|-------------|-------------|----------------|
| **DIM_DATE** | Calendar attributes for time-series analysis | Year, Month, Quarter, Half_Year |
| **DIM_GEOPOLITICALRISK** | Categorization of geopolitical risks | Risk_Type, Risk_Source |
| **DIM_EVENT** | Market events and their calculated impact | Event_Type, Impact_on_Gold, Impact_on_Silver |
| **DIM_ASSET** | Lookup table for asset types | Asset_Type |
---

## Technical Challenges 

### 1. Date Format Standardization
Problem:  
Source datasets contained inconsistent **date formats**, which Informatica could not parse as Date/Time.

Solution:  
Dates were first imported as **VARCHAR**, then standardized using an **Expression Transformation** before conversion to **Date format**.

---

### 2. Decimal Precision Issues
Problem:  
Informatica skipped rows due to **incorrect decimal delimiters** in financial datasets.

Solution:  
Fields were initially read as **strings**, then converted using **TO_DECIMAL()** to ensure accurate numeric formatting.

---

### 3. Joiner Transformation Cache Limitation
Problem:  
Join operations produced incomplete results due to insufficient **Integration Service cache memory**.

Solution:  
The **Index Cache** and **Data Cache** sizes were increased within **Workflow Manager**.

---

## Use Cases  

### Market Sentiment Analysis
Analyze how **high geopolitical risk levels** correlate with **Gold price spikes**, demonstrating safe-haven asset behavior.

### Volatility Forecasting
Compare the influence of **Military vs Political risks** on **Silver price fluctuations**.

### Historical Benchmarking
Evaluate asset performance across **Half-Year periods** during global conflicts and geopolitical crises.

---

# Tools Used

**ETL Tool**

- Informatica PowerCenter  
  - Mapping Designer  
  - Workflow Manager  

**Database**

- Microsoft SQL Server (Data Warehouse)

**Transformations**

- Source Qualifier
- Expression
- Joiner
- Union
- Lookup
- Sorter
- Router
- Sequence Generator
