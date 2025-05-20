# End-to-End Testing & Validation

This folder contains scripts and documentation for validating the full data pipeline from on‑prem SQL Server to Power BI.

## What’s Inside

- **Test Data Script**
  - `insert_test_records.sql` – T‑SQL script to insert new sample rows into on‑prem AdventureWorks tables for each entity (e.g., `Customer`, `Product`, `SalesOrderHeader`).

- **Validation Checklist**
  - `validation_checklist.md` – Step‑by‑step guide to:
    1. Triggering the ADF pipeline  
    2. Verifying data in Bronze, Silver, and Gold layers  
    3. Confirming Power BI dashboard updates  

## How It Works

1. **Insert Sample Data**  
   Run `insert_test_records.sql` on the on‑prem SQL Server. It inserts one new record into each key table with a timestamped flag.

2. **Trigger Pipeline**  
   - **Manual**: In ADF Studio → Author → Pipelines → `PL_Ingest_AdventureWorks` → **Debug/Trigger Now**.  
   - **Scheduled**: Wait for the daily trigger at 2 AM UTC.

3. **Verify Bronze Layer**  
   In ADLS Gen2 `bronze/<table>/` container, check that new CSV files containing your test rows are present (via Azure Storage Explorer or `%fs ls`).

4. **Verify Silver Layer**  
   In Databricks, mount and list `/mnt/silver/<table>/`, then:
   ```python
   spark.read.parquet("/mnt/silver/customers") \
     .filter(col("TestFlag") == "<today’s_timestamp>") \
     .show()
