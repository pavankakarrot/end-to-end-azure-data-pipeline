

# End-to-End Azure Data Engineering Project

This repository demonstrates an end-to-end data engineering pipeline on Azure using the AdventureWorks dataset. It covers ingestion, transformation, storage, analytics, and monitoring, showcasing best practices and infrastructure-as-code.

## Table of Contents

1. [Azure Data Factory (ADF)](./adf)  
2. [Azure Databricks](./databricks)  
3. [Azure Synapse Analytics](./synapse)  
4. [Power BI Reporting](./powerbi)  
5. [Automation & Monitoring](./automation_monitoring)  
6. [Security & Governance](./security_governance)  
7. [Testing & Validation](./testing_validation)  

---

## 1. Azure Data Factory (ADF)
- **Purpose**: Dynamically ingest 10 CSV files from GitHub into Bronze layer on ADLS Gen2.  
- **Key Artifacts**: Linked Services, Datasets, Pipelines with Lookup, ForEach, Copy Data, and daily trigger.

## 2. Azure Databricks
- **Purpose**: Transform raw Bronze CSVs into cleaned Parquet files in the Silver layer.  
- **Key Artifacts**: Python notebook (`silver_layer.ipynb`), Service Principal authentication, DBFS mounts.

## 3. Azure Synapse Analytics
- **Purpose**: Load Silver Parquet data into a dedicated SQL pool (Gold layer) and create views with column-level security.  
- **Key Artifacts**: Provisioning scripts, `Create_Views_Gold.sql`, column-level security policies.

## 4. Power BI Reporting
- **Purpose**: Build interactive dashboards on Gold-layer views for business insights.  
- **Key Artifacts**: `AdventureWorks_Dashboard.pbix`, DirectQuery to Synapse, DAX measures, multiple report pages.

## 5. Automation & Monitoring
- **Purpose**: Schedule nightly runs and monitor pipeline and SQL pool health.  
- **Key Artifacts**: ADF trigger JSON, monitoring guides for ADF and Synapse, optional GitHub Actions workflow.

## 6. Security & Governance
- **Purpose**: Enforce RBAC via Azure AD and manage secrets securely with Key Vault.  
- **Key Artifacts**: `rbac_setup.ps1`, Bicep template for Key Vault, secrets management documentation.

## 7. Testing & Validation
- **Purpose**: End-to-end tests to verify data flow from on-prem SQL to Power BI.  
- **Key Artifacts**: `insert_test_records.sql`, validation checklist, manual and automated verification steps.

---

*For detailed instructions and assets, navigate to each folder above.*

*Designed to showcase comprehensive Azure data engineering skills and best practices.*  
