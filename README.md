# E-commerce Data Warehouse & SQL Analysis

An end-to-end **E-commerce Data Warehouse** project built using **SQL Server**, covering the complete data pipeline from raw data ingestion and transformation to dimensional modeling and analytical SQL queries.

## 📌 Project Overview

This project demonstrates how raw e-commerce data can be transformed into a structured **Data Warehouse** using a **Bronze → Silver → Gold** architecture.

The final Gold layer follows a **Star Schema** design, making the data suitable for analytical reporting and business intelligence.

The project also includes a set of SQL-based analytical questions to extract insights related to:

- Sales and Profit
- Products and Categories
- Customers and Segments
- States and Locations
- Product performance
- Sales rankings and comparisons

## 🏗️ Data Warehouse Architecture

```text
Raw CSV Data
     │
     ▼
  Staging
     │
     ▼
   Bronze
     │
     ▼
   Silver
     │
     ▼
    Gold
     │
     ├── dimCustomer
     ├── dimDate
     ├── dimProduct
     ├── dimLocation
     └── FactSales
          │
          ▼
    SQL Analysis
```

## ⭐ Star Schema

The Gold layer is organized into a Star Schema consisting of:

### Fact Table

- `FactSales`
  - Sales
  - Quantity
  - Discount
  - Profit
  - CustomerKey
  - ProductKey
  - DateKey
  - LocationKey

### Dimension Tables

- `dimCustomer`
- `dimDate`
- `dimProduct`
- `dimLocation`

## 🔄 ETL Process

### 1. Staging

Raw CSV data is imported into the staging layer using `BULK INSERT`.

### 2. Bronze Layer

Raw data is stored while handling duplicate records.

### 3. Silver Layer

Data is cleaned and validated through:

- Duplicate removal
- Data type conversion
- Null handling
- Invalid value detection
- Date validation
- Sales outlier detection using the IQR method

### 4. Gold Layer

Cleaned data is transformed into dimension and fact tables following a Star Schema.

## 📊 SQL Analysis

The project includes analytical queries using:

- `GROUP BY`
- `HAVING`
- Subqueries
- CTEs
- Window Functions
- `ROW_NUMBER()`
- `LAG()`
- Aggregations
- Ranking and comparison techniques

Example analytical questions include:

- Which customer segments have the highest average sales?
- Which categories have the highest average quantity?
- Which products have the highest total quantity sold?
- Which states generate high sales but relatively low profit?
- Which products generate above-average total sales?
- Which products have the highest profit margin within each category?
- What are the top 3 products by sales within each category?
- What is the sales difference between consecutive states ordered by total sales?

## 🛠️ Technologies

- **SQL Server**
- **T-SQL**
- **SSMS**
- **CSV**
- **Data Warehousing**
- **Star Schema**
- **ETL / ELT Concepts**
- **Window Functions**

## 📁 Project Structure

```text
Ecommerce-Data-Warehouse-SQL/
│
├── scripts/
│   ├── staging/
│   ├── bronze/
│   ├── silver/
│   ├── gold/
│   └── analysis/
│
├── data/
│
└── README.md
```

## 🎯 Key Learning Outcomes

Through this project, I practiced:

- Designing a SQL Server Data Warehouse
- Building a layered ETL pipeline
- Cleaning and validating real-world data
- Designing a Star Schema
- Creating Fact and Dimension tables
- Writing advanced analytical SQL queries
- Using CTEs and Window Functions
- Performing business-oriented data analysis

## 👤 Author

**Hamza Osama**

Data Analyst | Python & SQL | Power BI

[GitHub](https://github.com/hamza0sama)
