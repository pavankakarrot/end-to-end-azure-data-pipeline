# Security & Governance

This folder contains scripts and documentation for securing the Azure environment with Role‑Based Access Control (RBAC) and managing secrets using Azure Key Vault.

## What’s Inside

- **RBAC Setup**
  - `rbac_setup.ps1` – PowerShell script to:
    1. Create Azure AD groups (e.g., `adf-operators`, `databricks-engineers`, `synapse-analysts`)  
    2. Assign role definitions (e.g., Contributor, Data Factory Operator, Storage Blob Data Reader) scoped to each resource group or individual service  

- **Key Vault Configuration**
  - `keyvault_deploy.bicep` – Bicep template to provision a Key Vault with:
    - Soft-delete and purge protection enabled  
    - Access policies granting only required service principals and user groups  

- **Secrets Management**
  - `secrets_management.md` – Guide to:
    1. Add secrets (storage account keys, service principal client secrets)  
    2. Configure Databricks Secret Scopes from Key Vault  
    3. Reference secrets in ADF and Synapse linked services  

## How It Works

1. **RBAC**
   - Run `rbac_setup.ps1` (after logging in with Az CLI) to:
     ```powershell
     Connect-AzAccount
     .\rbac_setup.ps1 -ResourceGroupName "rg-adventureworks" -Location "WestEurope"
     ```
   - This script creates AD groups, service principals, and assigns built‑in roles at the appropriate scopes.

2. **Key Vault Deployment**
   - Deploy the Bicep template:
     ```bash
     az deployment group create \
       --resource-group rg-adventureworks \
       --template-file keyvault_deploy.bicep \
       --parameters vaultName="kv-adventureworks" tenantId="<your-tenant-id>"
     ```
   - Configures Key Vault with strict access policies.

3. **Secrets Management**
   - **Add Secrets**  
     ```bash
     az keyvault secret set --vault-name kv-adventureworks \
       --name "databricks-client-secret" \
       --value "<your-client-secret>"
     ```
   - **Databricks**  
     In Databricks CLI or UI, create a secret scope backed by Key Vault:  
     ```bash
     databricks secrets create-scope \
       --scope adls-scope \
       --scope-backend-type AZURE_KEYVAULT \
       --resource-id "/subscriptions/…/resourceGroups/rg-adventureworks/providers/Microsoft.KeyVault/vaults/kv-adventureworks" \
       --dns-name "https://kv-adventureworks.vault.azure.net/"
     ```
   - **ADF & Synapse**  
     In Linked Services JSON, reference Key Vault–backed connection strings via `@Microsoft.KeyVault(SecretUri=...)`.

## How to Deploy

1. **RBAC**  
   - Ensure you’re Global Administrator or Privileged Role Administrator in Azure AD.  
   - Run the PowerShell script as shown above.  

2. **Key Vault**  
   - Deploy via Azure CLI or Azure Portal.  
   - Verify `Soft Delete` and `Purge Protection` are **Enabled**.  

3. **Secrets**  
   - Add required secrets for storage and service principals.  
   - Grant each service principal or user group **Get** and **List** permissions on secrets.

4. **Validation**  
   - In the Portal → Key Vault → Access Policies, confirm only intended identities have access.  
   - In Portal → Access Control (IAM) of each resource, verify group and SP assignments.

