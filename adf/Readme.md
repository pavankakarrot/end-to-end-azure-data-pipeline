# Azure Data Factory Pipelines

This folder contains our ADF pipelines and related configuration for dynamically ingesting 10 AdventureWorks CSV files from GitHub into the Bronze layer of our ADLS Gen2.

## What’s Inside

- **Linked Services**
  - `LS_HTTP_GitHub.json` – HTTP connection to GitHub REST API  
  - `LS_ADLS_Raw.json` – Azure Data Lake Storage Gen2 connection for Bronze files  

- **Datasets**
  - `DS_JSON_Config.json` – JSON file listing filenames and target folders (parameters)  
  - `DS_GitHub_CSV.json` – Dataset pointing to the HTTP-linked service  
  - `DS_ADLS_CSV.json` – Dataset pointing to the ADLS-linked service  

- **Pipelines**
  - `PL_Ingest_AdventureWorks.json` – Main pipeline with:
    1. **Lookup** activity to read `DS_JSON_Config.json`
    2. **ForEach** activity to iterate over each file entry
    3. **Copy Data** activity to fetch from GitHub and land into Bronze container, using dynamic parameters  

- **Parameters**
  - `param_pipeline.json` – Defines pipeline parameters (e.g., `fileName`, `folderPath`)  

- **Triggers**
  - `TR_Ingest_Daily.json` – Schedule trigger to run the pipeline daily at 2 AM UTC  

## How It Works

1. **Lookup** reads our configuration JSON from GitHub, which defines the list of CSV filenames and target ADLS paths.  
2. **ForEach** iterates through each entry in the JSON array.  
3. Within the loop, **Copy Data** uses:
   - `@item().fileName` as the source path on GitHub  
   - `@item().folderPath` as the sink path in ADLS (e.g., `bronze/<tablename>/`)  
4. All connections are secured via managed identity and Key Vault–backed credentials.

## How to Deploy

1. Create an Azure Resource Group and grant ADF access to your Key Vault.  
2. In Data Factory > ARM templates, deploy the contents of this folder.  
3. Validate that Linked Services are pointing to the correct GitHub repo URL and storage account.  
4. Enable the daily trigger or run the pipeline manually from the ADF UI.

