# Power BI Dashboard – AdventureWorks Analytics

This folder contains the Power BI Desktop file and documentation for connecting to our Gold‑layer views in Azure Synapse Analytics and building interactive dashboards based on business requirements.

## What’s Inside

- **PBIX File**
  - `AdventureWorks_Dashboard.pbix` – Power BI Desktop report containing:
    1. **Data Source** – DirectQuery connection to Synapse dedicated SQL pool  
    2. **Data Model** – Star schema views (`gold.sales`, `gold.customers`, `gold.products`, etc.)  
    3. **Measures & Calculations** – DAX measures for revenue, average order value, year‑over‑year growth, etc.  
    4. **Report Pages** –  
       - **Overview**: KPI cards (Total Sales, YoY Growth, Top 5 Products)  
       - **Sales by Region**: Map visual + bar chart by territory  
       - **Customer Insights**: Table and slicers for customer demographics  
       - **Returns Analysis**: Donut chart for return reasons and trendline  

- **Dataset Settings Export**
  - `DatasetSettings.json` – Export of the dataset connection settings and parameters.

## How It Works

1. **Connection**  
   - The report uses DirectQuery to the Synapse SQL endpoint:  
     ```
     Server=tcp:<your-synapse-workspace>.sql.azuresynapse.net;Initial Catalog=AdventureWorksGoldPool
     ```

2. **Data Model**  
   - Imported as a set of views with relationships defined by foreign keys (e.g., `sales.customerID → customers.customerID`).

3. **Measures**  
   - Example DAX for Total Sales:
     ```DAX
     Total Sales = SUM(gold[sales][SalesAmount])
     ```
   - YoY Growth:
     ```DAX
     YoY Growth % = 
       DIVIDE(
         [Total Sales] - CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Date'[Date])),
         CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Date'[Date]))
       )
     ```

4. **Report Pages**  
   - Navigate between pages using the left‑hand pane. Slicers allow filtering by year, region, and product category.

## How to Deploy

1. **Open** `AdventureWorks_Dashboard.pbix` in Power BI Desktop (version ≥ May 2025).  
2. **Edit Credentials**: In “Data source settings,” configure the Synapse SQL endpoint credentials.  
3. **Publish**: Click **Publish** and select your Power BI workspace.  
4. **Refresh Schedule**: In the Power BI Service, set up a scheduled refresh (e.g., daily at 3 AM UTC) under **Datasets → Settings**.  
5. **Share**: Grant access to users and pin visuals to a new dashboard if desired.

