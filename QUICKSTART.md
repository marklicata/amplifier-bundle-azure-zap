# Azure ZAP Quick Start

**Get started with Azure deployment in 5 minutes.**

---

## Step 1: Prerequisites

Ensure you have:

```bash
# Azure MCP Server installed
npm install -g @azure/mcp-server

# Azure CLI authenticated
az login

# Amplifier installed
amplifier --version
```

---

## Step 2: Configure Azure MCP in Amplifier

```bash
amplifier mcp add azure-mcp
```

---

## Step 3: Load Azure ZAP Bundle

```bash
cd /mnt/c/Users/malicata/source/azure-zap
amplifier run --bundle ./bundle.md
```

---

## Step 4: Try It Out

### Example 1: Analyze Your Project

```
"Use project-analyzer to examine my project at ~/projects/my-webapp"
```

**What happens:**
- Agent scans your project files
- Detects framework and dependencies
- Recommends Azure services
- Estimates cost and complexity

---

### Example 2: Get Azure MCP Help

```
"Ask azure-mcp-expert: What tools do I need to deploy a Node.js app with PostgreSQL?"
```

**What happens:**
- Expert provides tool recommendations
- Lists required Azure MCP tools
- Shows parameters needed
- Estimates costs

---

### Example 3: Create Deployment Plan

```
"Use deployment-planner to create a plan for deploying my Express API at ~/my-api. It needs PostgreSQL."
```

**What happens:**
- Creates multi-phase deployment plan
- Orders service dependencies correctly
- Defines rollback strategies
- Provides cost breakdown

---

### Example 4: Validate Safety

```
"Use deployment-watchdog to validate this plan"
```

**What happens:**
- Checks cost against budget
- Validates Azure quota
- Detects destructive operations
- Returns APPROVED or BLOCKED verdict

---

## Next Steps

After getting familiar with the agents:

1. **Analyze a real project** with project-analyzer
2. **Create a deployment plan** with deployment-planner
3. **Validate safety** with deployment-watchdog
4. **Execute via Azure MCP Server** (use natural language prompts with Azure MCP tools)
5. **Save as recipe** with recipe-generator for future reuse

---

## Configuration (Optional)

Create `~/.amplifier/azure-zap.yaml` for defaults:

```yaml
subscription:
  default_id: "your-subscription-id"
  default_region: "eastus"

watchdog:
  mode: "strict"
  budget:
    monthly_limit: 100  # USD
```

---

## Simple Example End-to-End

**Goal:** Deploy a React website to Azure Storage

**Commands:**
```
1. "Use project-analyzer on my React app at ~/my-site"
   → Returns: Recommends Azure Storage, $0.50/month

2. "Use deployment-planner to create a static website deployment plan"
   → Returns: 3-phase plan (create storage, enable hosting, upload files)

3. "Use deployment-watchdog to validate the plan"
   → Returns: ✅ APPROVED (cost: $0.50/month)

4. Now execute via Azure MCP natural language:
   "Create storage account 'stmysitedev' in resource group 'rg-mysite-dev' in East US"
   "Enable static website hosting on storage account 'stmysitedev'"
   "Upload files from ~/my-site/build to $web container in storage account 'stmysitedev'"

5. "Use recipe-generator to save this deployment as a recipe"
   → Returns: Reusable recipe YAML
```

---

## Troubleshooting

**Bundle won't load:**
- Check all agent files exist in `agents/`
- Verify YAML frontmatter is valid
- Check network access to GitHub for tool sources

**Agents not available:**
- Ensure you loaded the bundle with `--bundle ./bundle.md`
- Check agent names match bundle.md configuration

**Azure MCP not working:**
- Verify `amplifier mcp list` shows azure-mcp
- Run `az login` to authenticate
- Check Azure subscription access

---

**You're ready to go!** Start with analyzing a real project and see what Azure ZAP recommends.
