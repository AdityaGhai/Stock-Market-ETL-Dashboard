# 📈 Stock Market ETL & Dashboard 

This project demonstrates a complete **end-to-end data pipeline** for stock market analysis using the Azure Data Engineering stack. It fetches real-time stock prices, processes them using Databricks, stores the results in Azure Data Lake, and visualizes insights in Power BI.

---

## 🔧 Tech Stack

- **Azure Data Factory** – Orchestrates data extraction from APIs
- **Azure Data Lake Gen2 (ADLS)** – Cloud storage for raw, clean, and curated data
- **Azure Databricks** – PySpark-based transformation and data cleaning
- **Power BI** – Interactive dashboard and KPIs
- **Delta Lake Format** – For efficient storage and querying

---

## 🏗️ Architecture

     ┌───────────────┐
     │    Power BI   │ ◄──────────────┐
     │   Dashboard   │                │
     └──────┬────────┘                │
            │                         │
            ▼                         │
     ┌───────────────┐       ┌────────▼────────┐
     │     Gold      │◄──────┤  Silver Layer   │
     │ (Fact & Dim)  │       │ Cleaned Parquet │
     └──────┬────────┘       └────────┬────────┘
            │                         │
            ▼                         ▼
     ┌───────────────┐        ┌───────────────┐
     │   Databricks  │ ◄─────▶│    Bronze      │
     │ (Transform)   │        │ (Raw CSV)      │
     └──────┬────────┘        └────────┬────────┘
            │                         ▲
            ▼                         │
     ┌───────────────┐        ┌───────┴────────┐
     │ Azure Data     │──────▶│   ADLS Gen2    │
     │   Factory      │        │ (Storage Layer)│
     └───────────────┘        └────────────────┘


---


## 📂 Project Structure

📁 data/  
   ├── bronze/ — Raw CSVs  
   ├── silver/ — Cleaned parquet  
   └── gold/ — Fact & dimension


---

## 🔄 Pipeline Flow

### 1. Extract (ADF)
- Parameterized ADF pipeline fetches daily stock prices for symbols like `IBM`, `AMZN`, `GOOGL`, `ORCL`, `ADBE`.
- Saves CSVs to `bronze/raw/`.

### 2. Transform (Databricks)
- Cleans nulls, handles schema issues.
- Derives fields like `% Change = (close - open)/open`.
- Writes cleaned data as Parquet to `silver/`.

### 3. Model (Gold Layer)
- Fact Table: `fact_stockprice` (price, volume, date, symbol)
- Dim Table: `dim_company` (name, symbol, sector)

### 4. Visualize (Power BI)
Key visuals built:
- 📉 **Line Chart** – Closing price over time
- 📊 **Bar Chart** – Volume traded by company
- 🧮 **KPI Cards** – Latest stock price
- 📅 **Slicer** – By date or company
- 🧾 **Table** – Open, close, high, low, volume

---

## 📊 Sample Dashboard

![image](https://github.com/user-attachments/assets/bcf25d49-2a5c-406f-817e-02959dac1aae)


---

## ✨ Features

- Modular and scalable ETL architecture
- Parameterized for multiple stock symbols
- Power BI connected to Gold layer using ADLS (File System View)
- Uses Delta Lake for performance

---

## 🚀 How to Run Locally

1. Set up your Azure resources:
   - ADLS Gen2
   - ADF pipeline
   - Databricks workspace

2. Upload the stock symbols to ADF pipeline as parameters.

3. Run Databricks notebooks to create Silver & Gold layers.

4. Open `stock_dashboard.pbix` and connect Power BI to the Gold layer.

---

## 🙌 What I Learned

- Real-world implementation of Medallion Architecture (Bronze → Silver → Gold)
- Using ADF + Databricks for scheduled data ingestion
- Creating dynamic Power BI dashboards
- Handling data lake connection permissions (RBAC, file system view)
- Working with semi-structured Parquet data in Power BI

---


---

