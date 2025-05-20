# Azure Synapse Analytics – Gold Layer

This folder contains scripts and configuration for provisioning a Synapse SQL pool, loading the Gold‑layer Parquet data, creating views, and enforcing column‑level security.

## What’s Inside

- **Infrastructure**
  - `synapse_provision.sql` – T-SQL script to create a dedicated SQL pool and configure firewall rules.

- **Data Loading & Views**
  - `Create_Views_Gold.sql` – SQL script defining views over the Silver Parquet files using `OPENROWSET`.  
    - Views: `gold.calendar`, `gold.customers`, `gold.products`, `gold.returns`, `gold.sales`, `gold.subcat`, `gold.territories`.

- **Security**
  - `column_level_security.sql` – T-SQL script to:
    1. Create database roles (e.g., `role_sales_analyst`, `role_marketing_analyst`)  
    2. Grant `SELECT` on only non‑sensitive columns  
    3. Deny or mask sensitive columns (e.g., `customer_email`, `customer_phone`)  

## How It Works

1. **Provision**  
   Run `synapse_provision.sql` in your Synapse workspace to:
   - Create a dedicated SQL pool named `AdventureWorksGoldPool`.  
   - Open firewall to your IP range.  

2. **Create Views**  
   Execute `Create_Views_Gold.sql` in the SQL pool to define views over the Silver data stored in ADLS Gen2.

   ![Alt text](https://github.com/pavankakarrot/end-to-end-azure-data-pipeline/blob/main/Screenshots/Synapse.png)

4. **Apply Column‑Level Security**  
   Run `column_level_security.sql` to create roles, assign permissions, and implement masking policies.

5. **Querying**  
   Users connect via Synapse Studio or external tools (e.g., Power BI) to query these views. Role memberships control which columns each user sees.

## How to Deploy

1. **Connect** to your Synapse workspace in Synapse Studio.  
2. **Open** the “Data” blade and select your dedicated SQL pool.  
3. **New SQL Script** → paste and run `synapse_provision.sql`.  
4. **New SQL Script** → paste and run `Create_Views_Gold.sql`.  
5. **New SQL Script** → paste and run `column_level_security.sql`.  
6. **Validate** by opening each view in the Data pane and running `SELECT TOP 10 * FROM gold.<view_name>`.  
7. **Test** with different users/role memberships to ensure sensitive columns are masked or hidden.

