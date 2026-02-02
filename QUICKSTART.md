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

### Example 3: Create Task Plan

```
"Use azure-task-planner to create a plan for deploying my Express API at ~/my-api. It needs PostgreSQL."
```

**What happens:**
- Creates multi-phase task plan
- Orders service dependencies correctly
- Defines rollback strategies
- Provides cost breakdown

---

### Example 4: Validate Safety

```
"Use azure-task-watchdog to validate this plan"
```

**What happens:**
- Checks cost against budget
- Validates Azure quota
- Detects destructive operations
- Returns APPROVED or BLOCKED verdict

---

### Example 5: Execute the Plan

```
"Use azure-task-executor to execute the approved plan"
```

**What happens:**
- Executes each phase sequentially
- Runs health checks after each phase
- Handles secrets via Azure MCP elicitation
- Provides detailed execution report
- Automatically rolls back on failure

---

## Next Steps

After getting familiar with the agents:

1. **Analyze a real project** with project-analyzer
2. **Create a task plan** with azure-task-planner
3. **Validate safety** with azure-task-watchdog
4. **Execute the plan** with azure-task-executor
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

2. "Use azure-task-planner to create a static website deployment plan"
   → Returns: 3-phase plan (create storage, enable hosting, upload files)

3. "Use azure-task-watchdog to validate the plan"
   → Returns: ✅ APPROVED (cost: $0.50/month)

4. "Use azure-task-executor to execute the approved plan"
   → Executes: Creates storage, enables hosting, uploads files
   → Returns: ✅ SUCCESS - Website live at https://stmysitedev.z13.web.core.windows.net

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
