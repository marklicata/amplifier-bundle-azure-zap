---
bundle:
  name: azure-zap
  version: 0.1.0
  description: "Azure Zero-to-Azure Planner - Intelligent deployment orchestration bundle that transforms natural language requests like 'deploy my website' into validated, executable Azure deployment plans using Azure MCP Server."

tools:
  - module: tool-filesystem
    source: git+https://github.com/microsoft/amplifier-module-tool-filesystem@main
  - module: tool-search
    source: git+https://github.com/microsoft/amplifier-module-tool-search@main
  - module: tool-bash
    source: git+https://github.com/microsoft/amplifier-module-tool-bash@main
  - module: tool-mcp
    source: git+https://github.com/microsoft/amplifier-module-tool-mcp@main

agents:
  include:
    - azure-zap:project-analyzer
    - azure-zap:azure-mcp-expert
    - azure-zap:azure-task-planner
    - azure-zap:azure-task-watchdog
    - azure-zap:azure-task-executor
    - azure-zap:recipe-generator
    - https://raw.githubusercontent.com/microsoft/amplifier-foundation/main/agents/session-analyst.md
---

# Azure ZAP (Zero-to-Azure Planner)

**Intelligent Azure deployment orchestration for Amplifier.**

## What is Azure ZAP?

Azure ZAP transforms natural language deployment requests into validated, executable Azure deployment plans. It analyzes your project, recommends appropriate Azure services, validates safety and cost, and leverages Azure MCP Server for all Azure operations.

## Core Capabilities

### 🔍 Intelligent Project Analysis
Automatically examines your codebase to determine:
- Project type and framework
- Required Azure services
- Database and storage needs
- Security requirements
- Deployment complexity

### 📋 Smart Deployment Planning
Creates comprehensive, phased deployment plans with:
- Service dependency ordering
- Rollback strategies for each phase
- Health check verification
- Cost estimation and tier recommendations

### 🛡️ Safety Guardrails
Prevents costly mistakes with:
- Budget threshold enforcement
- Azure quota validation
- Destructive operation detection
- Production deployment safeguards
- Compliance checking

### 🎯 Azure MCP Integration
All Azure operations use Azure MCP Server tools:
- Standardized tool usage
- Authentication best practices
- Secret handling via elicitation
- Tool annotation awareness

### 🔄 Recipe Generation
Save successful deployments as reusable recipes:
- Parameterized for different projects
- Approval gates for sensitive operations
- Complete documentation
- Team sharing ready

## Quick Start

### Prerequisites

1. **Azure MCP Server configured:**
   ```bash
   npm install -g @azure/mcp-server
   amplifier mcp add azure-mcp
   ```

2. **Azure CLI authenticated:**
   ```bash
   az login
   ```

3. **Azure ZAP bundle loaded** (automatic when using Amplifier)

### Basic Usage

**Simple deployment:**
```
"Deploy my website to Azure"
```

Azure ZAP will:
1. Analyze your project structure
2. Recommend appropriate Azure services
3. Create a deployment plan
4. Validate cost and safety
5. Execute with your approval

**Specify environment:**
```
"Deploy my Node.js API to production"
```

**Save as recipe:**
```
"Save this deployment as a recipe for future use"
```

## How It Works

### The Orchestration Flow

```
User Request: "Deploy my website"
    ↓
┌─────────────────────────────────────────┐
│  Project Analyzer Agent                 │
│  - Examines codebase                    │
│  - Identifies dependencies              │
│  - Recommends Azure services            │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  Azure MCP Expert Agent                 │
│  - Validates service selection          │
│  - Provides tool guidance               │
│  - Estimates costs                      │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  Azure Task Planner Agent               │
│  - Creates multi-phase plan             │
│  - Orders service dependencies          │
│  - Defines rollback strategies          │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  Azure Task Watchdog Agent              │
│  - Validates budget compliance          │
│  - Checks Azure quotas                  │
│  - Detects destructive operations       │
│  - Enforces production safeguards       │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  User Approval Gate                     │
│  - Review plan                          │
│  - Approve cost                         │
│  - Confirm services                     │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  Azure Task Executor Agent              │
│  - Phase-by-phase execution             │
│  - Health checks after each phase       │
│  - Rollback on failure                  │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  Recipe Generator Agent (Optional)      │
│  - Extract deployment pattern           │
│  - Parameterize for reuse               │
│  - Generate recipe YAML                 │
└─────────────────────────────────────────┘
```

## Configuration

Create `~/.amplifier/azure-zap.yaml`:

```yaml
# Azure subscription defaults
subscription:
  default_id: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
  default_resource_group: "rg-dev"
  default_region: "eastus"

# Naming conventions
naming:
  prefix: "mycompany"
  environment_suffix: true  # Append -dev, -staging, -prod

# Safety settings
safety:
  mode: "strict"  # strict | development | production
  cost_alert_threshold: 100  # USD/month
  require_approval_above: 50  # USD/month
  block_destructive_in_prod: true

# Deployment defaults
deployment:
  default_tier: "basic"  # free | basic | standard | premium
  enable_monitoring: true
  enable_auto_scaling: false
  tags:
    managed-by: "amplifier"
    team: "platform"
    cost-center: "engineering"

# Watchdog configuration
watchdog:
  mode: "strict"  # strict | development | production
  budget:
    monthly_limit: 100  # USD
    warning_threshold: 0.8  # Warn at 80%
    allow_override: false
  production:
    require_change_ticket: true
    require_manual_approval: true
    require_rollback_plan: true
    require_health_checks: true
    require_backup_for_data_ops: true
    block_destructive_ops: true
  compliance:
    enforce_naming_conventions: true
    required_tags:
      - environment
      - managed-by
      - cost-center  # production only
    enforce_security_baseline: true
```

## Example Scenarios

### Scenario 1: Static Website

**Request:**
```
"Deploy my React website to Azure"
```

**What happens:**
1. Project Analyzer detects React build output
2. Recommends Azure Storage (static website hosting)
3. Creates simple deployment plan (3 steps)
4. Watchdog validates cost (~$0.50/month)
5. Executes: Create storage → Enable hosting → Upload files
6. **Time:** 5 minutes | **Cost:** $0.50/month

### Scenario 2: Node.js API + Database

**Request:**
```
"Deploy my Express API with PostgreSQL"
```

**What happens:**
1. Project Analyzer detects Express + pg dependency
2. Recommends App Service + PostgreSQL + Key Vault
3. Creates 5-phase deployment plan
4. Watchdog validates cost (~$75/month)
5. Executes:
   - Phase 1: Resource Group + Key Vault
   - Phase 2: PostgreSQL (Basic tier)
   - Phase 3: App Service (B1)
   - Phase 4: Application Insights
   - Phase 5: Deploy code
6. **Time:** 20 minutes | **Cost:** $75/month

### Scenario 3: Microservices on AKS

**Request:**
```
"Deploy my microservices application"
```

**What happens:**
1. Project Analyzer detects kubernetes/ directory
2. Recommends AKS + ACR + supporting services
3. Creates 9-phase complex deployment plan
4. Watchdog validates cost (~$600/month, requires approval)
5. User reviews and approves
6. Executes multi-service deployment
7. **Time:** 60 minutes | **Cost:** $600/month

## Safety Features

### Cost Protection
- Pre-deployment cost estimation
- Budget threshold alerts
- **BLOCKS** deployments exceeding budget
- Tier alternatives suggested

### Quota Validation
- Checks available vCPUs, RAM, storage
- **BLOCKS** if insufficient quota
- Suggests quota increase process

### Destructive Operation Protection
- Detects delete, purge, drop operations
- **REQUIRES** explicit user approval
- Impact analysis before execution
- Audit logging

### Production Safeguards
When deploying to production:
- **REQUIRES** change ticket number
- **REQUIRES** manual approval gate
- **REQUIRES** validated rollback plan
- **BLOCKS** direct delete operations
- **REQUIRES** database backup strategy

### Secret Handling
- Azure MCP elicitation prompts user
- Secrets stored in Key Vault only
- Managed identity preferred over keys
- Connection strings never in environment variables

## Agent Reference

### project-analyzer
**Purpose:** Examine project structure and infer Azure deployment requirements

**Triggers:** "Deploy my [project]", "What Azure services do I need?"

**Output:** Project analysis with service recommendations

### azure-mcp-expert
**Purpose:** Authoritative consultant for all Azure MCP Server capabilities

**Triggers:** Service selection, tool usage questions, cost estimation

**Output:** Service recommendations, tool guidance, parameter help

### azure-task-planner
**Purpose:** Create comprehensive, phased Azure task plans (deployments, configurations, CRUD operations)

**Triggers:** After project analysis, "Create deployment plan", "Plan Azure changes"

**Output:** Multi-phase plan with dependencies and rollback strategies

### azure-task-watchdog
**Purpose:** Safety monitor and compliance validator for all Azure operations

**Triggers:** **REQUIRED** before executing any Azure task plan

**Output:** Validation report with APPROVED/BLOCKED verdict

### azure-task-executor
**Purpose:** Execute validated Azure task plans phase-by-phase using Azure MCP tools

**Triggers:** After azure-task-watchdog approval, "Execute the plan", "Deploy to Azure"

**Output:** Detailed execution report with status, resources created, and verification results

### recipe-generator
**Purpose:** Convert successful deployments into reusable recipes

**Triggers:** "Save this as a recipe", after successful deployment

**Output:** Parameterized recipe YAML with documentation

## Available Recipes

Pre-built recipes for common patterns:

- `deploy-static-website.yaml` - Static site to Azure Storage
- `deploy-nodejs-app.yaml` - Node.js to App Service
- `deploy-containerized-app.yaml` - Docker to Container Apps
- `deploy-aks-cluster.yaml` - Microservices to AKS

Use via:
```
"Run the deploy-nodejs-app recipe for my project 'myapi'"
```

## Best Practices

### Development Workflow
1. Start with dev environment (lower cost)
2. Test deployment end-to-end
3. Save as recipe once working
4. Use recipe for staging/production

### Cost Optimization
- Use Basic tiers for dev/staging
- Delete dev resources when not in use
- Enable auto-scaling only in production
- Review Azure Advisor recommendations

### Security
- Always use managed identity (not connection strings)
- Store all secrets in Key Vault
- Apply least privilege RBAC
- Enable HTTPS enforcement
- Configure network restrictions

### Production Deployments
- Use deployment slots for zero-downtime
- Implement blue-green deployment pattern
- Always backup databases before schema changes
- Monitor Application Insights after deployment
- Set up alerts for critical metrics

## Troubleshooting

### "Deployment blocked by watchdog - cost exceeds budget"
**Solution:** Adjust tiers or increase budget in config

### "Insufficient quota for deployment"
**Solution:** Azure Portal → Subscription → Usage + quotas → Request increase

### "Azure MCP Server not found"
**Solution:** Install with `npm install -g @azure/mcp-server` and configure

### "Authentication failed"
**Solution:** Run `az login` to authenticate Azure CLI

### "Resource name conflicts"
**Solution:** Use unique project names in deployment request

## Documentation

- [Architecture Plan](./ARCHITECTURE_PLAN.md) - Complete design and roadmap
- [Agent Reference](./docs/AGENT_REFERENCE.md) - Detailed agent capabilities
- [User Guide](./docs/USER_GUIDE.md) - Comprehensive usage guide
- [Troubleshooting](./docs/TROUBLESHOOTING.md) - Common issues and solutions

## Roadmap

### Phase 1: MVP (Current)
- ✅ Project analysis for common project types
- ✅ Simple and moderate deployment planning
- ✅ Cost and safety validation
- 🚧 Static website deployment
- 🚧 Node.js app deployment

### Phase 2: Enhanced Safety
- Comprehensive cost estimation
- Production safeguards
- Rollback automation
- Compliance validation

### Phase 3: Recipe Library
- Recipe generation from deployments
- 10+ pre-built recipe patterns
- Recipe sharing and discovery

### Phase 4: Advanced Deployments
- AKS microservices support
- Multi-region deployments
- Blue-green deployment patterns
- Infrastructure as Code generation

## Contributing

Azure ZAP is part of the Amplifier ecosystem. Contributions welcome:

1. Add support for new project types
2. Create pre-built recipes
3. Enhance safety validations
4. Improve Azure service detection

## License

MIT License - See LICENSE file

## Support

- GitHub Issues: [Report bugs or request features]
- Documentation: [Link to comprehensive docs]
- Community: [Discord/Slack/Forum]

---

**Version:** 0.1.0-alpha  
**Status:** Early Development  
**Last Updated:** 2026-01-30

Built with ❤️ by the Amplifier community
