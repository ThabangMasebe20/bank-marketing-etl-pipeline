# Bank Marketing Target Pipeline (End-to-End ETL)

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Oracle](https://img.shields.io/badge/Oracle-F80000?style=for-the-badge&logo=oracle&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=Jupyter&logoColor=white)

## 📌 Project Overview
This project is an end-to-end Data Engineering pipeline designed to process raw, messy banking data and generate a targeted marketing list for a premium credit card campaign. 
...

## 🎯 Business Goal
The marketing team wants to target existing customers who are financially healthy and actively using their cards. The criteria for the premium card are:
*   Credit Score > 500
*   Total Deposits (Cash Inflow) > R15,000
*   At least 2 transactions in the 'Travel' category
*   Account Age > 2 years

## 🛠️ Tech Stack
*   **Python (Pandas):** Data profiling, cleaning, and formatting.
*   **Oracle SQL:** Database creation, Medallion Architecture, aggregations, window functions, and final reporting.

## 🏗️ Pipeline Architecture

### 1. Python Data Cleaning (`/notebooks/pipeline.ipynb`)
*   Loaded 4 raw CSVs (Demographics, Accounts, Credit Scores, Transactions).
*   Handled mixed date formats using `pd.to_datetime(..., format='mixed')`.
*   Cleaned the `AMOUNT` column by stripping 'R' and ',' using Regex, converting it to a numeric format.
*   Handled massive null counts (e.g., 202 nulls in `MERCHANT_CATEGORY`).
*   Exported the cleaned data to `/Updated_files` for database ingestion.

### 2. SQL Data Warehousing (`/sql_scripts/bank_pipeline.sql`)
*   **Bronze Layer:** Loaded the cleaned CSVs into raw Oracle tables.
*   **Silver Layer:** Created Views to standardize text to uppercase, remove duplicates, and format dates.
*   **Gold Layer:** Built a `CTAS` (Create Table As Select) query using subqueries to aggregate total deposits and travel counts. Calculated account age using `MONTHS_BETWEEN`.
*   **Serving Layer:** Created the final `FINAL_MARKETING_TARGETS` table. Used a `CASE` statement to standardize messy province abbreviations (e.g., 'WC', 'W CAPE' -> 'WESTERN CAPE') and `DENSE_RANK()` to rank high-value customers by province.

## 📊 Key Insights & Logic
*   **Null Handling:** Used `NVL()` in the Gold layer to replace null names with 'UNKNOWN', ensuring clean reporting.
*   **Data Integrity:** Decided to leave missing Credit Scores as `NULL` so they automatically fail the `> 500` filter, ensuring we don't accidentally target customers with unverified credit.

## 🚀 How to Run
1. Clone this repository.
2. Open and run `pipeline.ipynb` to clean the raw CSVs and generate the formatted files.
3. Open Oracle SQL Developer (or FreeSQL) and run `bank_pipeline.sql` to create the tables, clean the data, and generate the final marketing list.