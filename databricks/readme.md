# Azure Databricks Transformations

This folder contains our Databricks notebooks and supporting configuration for transforming raw Bronze data into Silver Parquet files in ADLS Gen2.

## What’s Inside

- **Notebook**
  - `silver_layer.ipynb` – Python notebook that:
    1. **Authenticates** using Azure Service Principal (via App Registration)  
    2. **Mounts** the Bronze ADLS Gen2 container  
    3. **Loads** multiple CSV tables (10+ AdventureWorks files) into Spark DataFrames  
    4. **Applies Transformations** per table:
       - Customers, Products, SubCategories, Returns, Territories, Sales, etc.  
    5. **Writes** transformed DataFrames to the Silver container in Parquet format  

- **Cluster Configuration**
  - Runtime: 15.3 LTS (Scala & Spark)  
  - Node type: 4‑core, 14 GB (DS3_v2 equivalent)  
  - Libraries: None (all built‑in)  
  - Init Script: (none)  
  - Authentication: Service Principal with `Storage Blob Data Contributor` role  

## Key Steps

1. **Service Principal Setup**  
   - Created an Azure AD App Registration  
   - Granted the SP “Storage Blob Data Contributor” on the target storage account  
   - Saved Tenant ID, Client ID, and Client Secret as Databricks Secrets  

2. **Mounting ADLS Gen2**  
   ```python
   configs = {
     "fs.azure.account.auth.type": "OAuth",
     "fs.azure.account.oauth.provider.type":
       "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
     "fs.azure.account.oauth2.client.id": dbutils.secrets.get(scope, "client-id"),
     "fs.azure.account.oauth2.client.secret": dbutils.secrets.get(scope, "client-secret"),
     "fs.azure.account.oauth2.client.endpoint": f"https://login.microsoftonline.com/{dbutils.secrets.get(scope, 'tenant-id')}/oauth2/token"
   }
   dbutils.fs.mount(
     source = f"abfss://bronze@{storage_account_name}.dfs.core.windows.net/",
     mount_point = "/mnt/bronze",
     extra_configs = configs
   )
3. **Data Loading & Transformation**
   - Read CSVs with spark.read.option("header","true").csv(...)
   - Cleanse and apply business rules (e.g., cast data types, drop nulls)
   - Use Spark SQL or DataFrame APIs to join/filter where needed
  
4. **Writing to Silver**
   - df.write.mode("overwrite") \
  .parquet("/mnt/silver/customers")


