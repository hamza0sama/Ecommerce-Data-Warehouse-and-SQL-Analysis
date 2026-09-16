# E-commerce Data Warehouse & SQL Analysis

An end-to-end data warehouse project built on **SQL Server**, covering the full pipeline from raw CSV ingestion to dimensional modeling and advanced SQL analysis — implemented as a **Staging → Bronze → Silver → Gold** architecture.

## Overview

This project transforms raw e-commerce transaction data (Central Superstore dataset) into a clean, query-ready data warehouse. The database, **`Mini_Project_02`**, follows a layered ETL design where each stage progressively cleans, validates, and structures the data, culminating in a **Star Schema** used for business-oriented SQL analysis.

## Architecture

```
Raw CSV Data
     │
     ▼
  Staging  →  raw ingestion via BULK INSERT (all columns as NVARCHAR)
     │
     ▼
   Bronze  →  raw data preserved with an identity tracking column
     │
     ▼
   Silver  →  cleaned, validated, and type-cast data
     │
     ▼
    Gold   →  Star Schema (4 dimensions + 1 fact table)
     │
     ▼
SQL Analysis
```

## Database Structure

```
Mini_Project_02
├── staging
├── bronze
├── silver
└── gold
```

Schemas are created automatically if they do not already exist.

## ETL Pipeline

### 1. Staging Layer
Raw CSV data is loaded into `staging.raw_encounters` using `BULK INSERT`. All columns are initially stored as `NVARCHAR` to preserve the source data before any transformation.

### 2. Bronze Layer
Data flows into `bronze.encounters`, with an added `bronze_id` identity column for record tracking. Records are inserted from staging using `EXCEPT` to prevent duplicate loads.

### 3. Silver Layer
`silver.encounters` holds the cleaned and validated dataset. This layer applies:

**Type conversion**
| Column | Target Type |
|---|---|
| Row ID | `INT` |
| Order Date / Ship Date | `DATETIME2` |
| Sales / Profit | `DECIMAL(18,2)` |
| Quantity | `INT` |
| Discount | `DECIMAL(5,2)` |

**Cleaning steps**
- Trimming extraneous whitespace
- Converting empty strings to `NULL`
- Type validation via `TRY_CAST`
- Duplicate detection using `ROW_NUMBER()`
- Missing and invalid value detection
- Sales outlier detection using the IQR method

**Data quality flags** stored per record: `has_missing_value`, `has_invalid_value`, `has_outlier_value`

**Validation rules** — a record is flagged invalid when:
- `Sales < 0`
- `Quantity <= 0`
- `Discount < 0` or `Discount > 1`
- `Ship Date < Order Date`

**Outlier detection** uses the standard IQR bounds:
```
Lower Bound = Q1 − 1.5 × IQR
Upper Bound = Q3 + 1.5 × IQR
```
Outliers are flagged rather than removed, preserving data quality information for downstream analysis.

### 4. Gold Layer — Star Schema
Cleaned Silver data is modeled into dimension and fact tables.

## Star Schema

| Table | Key | Description |
|---|---|---|
| `gold.dimCustomer` | `CustomerKey` (surrogate) | Customer ID (unique), Customer Name, Segment |
| `gold.dimDate` | `DateKey` (`YYYYMMDD`) | Full date, day/month/quarter/year numbers, day name, weekend flag |
| `gold.dimProduct` | `ProductKey` (surrogate) | Product ID (unique), Category, Sub-Category, Product Name |
| `gold.dimLocation` | `LocationKey` | Country, City, State, Postal Code, Region |
| `gold.FactSales` | Row ID + 4 foreign keys | Ship Mode, Sales, Quantity, Discount, Profit |

`FactSales` connects to all four dimensions via foreign keys, with `CustomerKey`, `DateKey`, `ProductKey`, and `LocationKey` as surrogate keys.

## Gold Layer Row Counts

| Table | Rows |
|---|---:|
| dimCustomer | 629 |
| dimDate | 720 |
| dimLocation | 195 |
| dimProduct | 1,310 |
| FactSales | 2,323 |

*(Produced by the validation queries included in the SQL scripts.)*

## SQL Business Analysis

Ten analytical questions were answered using the Gold layer:

1. Which customer segments have the highest average sales per transaction?
2. Which categories have the highest average quantity sold?
3. Which products have the highest total quantity sold?
4. Which states generated sales above 50,000 but profit below 5,000?
5. Which products generated more total sales than the average across all products?
6. Which categories have total profit above the average across all categories?
7. What are the top 3 products by total sales within each category?
8. For each state (ordered by total sales), what is the difference from the previous state's total sales?
9. Which products have the highest profit margin within each category?
10. Which customer segments have total profit above the average across all segments?

**Techniques used:** `SELECT`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`, `JOIN` / `LEFT JOIN` / `INNER JOIN`, subqueries, CTEs, `ROW_NUMBER()`, `LAG()`, aggregate functions (`SUM`, `AVG`), `PERCENTILE_CONT()`, ranking, `CASE` logic, `TRY_CAST()`, `EXCEPT`.

## Technologies

SQL Server · T-SQL · SSMS · CSV · Data Warehousing · ETL/ELT · Star Schema · Dimensional Modeling · Data Quality Validation · Window Functions · CTEs

## Project Structure

```
Mini_Project_02/
├── data/
│   └── Central_Superstore.csv
├── scripts/
│   ├── staging/
│   ├── bronze/
│   ├── silver/
│   ├── gold/
│   └── analysis/
└── README.md
```

## Key Learning Outcomes

- Designing a multi-layer SQL Server data warehouse (Staging → Bronze → Silver → Gold)
- Loading raw CSV data with `BULK INSERT`
- Cleaning and validating real-world data: duplicates, missing/invalid values, IQR-based outlier detection
- Designing a Star Schema with surrogate keys and fact-dimension relationships
- Writing advanced analytical SQL using CTEs, window functions, and ranking logic
- Answering business-oriented questions on sales, profit, customers, and products

## Author

**Hamza Osama**
Data Analyst | Python & SQL | Power BI
[GitHub](https://github.com/hamza0sama)
