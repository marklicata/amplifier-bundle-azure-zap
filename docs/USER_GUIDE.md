# Azure ZAP User Guide

**Quick start guide for deploying to Azure using natural language.**

---

## Prerequisites

Before using Azure ZAP, ensure you have:

1. **Azure MCP Server configured:**
   ```bash
   npm install -g @azure/mcp-server
   amplifier mcp add azure-mcp
   ```

2. **Azure CLI authenticated:**
   ```bash
   az login
   ```

3. **Azure subscription** with sufficient permissions (Contributor or Owner)

4. **Amplifier installed** and running

---

## Loading the Bundle

Start an Amplifier session with Azure ZAP:

```bash
amplifier run --bundle /mnt/c/Users/malicata/source/azure-zap/bundle.md
```

Or if you've published it to a git repository:

```bash
amplifier run --bundle git+https://github.com/yourusername/azure-zap@main
```

---

## Basic Usage

### Simple Deployment Request

```
"Analyze my project at /path/to/my/webapp and recommend Azure services"
```

Azure ZAP will:
1. Use **project-analyzer** to examine your codebase
2. Recommend appropriate Azure services
3. Estimate costs

---

### Create Deployment Plan

```
"Create a deployment plan for my Node.js API at /path/to/my/api"
```

Azure ZAP will:
1. Analyze your project structure
2. Consult **azure-mcp-expert** for service selection
3. Use **azure-task-planner** to create multi-phase plan
4. Estimate time and cost

---

### Validate Safety

```
"Validate this deployment plan for safety and cost"
```

Azure ZAP will:
1. Use **azure-task-watchdog** to check:
   - Cost against budget
   - Azure quota availability
   - Destructive operations
   - Production safeguards
   - Compliance requirements

---

## Example Workflows

### Scenario 1: Deploy Static Website

**Your request:**
```
"I have a React website at ~/projects/my-site. Help me deploy it to Azure."
```

**What happens:**

**Step 1: Project Analysis**
```
Using project-analyzer agent to examine ~/projects/my-site...

✅ Detected: React application
   - Found: package.json, public/, src/, build/
   - Framework: Create React App
   - Build output: build/ directory

Recommendation:
- Azure Storage (static website hosting)
- Estimated cost: $0.50-5/month
- Complexity: Simple
```

**Step 2: Task Plan**
```
Using azure-task-planner to create deployment strategy...

Deployment Plan: my-site

Phase 1: Create Storage Account
  - Tool: azmcp_storage_account_create
  - Name: stmysitedev (auto-generated)
  - Time: 2 minutes

Phase 2: Enable Static Website Hosting
  - Tool: azmcp_storage_account_enable_static_website
  - Index document: index.html
  - Time: 1 minute

Phase 3: Upload Files
  - Tool: azmcp_storage_blob_upload
  - Source: build/
  - Container: $web
  - Time: 2 minutes

Total time: ~5 minutes
Total cost: ~$0.50/month
```

**Step 3: Safety Validation**
```
Using azure-task-watchdog to validate...

✅ APPROVED

Cost: $0.50/month (well within budget)
Quota: Minimal usage
Destructive operations: None
Compliance: ✅ All checks passed
```

**Step 4: Execute**
```
Using azure-task-executor to execute the approved plan...

✅ Phase 1 completed (2 min)
✅ Phase 2 completed (1 min)
✅ Phase 3 completed (2 min)

Deployment successful!
Website URL: https://stmysitedev.z13.web.core.windows.net
```

---

### Scenario 2: Deploy Node.js API with Database

**Your request:**
```
"Deploy my Express API at ~/projects/my-api to Azure. It needs PostgreSQL."
```

**What happens:**

**Step 1: Project Analysis**
```
Using project-analyzer agent...

✅ Detected: Node.js API
   - Framework: Express.js
   - Database: PostgreSQL (detected pg dependency)
   - Environment variables: DATABASE_URL required

Recommended Services:
- Azure App Service (B1 tier)
- Azure Database for PostgreSQL (Basic tier)
- Azure Key Vault (for secrets)
- Application Insights (monitoring)

Estimated cost: $75/month
Complexity: Moderate
```

**Step 2: Task Plan**
```
Using azure-task-planner and azure-mcp-expert...

Deployment Plan: my-api

Phase 1: Foundation (3 min)
  1.1 Create Resource Group: rg-myapi-dev
  1.2 Create Key Vault: kv-myapi-dev

Phase 2: Data Tier (8 min)
  2.1 Create PostgreSQL Server: postgres-myapi-dev
  2.2 Store connection string in Key Vault

Phase 3: Compute Tier (7 min)
  3.1 Create App Service Plan: plan-myapi-dev (B1)
  3.2 Create Web App: app-myapi-dev
  3.3 Enable Managed Identity
  3.4 Configure app settings (DB from Key Vault)

Phase 4: Monitoring (3 min)
  4.1 Create Application Insights
  4.2 Link to Web App

Phase 5: Deploy (5 min)
  5.1 Deploy application code

Total: ~25 minutes
Cost: ~$75/month
```

**Step 3: Safety Validation**
```
Using azure-task-watchdog...

✅ APPROVED WITH WARNINGS

Cost Analysis:
- Estimated: $75/month
- Budget: $100/month
- Utilization: 75%
- Status: ✅ APPROVED

Quota Check:
- Required: 2 vCPUs, 4GB RAM
- Available: Sufficient
- Status: ✅ APPROVED

Compliance:
- Naming: ✅ Follows conventions
- Tags: ⚠️  Recommended to add cost-center tag
- Security: ✅ Managed identity, Key Vault configured
```

**Step 4: Execute**
```
Using azure-task-executor to execute 5-phase deployment...

✅ Phase 1: Foundation completed (3 min)
✅ Phase 2: Data Tier completed (8 min)
✅ Phase 3: Compute Tier completed (7 min)
✅ Phase 4: Monitoring completed (3 min)
✅ Phase 5: Deploy completed (5 min)

Deployment successful!
API URL: https://app-myapi-dev.azurewebsites.net
Database: postgres-myapi-dev.postgres.database.azure.com
```

---

## Agent Reference

### When to Use Each Agent

**project-analyzer:**
```
"Analyze my project at [path]"
"What Azure services does my project need?"
"Is my app ready to deploy?"
```

**azure-mcp-expert:**
```
"What Azure MCP tools create a storage account?"
"How do I configure PostgreSQL using Azure MCP?"
"What's the cost difference between App Service tiers?"
```

**azure-task-planner:**
```
"Create task plan for [project]"
"What's the deployment strategy for this app?"
"Show me the phases for deploying this service"
```

**azure-task-watchdog:**
```
"Validate this task plan"
"Is this deployment safe?"
"Check if I have enough quota for this deployment"
```

**azure-task-executor:**
```
"Execute the approved plan"
"Deploy to Azure using the validated plan"
"Run the deployment phases"
```

**recipe-generator:**
```
"Save this deployment as a recipe"
"Can I reuse this deployment pattern?"
"Generate recipe from this successful deployment"
```

---

## Configuration

### Optional Configuration File

Create `~/.amplifier/azure-zap.yaml`:

```yaml
# Azure defaults
subscription:
  default_id: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
  default_region: "eastus"

# Safety settings
watchdog:
  mode: "strict"  # strict | development | production
  budget:
    monthly_limit: 100  # USD
    warning_threshold: 0.8
```

**Default behavior if not configured:**
- Uses current Azure subscription
- Strict safety mode
- No budget limits (all deployments allowed)

---

## Tips and Best Practices

### Start Small
- Deploy to dev environment first
- Use Basic tiers initially
- Scale up after validating

### Use Managed Identity
- Always prefer managed identity over connection strings
- More secure and easier to manage

### Store Secrets in Key Vault
- Never put secrets in environment variables
- Use Key Vault references in app settings

### Monitor Your Apps
- Enable Application Insights by default
- Set up alerts for critical metrics

### Save Successful Deployments as Recipes
- After first successful deployment, generate a recipe
- Reuse for future similar projects
- Share with your team

---

## Troubleshooting

### "Azure MCP Server not configured"
**Solution:**
```bash
npm install -g @azure/mcp-server
amplifier mcp add azure-mcp
```

### "Authentication failed"
**Solution:**
```bash
az login
```

### "Insufficient permissions"
**Solution:** Ask your Azure admin for Contributor role on subscription

### "Cost exceeds budget"
**Solution:** Adjust budget in config or use lower-tier services

### "Agent failed to load"
**Solution:** Check bundle.md is valid, all agent files exist

---

## Next Steps

After successful deployment:

1. **Save as recipe** for future use
2. **Add custom domain** via Azure Portal
3. **Configure CI/CD** with GitHub Actions
4. **Set up alerts** in Application Insights
5. **Scale up** tiers when ready for production

---

**For more help:**
- See [ARCHITECTURE_PLAN.md](../ARCHITECTURE_PLAN.md) for complete design
- See [README.md](../README.md) for overview
- Check agent files in `agents/` for detailed capabilities
