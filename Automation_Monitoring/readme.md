# Automation & Monitoring

This folder contains configuration and documentation for scheduling your ADF pipelines and monitoring their execution in both Data Factory and Synapse.

## What’s Inside

- **Triggers**
  - `TR_Ingest_Daily.json` – Schedule trigger for `PL_Ingest_AdventureWorks`, set to run daily at 2 AM UTC.

- **Monitoring Dashboards**
  - `ADF_Monitoring_Guide.md` – Instructions and sample queries for using ADF’s built‑in monitoring.  
  - `Synapse_Monitoring_Guide.md` – Tips for leveraging Synapse Studio’s Monitoring pane and Log Analytics.

## Automation

1. **Deploy Trigger**  
   - Import `TR_Ingest_Daily.json` into ADF (Manage → Triggers → New).  
   - Validate that it’s attached to `PL_Ingest_AdventureWorks` with the correct schedule.  

2. **CI/CD Integration (Optional)**  
   - Use GitHub Actions to auto‑deploy the trigger whenever you merge to `main`.  
   - See sample workflow in `.github/workflows/deploy-adf.yml`.

## Monitoring

1. **ADF Monitoring**  
   - Navigate to the “Monitor” tab in ADF Studio.  
   - View pipeline run history, duration, input/output counts, and any activity‑level failures.  

2. **Synapse Monitoring**  
   - In Synapse Studio, open the “Monitor” pane.  
   - For your dedicated SQL pool, review SQL requests, data warehouse utilization, and errors.  
   - (If configured) query your Log Analytics workspace for detailed pipeline and SQL logs.

## How to Use

1. **Check Trigger Status**  
   - In ADF Studio → Manage → Triggers, ensure `TR_Ingest_Daily` is **Enabled**.  

2. **Review Pipeline Runs**  
   - ADF Studio → Monitor → Pipeline Runs.  
   - Filter by date or pipeline name to drill into specific executions.

3. **Review SQL Pool Metrics**  
   - Synapse Studio → Monitor → SQL Requests or DWU utilization charts.  

4. **Set Alerts (Optional)**  
   - In Azure Monitor, create alerts based on metrics (e.g., failed pipeline runs, DWU CPU% > 80%).  
   - Configure email or Teams notifications.

