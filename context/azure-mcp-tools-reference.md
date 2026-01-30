# Azure MCP Server Tools Reference

**Complete catalog of Azure MCP Server tools organized by service category.**

This reference is used by the azure-mcp-expert agent to provide authoritative guidance on Azure service selection, tool usage, and deployment patterns.

---

## Azure MCP Server Overview

The Azure Model Context Protocol (MCP) Server exposes Azure services through natural language-accessible tools. All tools support global parameters and follow consistent naming patterns.

### Global Parameters (All Tools)

Every Azure MCP tool accepts these global parameters:

| Parameter | Description | Default |
|-----------|-------------|---------|
| `subscription` | Azure subscription ID or name | Current subscription |
| `resource_group` | Resource group name | N/A (usually required) |
| `tenant_id` | Azure tenant ID for authentication | Default tenant |
| `authentication_method` | Auth method: credential, key, connectionString | credential |
| `maximum_retries` | Max retry attempts for failed operations | 3 |
| `retry_delay` | Initial delay in seconds between retries | 2 |
| `retry_delay_maximum` | Max delay cap in seconds | 10 |
| `retry_mode` | Retry strategy: fixed or exponential | exponential |
| `retry_network_timeout` | Network operation timeout in seconds | 100 |

### Tool Annotations

Tools are marked with these safety annotations:

| Annotation | Meaning | Impact |
|------------|---------|--------|
| **destructive** | Can delete/modify resources | Requires user confirmation |
| **idempotent** | Same inputs = same result | Safe to retry |
| **open_world** | Interacts with external entities | Unpredictable behavior |
| **read_only** | No state changes | Safe for exploration |
| **secret** | Response may contain sensitive data | Requires elicitation (user prompt) |
| **local_required** | Needs local execution | Only available in STDIO mode |

### Authentication Methods

**Recommended hierarchy:**
1. **credential** (preferred) - Managed identity or Azure CLI authentication
2. **key** - Access key (avoid for production)
3. **connectionString** - Connection string (least secure)

---

## Service Namespaces and Tools

### AI and Machine Learning

#### Azure AI Foundry (`foundry`)
Work with Azure AI Foundry models, deployments, and endpoints.

**Common Tools:**
- `azmcp_foundry_model_list` - List available models
- `azmcp_foundry_deployment_create` - Create model deployment
- `azmcp_foundry_endpoint_get` - Get endpoint details

**Use Cases:** AI model deployment, inference endpoints

#### Azure AI Search (`search`)
Manage Azure AI Search services, indexes, and queries.

**Common Tools:**
- `azmcp_search_service_create` - Create search service
- `azmcp_search_index_create` - Create search index
- `azmcp_search_query` - Execute search queries

**Use Cases:** Full-text search, semantic search, vector search

#### Azure AI Speech (`speech`)
Manage Azure AI Speech resources.

**Common Tools:**
- `azmcp_speech_service_create` - Create speech service
- `azmcp_speech_synthesize` - Text-to-speech
- `azmcp_speech_recognize` - Speech-to-text

**Use Cases:** Voice interfaces, transcription, audio generation

---

### Compute

#### Azure App Service (`appservice`)
Manage App Service plans, web apps, and configurations.

**Common Tools:**
- `azmcp_appservice_plan_create` - Create App Service plan
- `azmcp_appservice_webapp_create` - Create web app
- `azmcp_appservice_webapp_config_set` - Configure app settings
- `azmcp_appservice_webapp_deploy` - Deploy application code
- `azmcp_appservice_webapp_identity_assign` - Enable managed identity
- `azmcp_appservice_webapp_delete` ⚠️ - Delete web app (destructive)

**Pricing Tiers:**
- F1 (Free): $0/month, shared compute
- B1 (Basic): ~$13/month, 1 core, 1.75GB RAM
- S1 (Standard): ~$70/month, auto-scale, staging slots
- P1v3 (Premium): ~$150/month, 2 cores, 8GB RAM

**Use Cases:** Web applications, APIs, containers

#### Azure Functions (`functionapp`)
List and manage Azure Functions.

**Common Tools:**
- `azmcp_functionapp_list` - List function apps
- `azmcp_functionapp_get` - Get function app details

**Use Cases:** Serverless functions, event-driven processing

#### Azure Kubernetes Service (`aks`)
Manage AKS clusters.

**Common Tools:**
- `azmcp_aks_cluster_create` - Create AKS cluster
- `azmcp_aks_cluster_list` - List clusters
- `azmcp_aks_cluster_get_credentials` - Get kubeconfig
- `azmcp_aks_cluster_delete` ⚠️ - Delete cluster (destructive)

**Use Cases:** Container orchestration, microservices

---

### Databases

#### Azure Cosmos DB (`cosmos`)
Work with Cosmos DB accounts, databases, containers, documents.

**Common Tools:**
- `azmcp_cosmos_account_create` - Create Cosmos account
- `azmcp_cosmos_database_create` - Create database
- `azmcp_cosmos_container_create` - Create container
- `azmcp_cosmos_document_query` - Query documents
- `azmcp_cosmos_account_delete` ⚠️ - Delete account (destructive)

**Use Cases:** NoSQL database, globally distributed data

#### Azure Database for PostgreSQL (`postgres`)
Manage PostgreSQL servers, databases, tables.

**Common Tools:**
- `azmcp_postgres_server_create` - Create PostgreSQL server
- `azmcp_postgres_database_create` - Create database
- `azmcp_postgres_server_list_connection_strings` 🛡️ - Get connection strings (secret)
- `azmcp_postgres_server_delete` ⚠️ - Delete server (destructive)

**Pricing Tiers:**
- Basic (B_Gen5_1): ~$30/month, 1 vCore, 50GB storage
- General Purpose (GP_Gen5_2): ~$120/month, 2 vCores, 100GB storage

**Use Cases:** Relational database for web apps, APIs

#### Azure Database for MySQL (`mysql`)
Manage MySQL servers, databases, tables.

**Common Tools:**
- `azmcp_mysql_server_create` - Create MySQL server
- `azmcp_mysql_database_create` - Create database
- `azmcp_mysql_server_delete` ⚠️ - Delete server (destructive)

**Use Cases:** MySQL-compatible applications

#### Azure Redis (`redis`)
Create and manage Azure Redis resources.

**Common Tools:**
- `azmcp_redis_cache_create` - Create Redis cache
- `azmcp_redis_cache_get` - Get cache details
- `azmcp_redis_cache_list_keys` 🛡️ - Get access keys (secret)

**Pricing Tiers:**
- Basic C0: ~$15/month, 250MB cache
- Standard C1: ~$75/month, 1GB cache

**Use Cases:** Caching, session storage, message broker

#### Azure SQL (`sql`)
Work with Azure SQL Database servers, databases, firewall rules.

**Common Tools:**
- `azmcp_sql_server_create` - Create SQL server
- `azmcp_sql_database_create` - Create database
- `azmcp_sql_server_firewall_create` - Add firewall rule
- `azmcp_sql_database_delete` ⚠️ - Delete database (destructive)

**Use Cases:** SQL Server workloads, enterprise databases

---

### Storage

#### Azure Storage (`storage`)
List and manage storage accounts, containers, blobs, tables.

**Common Tools:**
- `azmcp_storage_account_create` - Create storage account
- `azmcp_storage_account_enable_static_website` - Enable static website hosting
- `azmcp_storage_blob_upload` - Upload blob
- `azmcp_storage_container_create` - Create container
- `azmcp_storage_account_list_keys` 🛡️ - Get access keys (secret)
- `azmcp_storage_account_delete` ⚠️ - Delete account (destructive)

**Pricing:**
- Standard_LRS: ~$0.02/GB/month
- Total: ~$0.50-5/month for typical usage

**Use Cases:** Static website hosting, file storage, blob storage

#### Azure Container Registry (`acr`)
Manage container registries.

**Common Tools:**
- `azmcp_acr_create` - Create container registry
- `azmcp_acr_build` - Build container image
- `azmcp_acr_list` - List registries

**Pricing:**
- Basic: ~$5/month
- Standard: ~$20/month

**Use Cases:** Container image storage, Docker registry

---

### Security

#### Azure Key Vault (`keyvault`)
Manage keys, secrets, and certificates.

**Common Tools:**
- `azmcp_keyvault_create` - Create Key Vault
- `azmcp_keyvault_secret_set` - Store secret
- `azmcp_keyvault_secret_get` 🛡️ - Retrieve secret (requires elicitation)
- `azmcp_keyvault_secret_list` 🛡️ - List secrets (requires elicitation)
- `azmcp_keyvault_secret_delete` ⚠️ - Delete secret (destructive)

**Pricing:** ~$0.03 per 10,000 operations

**Use Cases:** Secret storage, connection strings, API keys, certificates

#### Azure RBAC (`role`)
View and manage role-based access control.

**Common Tools:**
- `azmcp_role_assignment_list` - List role assignments
- `azmcp_role_assignment_create` - Assign role
- `azmcp_role_definition_list` - List available roles

**Use Cases:** Access control, permissions management

---

### Developer Tools

#### Azure Application Insights (`applicationinsights`)
List and manage Application Insights resources.

**Common Tools:**
- `azmcp_applicationinsights_create` - Create Application Insights
- `azmcp_applicationinsights_list` - List components
- `azmcp_applicationinsights_query` - Query telemetry data

**Pricing:** ~$2.30/GB of data ingested

**Use Cases:** Application monitoring, telemetry, diagnostics

#### Azure App Configuration (`appconfig`)
Manage centralized application settings and feature flags.

**Common Tools:**
- `azmcp_appconfig_create` - Create App Configuration store
- `azmcp_appconfig_set` - Set configuration value
- `azmcp_appconfig_get` 🛡️ - Get configuration value (may contain secrets)

**Use Cases:** Feature flags, centralized config, A/B testing

---

### Management

#### Resource Groups (`group`)
Manage Azure resource groups.

**Common Tools:**
- `azmcp_group_create` - Create resource group
- `azmcp_group_list` - List resource groups
- `azmcp_group_delete` ⚠️ - Delete resource group and ALL contents (destructive)

**Use Cases:** Resource organization, deployment containers

#### Subscriptions (`subscription`)
List Azure subscriptions.

**Common Tools:**
- `azmcp_subscription_list` - List accessible subscriptions

**Use Cases:** Multi-subscription management

#### Azure Monitor (`monitor`)
Query logs and metrics.

**Common Tools:**
- `azmcp_monitor_logs_query` - Query Log Analytics
- `azmcp_monitor_metrics_query` - Query metrics

**Use Cases:** Observability, diagnostics, alerts

---

### DevOps

#### Azure Bicep Schema (`bicepschema`)
Retrieve Bicep schemas for Infrastructure-as-Code templates.

**Common Tools:**
- `azmcp_bicepschema_get` - Get schema for resource type

**Use Cases:** Bicep template authoring, IAC validation

#### Azure Deploy (`deploy`)
Deploy and manage Azure resources using templates.

**Common Tools:**
- `azmcp_deploy_template` - Deploy ARM/Bicep template
- `azmcp_deploy_script` - Execute deployment script

**Use Cases:** Infrastructure-as-Code deployment

---

## Tool Naming Patterns

All Azure MCP tools follow consistent naming:

**Pattern:** `azmcp_[namespace]_[resource]_[operation]`

**Examples:**
- `azmcp_storage_account_create` - Create storage account
- `azmcp_postgres_server_delete` - Delete PostgreSQL server
- `azmcp_keyvault_secret_get` - Get Key Vault secret

### Common Operations

| Operation Suffix | Purpose | Example |
|-----------------|---------|---------|
| `_create` | Create new resource | `azmcp_appservice_webapp_create` |
| `_list` | List resources | `azmcp_storage_account_list` |
| `_get` | Get resource details | `azmcp_keyvault_secret_get` |
| `_update` | Update resource | `azmcp_appconfig_update` |
| `_set` | Set configuration | `azmcp_appservice_webapp_config_set` |
| `_delete` | Delete resource ⚠️ | `azmcp_storage_account_delete` |
| `_query` | Query data | `azmcp_monitor_logs_query` |
| `_deploy` | Deploy code/config | `azmcp_appservice_webapp_deploy` |

---

## Service Selection Guide

### Static Website Hosting

**Recommended Service:** Azure Storage  
**Namespace:** `storage`  
**Cost:** ~$0.50-5/month

**Key Tools:**
```
azmcp_storage_account_create
azmcp_storage_account_enable_static_website
azmcp_storage_blob_upload
```

**Alternative:** Azure Static Web Apps (if CI/CD needed)

---

### Web Applications (Platform)

**Recommended Service:** Azure App Service  
**Namespace:** `appservice`  
**Cost:** ~$13-150/month (B1-S1 tier)

**Key Tools:**
```
azmcp_appservice_plan_create
azmcp_appservice_webapp_create
azmcp_appservice_webapp_config_set
azmcp_appservice_webapp_deploy
azmcp_appservice_webapp_identity_assign
```

**Supporting Services:**
- Key Vault (`keyvault`) - Store secrets
- Application Insights (`applicationinsights`) - Monitoring

---

### Web Applications (Container)

**Recommended Service:** Azure App Service (container) OR Container Apps  
**Namespace:** `appservice` + `acr`  
**Cost:** ~$50-200/month

**Key Tools:**
```
azmcp_acr_create
azmcp_acr_build
azmcp_appservice_webapp_create (with container config)
```

**Supporting Services:**
- Container Registry (`acr`) - Store images
- Key Vault (`keyvault`) - Secrets
- Managed Identity for ACR pull

---

### API + Database

**Recommended Services:** App Service + PostgreSQL/SQL  
**Namespaces:** `appservice` + `postgres` or `sql`  
**Cost:** ~$50-150/month

**Key Tools:**
```
# Foundation
azmcp_group_create
azmcp_keyvault_create

# Database
azmcp_postgres_server_create
azmcp_postgres_database_create
azmcp_keyvault_secret_set (store connection string)

# Web App
azmcp_appservice_plan_create
azmcp_appservice_webapp_create
azmcp_appservice_webapp_config_set (reference Key Vault secret)
```

**Connection String Pattern:**
```
DATABASE_URL=@Microsoft.KeyVault(SecretUri=https://[vault-name].vault.azure.net/secrets/[secret-name])
```

---

### Kubernetes Applications

**Recommended Service:** Azure Kubernetes Service  
**Namespace:** `aks` + `acr`  
**Cost:** ~$300-1000+/month

**Key Tools:**
```
azmcp_aks_cluster_create
azmcp_acr_create
azmcp_aks_cluster_get_credentials
```

**Supporting Services:**
- Container Registry (`acr`)
- Load Balancer (automatic)
- Azure Monitor (`monitor`)

---

## Cost Reference Table

Quick reference for pricing estimates:

| Service | Tier | Monthly Cost | Specs |
|---------|------|--------------|-------|
| **App Service** | B1 | $13 | 1 core, 1.75GB RAM |
| **App Service** | S1 | $70 | Auto-scale, staging slots |
| **App Service** | P1v3 | $150 | 2 cores, 8GB RAM |
| **PostgreSQL** | Basic 1 vCore | $30 | 1 vCore, 50GB storage |
| **PostgreSQL** | GP 2 vCore | $120 | 2 vCores, 100GB storage |
| **Redis Cache** | Basic C0 | $15 | 250MB cache |
| **Redis Cache** | Standard C1 | $75 | 1GB cache |
| **Storage Account** | Standard_LRS | $0.50 + usage | ~$0.02/GB/month |
| **Container Registry** | Basic | $5 | 10GB storage |
| **AKS** | 2 nodes B-series | $150 | 2 nodes, 2 vCPU each |
| **Key Vault** | Standard | $0.03/10k ops | Secrets storage |
| **Application Insights** | Basic | $2.30/GB | Telemetry ingestion |
| **Cosmos DB** | Provisioned | $24/100 RU/s | Starts at 400 RU/s = $96/month |

---

## Best Practices by Service

### App Service Best Practices

**Authentication:**
- Use managed identity (not connection strings)
- Configure in `azmcp_appservice_webapp_identity_assign`

**Configuration:**
- Store secrets in Key Vault
- Reference via `@Microsoft.KeyVault(...)` syntax
- Set via `azmcp_appservice_webapp_config_set`

**Deployment:**
- Use deployment slots for zero-downtime
- Deploy to staging, swap when ready
- Keep previous deployment for rollback

**Monitoring:**
- Always configure Application Insights
- Link via `APPLICATIONINSIGHTS_CONNECTION_STRING`

### Database Best Practices

**Security:**
- Never expose connection strings in code
- Always use Key Vault for storage
- Configure firewall rules (allow Azure services)
- Use managed identity when possible

**Backup:**
- Enable automated backups (default: 7 days)
- Increase retention for production (30 days)
- Test restore procedures

**Scaling:**
- Start with Basic tier for dev/test
- Use General Purpose for production
- Monitor performance before scaling up

### Storage Best Practices

**Static Websites:**
- Enable HTTPS (automatic)
- Configure custom domain via CDN
- Set cache-control headers
- Use $web container

**Security:**
- Use SAS tokens for temporary access
- Prefer managed identity over access keys
- Configure CORS if needed
- Enable soft delete for blobs

### Key Vault Best Practices

**Access Control:**
- Enable RBAC (not access policies)
- Use managed identity for apps
- Least privilege principle
- Audit access logs

**Secrets:**
- Rotate secrets regularly
- Use secret versions
- Enable soft delete
- Monitor expiration dates

---

## Common Deployment Patterns

### Pattern 1: Simple Static Website

**Services:** Storage  
**Tools:**
```
1. azmcp_storage_account_create
2. azmcp_storage_account_enable_static_website
3. azmcp_storage_blob_upload (for each file)
```

**Cost:** ~$0.50/month  
**Time:** 5 minutes

---

### Pattern 2: Web App + Database

**Services:** App Service, PostgreSQL, Key Vault, Application Insights  
**Tools:**
```
# Foundation
1. azmcp_group_create
2. azmcp_keyvault_create

# Data Tier
3. azmcp_postgres_server_create
4. azmcp_keyvault_secret_set (store DB connection)

# Compute
5. azmcp_appservice_plan_create
6. azmcp_appservice_webapp_create
7. azmcp_appservice_webapp_identity_assign
8. azmcp_appservice_webapp_config_set (DB connection from Key Vault)

# Monitoring
9. azmcp_applicationinsights_create
10. azmcp_appservice_webapp_config_set (App Insights connection)

# Deploy
11. azmcp_appservice_webapp_deploy
```

**Cost:** ~$75/month  
**Time:** 20 minutes

---

### Pattern 3: Containerized Application

**Services:** Container Registry, App Service (container), Key Vault  
**Tools:**
```
1. azmcp_group_create
2. azmcp_acr_create
3. azmcp_acr_build (build Docker image)
4. azmcp_appservice_plan_create
5. azmcp_appservice_webapp_create (with container config)
6. azmcp_appservice_webapp_identity_assign (for ACR pull)
```

**Cost:** ~$50-150/month  
**Time:** 15-25 minutes

---

## Tool Usage Examples

### Creating Resource Group

**Natural Language Prompt:**
```
"Create resource group 'rg-myapp-dev' in East US with tags environment=dev, managed-by=amplifier"
```

**Parameters:**
```yaml
subscription: "[subscription-id]"
resource_group: "rg-myapp-dev"
location: "eastus"
tags:
  environment: "dev"
  managed-by: "amplifier"
```

---

### Creating PostgreSQL Database

**Natural Language Prompt:**
```
"Create PostgreSQL server 'postgres-myapp-dev' with Basic tier 1 vCore and 50GB storage in resource group 'rg-myapp-dev'"
```

**Parameters:**
```yaml
subscription: "[subscription-id]"
resource_group: "rg-myapp-dev"
server_name: "postgres-myapp-dev"
location: "eastus"
sku: "B_Gen5_1"
storage_mb: 51200
administrator_login: "[username]"
administrator_password: "[secure-password]"
version: "14"
```

---

### Storing Secret in Key Vault

**Natural Language Prompt:**
```
"Store secret 'db-connection-string' with value '[connection-string]' in Key Vault 'kv-myapp-dev'"
```

**Parameters:**
```yaml
vault_name: "kv-myapp-dev"
secret_name: "db-connection-string"
value: "[connection-string]"
```

🛡️ **Note:** This is a secret-setting operation (safe), but retrieving it later requires user elicitation.

---

### Deploying Web App

**Natural Language Prompt:**
```
"Deploy application from 'dist/' directory to web app 'app-myapp-dev' in resource group 'rg-myapp-dev'"
```

**Parameters:**
```yaml
subscription: "[subscription-id]"
resource_group: "rg-myapp-dev"
webapp_name: "app-myapp-dev"
source_path: "dist/"
```

---

## Troubleshooting

### Authentication Issues

**Problem:** "Authentication failed"  
**Solution:** Run `az login` to authenticate Azure CLI

**Problem:** "Insufficient permissions"  
**Solution:** Ensure user has Contributor or Owner role on subscription/resource group

### Tool Not Found

**Problem:** "Tool [name] not found"  
**Solution:** Check tool name spelling, verify namespace is correct

**Example:** `azmcp_storage_account_create` (correct) not `azmcp_storage_create_account` (wrong)

### Quota Exceeded

**Problem:** "Quota exceeded for resource type"  
**Solution:** Azure Portal → Subscription → Usage + quotas → Request increase

### Cost Concerns

**Problem:** "Estimated cost too high"  
**Solution:** Use lower-tier services:
- App Service: B1 instead of S1 (saves $57/month)
- PostgreSQL: Basic instead of GP (saves $90/month)

---

## Quick Reference

### Most Common Tools for Web App Deployment

```
1. azmcp_group_create                    - Foundation
2. azmcp_keyvault_create                 - Security
3. azmcp_postgres_server_create          - Database (if needed)
4. azmcp_appservice_plan_create          - Compute foundation
5. azmcp_appservice_webapp_create        - Web app
6. azmcp_appservice_webapp_identity_assign - Security
7. azmcp_appservice_webapp_config_set    - Configuration
8. azmcp_applicationinsights_create      - Monitoring
9. azmcp_appservice_webapp_deploy        - Deploy code
```

### Always Remember

✅ **Use managed identity** instead of connection strings  
✅ **Store secrets in Key Vault** not environment variables  
✅ **Enable Application Insights** for monitoring  
✅ **Tag resources** for organization and cost tracking  
✅ **Use credential auth method** (most secure)  
✅ **Start with Basic tiers** then scale up  

⚠️ **Destructive operations** require user confirmation  
🛡️ **Secret operations** trigger elicitation prompts  

---

**Last Updated:** 2026-01-30  
**Azure MCP Server Version:** Latest  
**Source:** https://learn.microsoft.com/en-us/azure/developer/azure-mcp-server/tools/
