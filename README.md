#  Sales Analytics — End-to-End Microsoft Fabric + Power BI Project

> A complete data engineering and analytics pipeline — from raw CSV ingestion through Bronze → Silver → Gold Medallion Architecture in Microsoft Fabric Lakehouse, Star Schema Warehouse, to a 3-page interactive Power BI dashboard — uncovering ₹13.64M in revenue patterns across products, regions, and customers.

---

Brief Summary
=============

An end-to-end sales analytics project built entirely on **Microsoft Fabric** — ingesting raw sales data (1,525 orders), processing through a Medallion Architecture (Bronze → Silver → Gold), building a Star Schema Data Warehouse (`wh_sales`), creating a Semantic Model, and delivering a 3-page interactive Power BI dashboard — answering: **"How is revenue trending, which products and regions drive the most sales, and who are our top customers?"**

---

Overview
========

This project demonstrates a full modern data stack using Microsoft Fabric. Starting from a raw CSV file, data flows through a Fabric Pipeline into a Lakehouse (`lh_sales`), is transformed across Bronze, Silver, and Gold layers using PySpark Notebooks, loaded into a Star Schema Warehouse (`wh_sales`), and finally visualized in a Power BI report (`sales_report`) with 3 focused pages — Revenue, Product & Region, and Top 10 Customers.

---

Problem Statement
=================

Sales teams often lack a unified, real-time view of revenue performance across products, regions, and customers. Raw transactional data sits in CSV files without proper structure for analysis. This project builds a production-grade analytics solution to answer:

**Key Business Questions:**

| # | Question |
|---|----------|
| 1 | What is the total revenue and how is it trending monthly? |
| 2 | Which product categories and regions generate the most revenue and orders? |
| 3 | Who are the Top 10 customers by revenue and order volume? |

**Core Business Goal:**
> *"Build a scalable, automated sales analytics pipeline that gives business stakeholders clear visibility into revenue, product performance, and customer value."*

---

Dataset
-------

| Detail | Info |
|--------|------|
| File | `sales_data.csv` |
| Records | 1,525 orders |
| Features | 8 columns |

**Columns:**

| Column | Data Type | Description |
|--------|-----------|-------------|
| `OrderID` | String | Unique order identifier (e.g., ORD-01314) |
| `OrderDate` | String → Date | Date the order was placed |
| `CustomerName` | String | Name of the customer |
| `Region` | String | Sales region — North, South, East, West |
| `ProductCategory` | String | Product type — Electronics, Furniture, Office Supplies |
| `Revenue` | Float | Revenue generated from the order (INR) |
| `Quantity` | Integer | Number of units sold |
| `Status` | String | Order status — Active / Returned |

---

Architecture
------------

**Medallion Architecture (Microsoft Fabric Lakehouse — `lh_sales`):**

| Layer | Table | Description |
|-------|-------|-------------|
| Bronze | `bronze_sales` | Raw ingested data — no transformations, exact copy of source CSV |
| Silver | `silver_sales` | Cleaned data — nulls handled, data types fixed, Returned orders flagged |
| Gold | `gold_table` | Business-ready aggregated data — revenue_usd added, dim tables created |

**Star Schema (Data Warehouse — `wh_sales`, Schema: `analytics`):**

| Table | Type | Description |
|-------|------|-------------|
| `dim_customer` | Dimension | Customer details |
| `dim_date` | Dimension | Date dimension with month, quarter, year |
| `dim_product` | Dimension | Product category details |
| `dim_region` | Dimension | Region details |
| `fact_sales` | Fact | Central fact table — OrderID, Revenue, Quantity, foreign keys |

---

Tools & Technologies
--------------------

| Tool | Purpose |
|------|---------|
| **Microsoft Fabric** | End-to-end platform — Lakehouse, Warehouse, Pipeline, Notebooks |
| **Fabric Pipeline** | Automated data ingestion & orchestration (7-step pipeline) |
| **PySpark Notebooks** | Data transformation across Bronze → Silver → Gold layers |
| **Fabric Lakehouse (`lh_sales`)** | Delta Lake storage for raw and transformed data |
| **Fabric Warehouse (`wh_sales`)** | Star Schema data warehouse for analytical querying |
| **Semantic Model (`sales_semantic`)** | Power BI semantic layer with relationships & DAX measures |
| **Power BI** | 3-page interactive dashboard (`sales_report`) |
| **DAX** | Revenue KPIs, USD conversion, Avg Revenue per Order, Rankings |

---

Pipeline Architecture
---------------------

**7-Step Automated Fabric Pipeline:**

| Step | Activity | Description |
|------|----------|-------------|
| 1 | `copy_sales_csv` | Copy Activity — ingests raw CSV into Lakehouse `raw` folder |
| 2 | `convert_csv_to_delta` | PySpark Notebook — converts CSV to Delta format → `bronze_sales` |
| 3 | `bronze_to_silver` | PySpark Notebook — cleans data, fixes types → `silver_sales` |
| 4 | `silver_to_gold` | PySpark Notebook — creates Gold layer + Dim tables → `gold_table` |
| 5 | `lh_to_wh` | Copy Activity — loads Gold data into `wh_sales` Fact table |
| 6 | `lh_to_wh_customer / product / region` | Copy Activities — loads 3 Dimension tables into Warehouse |
| 7 | `lh_to_wh_factsales` | Copy Activity — loads `fact_sales` into Warehouse |

---

Methods
-------

| Step | Description |
|------|-------------|
| **1. Data Ingestion** | Raw `sales_data.csv` copied into `lh_sales` Lakehouse `raw` folder via Pipeline Copy Activity |
| **2. Bronze Layer** | CSV converted to Delta Lake format — stored as `bronze_sales` table with no transformation |
| **3. Silver Layer** | Data cleaned using PySpark — null handling, `OrderDate` to proper datetime, `Status` filtering, data type corrections — stored as `silver_sales` |
| **4. Gold Layer** | Business transformations — `revenue_usd` column added (INR → USD conversion), dimension tables (`dim_customer`, `dim_product`, `dim_region`) created — stored as `gold_table` |
| **5. Warehouse Load** | Gold layer tables copied into `wh_sales` Warehouse under `analytics` schema — Star Schema structure with `fact_sales` + 4 dimension tables |
| **6. Semantic Model** | Built `sales_semantic` model on top of `wh_sales` — defined relationships, hierarchies, and DAX measures |
| **7. Power BI Report** | Connected `sales_report` to `sales_semantic` — built 3-page interactive dashboard with Date, Month, Quarter, Year slicers |

---

Key Insights
------------

**Overall KPIs:**

| Metric | Value |
|--------|-------|
| **Total Revenue (INR)** | ₹ 13.64M |
| **Total Revenue (USD)** | $ 164.38K |
| **Total Orders** | 1,250 |
| **Total Quantity Sold** | 10,043 |
| **Avg Revenue per Order** | ₹ 10,915 |
| **Avg Quantity per Order** | 8 |

**Revenue & Monthly Trends:**

| # | Insight |
|---|---------|
| 1 | **November is the highest revenue month (₹1.39M)** — peak selling season |
| 2 | **September is the lowest revenue month (₹0.89M)** — potential focus area for promotions |
| 3 | Monthly revenue ranges between ₹0.89M–₹1.39M showing consistent business with seasonal peaks |

**Product & Region:**

| # | Insight |
|---|---------|
| 4 | **Electronics dominates revenue (₹9.0M)** — 66% of total revenue |
| 5 | **Furniture contributes ₹3.8M** — strong second category |
| 6 | **Office Supplies revenue is ₹0.8M** — lowest revenue despite highest order count (451 orders) |
| 7 | **Office Supplies has the highest order count (451)** but lowest revenue — low avg order value |
| 8 | **North and East regions lead revenue (₹3.6M each)** — tied for top performing regions |
| 9 | **South region trails at ₹2.9M** — potential growth opportunity |
| 10 | **North leads in orders (325)** followed by East and South (310 each) |

**Top 10 Customers:**

| Rank | Customer | Revenue | Orders |
|------|----------|---------|--------|
| 1 | Vivek Chopra | ₹5,32,815.94 | 39 |
| 2 | Sneha Gupta | ₹5,31,605.33 | 44 |
| 3 | Geeta Pandey | ₹5,26,082.91 | 39 |
| 4 | Manish Srivastava | ₹5,23,652.17 | 27 |
| 5 | Ajay Rastogi | ₹4,48,931.57 | 37 |

---

Dashboard / Output
------------------

**The Power BI dashboard (`sales_report`) is organized into 3 pages with Date, Month, Quarter & Year slicers on every page:**

### Page 1 — Revenue
![Dashboard Page 1](image/page1.png)

| Section | Details |
|---------|---------|
| KPI Cards | Total Revenue (₹13.64M) · Total Revenue USD ($164.38K) · Total Orders (1,250) · Total Quantity Sold (10,043) · Avg Revenue per Order (₹10,915) · Avg Quantity per Order (8) |
| Monthly Revenue & Orders Trend | Combo chart — Bar (Total Revenue) + Line (Total Orders) across Jan–Dec |
| Revenue by Product | Bar chart — Electronics (₹9.0M) · Furniture (₹3.8M) · Office Supplies (₹0.8M) |

---

### Page 2 — Product and Region
![Dashboard Page 2](image/page2.png)

| Section | Details |
|---------|---------|
| Orders by Product Category | Treemap — Office Supplies (451) · Electronics (401) · Furniture (398) |
| Quantity Sold by Product Category | Treemap — Office Supplies (7,028) · Furniture (1,753) · Electronics (1,262) |
| Revenue by Region | Bar chart — North (₹3.6M) · East (₹3.6M) · West (₹3.5M) · South (₹2.9M) |
| Orders by Region | Bar chart — North (325) · East (310) · South (310) · West (305) |

---

### Page 3 — Top 10 Customers
![Dashboard Page 3](image/page3.png)

| Section | Details |
|---------|---------|
| Top 10 Customers by Revenue | Ranked table — Customer Name · Total Revenue · Total Orders (Vivek Chopra leads at ₹5.32L with 39 orders) |

---

How to Run This Project
-----------------------

**Microsoft Fabric Setup**

| Step | Action |
|------|--------|
| 1 | Create a **Microsoft Fabric workspace** |
| 2 | Create a **Lakehouse** named `lh_sales` |
| 3 | Upload `sales_data.csv` to the Lakehouse `raw` folder |
| 4 | Create & run **3 PySpark Notebooks**: `convert_csv_to_delta` → `bronze_to_silver` → `silver_to_gold` |
| 5 | Create a **Fabric Warehouse** named `wh_sales` with `analytics` schema |
| 6 | Create & run the **7-step Pipeline** to load Gold tables into Warehouse |
| 7 | Build **Semantic Model** (`sales_semantic`) on top of `wh_sales` |
| 8 | Connect **Power BI report** (`sales_report`) to `sales_semantic` |

**Power BI Dashboard**

| Step | Action |
|------|--------|
| 1 | Open `sales_report.pbix` in Power BI Desktop |
| 2 | Refresh data connection to `sales_semantic` |
| 3 | Navigate between Revenue → Product and Region → Top 10 Customers |

>  **Power BI file — Download here:** [Google Drive Link](#) *(update with your link)*

---

Results & Conclusion
====================

This project successfully delivers a **production-grade sales analytics solution** on Microsoft Fabric. The Medallion Architecture ensures data quality at every layer, while the Star Schema Warehouse enables fast analytical queries. Key findings: **Electronics drives 66% of total revenue (₹9M)**, **November is the peak revenue month (₹1.39M)**, **North and East regions are equally strong at ₹3.6M each**, and the **Top 10 customers collectively account for a significant share of total revenue**. The automated pipeline ensures data can be refreshed with a single click — making this a scalable, production-ready solution.

---

Author & Contact
----------------

| Field | Info |
|-------|------|
| **Name** | *KRISHNA* |
| **LinkedIn** | *https://www.linkedin.com/in/krishna-prajapati-26a106231/* |
| **GitHub** | *https://github.com/* |

---

⭐ *If you found this project helpful, consider giving it a star!*
