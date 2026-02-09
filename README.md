# Geopolitical Risk & Precious Metals Data Warehouse

## 📖 Project Overview
This project is an end-to-end Business Intelligence solution designed to analyze the correlation between **Geopolitical Risks (GPRD)** and **Precious Metal Prices (Gold & Silver)**.

The system extracts historical market data and risk indices, transforms them using **Informatica PowerCenter**, organizes them into a **Star Schema**, and prepares the data for dashboard visualization to assess market impacts.

## 📑 Table of Contents
1. [Project Structure](#-project-structure)
2. [ETL Process](#-etl-process)
3. [Star Schema Design](#-star-schema-design)
4. [Dashboard](#-dashboard)


---

## Project Structure
The repository is organized into three main directories, reflecting the data flow:

text
├── etl/                   # Informatica mappings and transformation logic
├── design_star_schema/    # Data modeling diagrams and SQL definitions
└── dashboard/             # Visualization files and output reports
## ETL Process

Folder: etl/

The ETL (Extract, Transform, Load) logic is built using Informatica PowerCenter. The workflows ingest raw CSV/Flat files and load them into the Data Warehouse.

Key Mappings & Logic
1. Date Dimension Load
Source: Gold_Spot_Price_Daily

Logic: Extracts distinct dates from the transaction history to build a master calendar.

Transformations:

Expression (EXPTRANS): Parses Year, Month, Day, Quarter, and Half_Year.

Sequence Generator (SEQTRANS): Generates the surrogate primary key Date_Id.

Target: DIM_DATE table.

2. Geopolitical Risk Dimension Load
Source: Geopolitical_Risk_Index

Logic: Standardizes risk categories.

Transformations:

Sorter (SRTTRANS): Sorts data by Risk Category and Source for aggregation.

Expression: Classifies risks based on GPRD (General), GPRD_ACT (Actual Acts), and GPRD_THREAT (Threats).

Target: DIM_GEOPOLITICALRISK table.

3. Event Dimension Load (Complex Logic)
Source: Joins data from Silver_Spot_Price, Gold_Spot_Price, and Geopolitical_Risk_Index.

Logic: Identifies specific market events and calculates their immediate impact on asset prices.

Transformations:

Joiner (JNRTRANS): Combines Gold and Silver data with Risk data based on dates.

Expression: Calculates IMPACT_ON_GOLD and IMPACT_ON_SILVER (derived from price changes during high-risk events).

Filter: Ensures only significant events are loaded.

Target: DIM_EVENT table.

## Star Schema Design

Folder: design_star_schema/

The data model is a classic Star Schema optimized for analytical queries. It centers around a Fact table containing daily market metrics, surrounded by descriptive dimensions.

Fact Table: Tax_Driver_table
This is the central table containing the daily performance metrics.

Foreign Keys: DATE_ID, EVENT_ID, RISK_ID, ASSET_ID

Metrics:

Gold: GOLD_CLOSE, GOLD_OPEN, GOLD_HIGH, GOLD_LOW, GOLD_CHANGE

Silver: SILVER_CLOSE, SILVER_OPEN, SILVER_HIGH, SILVER_LOW, SILVER_CHANGE

Risk Indices: GPRD, GPRD_ACT, GPRD_THREAT
Table Name,Description,Key Attributes
DIM_DATE,Calendar attributes for time-series analysis.,"Year, Month, Quarter, Half_Year"
DIM_GEOPOLITICALRISK,Categorization of risk sources.,"Risk_Type, Risk_Source"
DIM_EVENT,Specific market events and their calculated impact.,"Event_Type, Impact_on_Gold, Impact_on_Silver"
DIM_ASSET,Lookup for asset types.,Asset_Type

## Dashboard
Folder: dashboard/

The dashboard utilizes the Star Schema to visualize trends and correlations.

Key Insights:

Price Correlation: Visualizing how Gold/Silver prices react on days with high Geopolitical Risk (GPRD) scores.

Event Impact: Analyzing specific "Event Types" (from DIM_EVENT) to see if they historically drive prices up (safe-haven behavior) or down.

Volatility Tracking: Monitoring GOLD_CHANGE and SILVER_CHANGE over time.



