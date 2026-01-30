# Azure ZAP (Zero-to-Azure Planner)

**Intelligent Azure deployment orchestration for Amplifier.**

Transform natural language requests like "deploy my website" into validated, executable Azure deployment plans.

## 🚀 What is Azure ZAP?

Azure ZAP is an Amplifier bundle that bridges the gap between developer intent and Azure infrastructure. It:

- 🔍 **Analyzes your project** - Automatically detects project type, dependencies, and requirements
- 📋 **Plans deployments** - Creates phased deployment strategies with proper ordering
- 🛡️ **Validates safety** - Prevents cost overruns, quota exhaustion, and dangerous operations
- 🎯 **Uses Azure MCP** - All Azure operations via standardized Azure MCP Server tools
- 🔄 **Generates recipes** - Save successful deployments as reusable templates

## Quick Example

```
User: "Deploy my Node.js API to Azure"

Azure ZAP:
1. ✅ Detected Express + PostgreSQL dependencies
2. ✅ Recommends: App Service (B1) + PostgreSQL (Basic) + Key Vault
3. ✅ Estimated cost: $75/month (within budget)
4. ✅ Creates 5-phase deployment plan
5. ⏳ Ready to execute (awaiting approval)

Time: 20 minutes | Cost: $75/month
```

## Prerequisites

1. **Azure MCP Server:**
   ```bash
   npm install -g @azure/mcp-server
   amplifier mcp add azure-mcp
   ```

2. **Azure CLI authenticated:**
   ```bash
   az login
   ```

3. **Sufficient Azure quota** (varies by deployment)

## Installation

```bash
# Add azure-zap bundle to your Amplifier configuration
amplifier bundle add azure-zap
```

Or include in your app's bundle composition.

## Usage

### Basic Deployment

```
"Deploy my website to Azure"
```

Azure ZAP handles everything:
- Project analysis
- Service recommendations
- Cost estimation
- Safety validation
- Phased execution

### Specify Environment

```
"Deploy my API to production"
```

Production deployments include additional safeguards:
- Change ticket requirement
- Manual approval gates
- Rollback plan validation

### Save as Recipe

```
"Save this deployment as a recipe"
```

Creates reusable recipe YAML for future deployments.

## How It Works

### The Five Agents

1. **project-analyzer** - Examines your codebase
2. **azure-mcp-expert** - Authoritative Azure MCP consultant
3. **deployment-planner** - Creates multi-phase plans
4. **deployment-watchdog** - Safety and compliance validator
5. **recipe-generator** - Converts deployments to recipes

### The Flow

```
Request → Analyze → Plan → Validate → Approve → Execute → (Optional) Recipe
```

Every deployment goes through safety validation before execution.

## Supported Project Types

### ✅ Currently Supported
- Static websites (HTML/CSS/JS, React, Vue, Angular)
- Node.js applications (Express, Fastify, NestJS)
- Python applications (Django, Flask, FastAPI)
- Containerized applications (Dockerfile present)

### 🚧 Coming Soon
- .NET applications (ASP.NET Core)
- Go applications
- Rust applications
- Multi-container (docker-compose)
- Kubernetes applications (AKS)

## Configuration

Create `~/.amplifier/azure-zap.yaml`:

```yaml
# Azure defaults
subscription:
  default_id: "your-subscription-id"
  default_region: "eastus"

# Safety settings
safety:
  mode: "strict"
  cost_alert_threshold: 100  # USD/month

# Deployment defaults
deployment:
  default_tier: "basic"
  enable_monitoring: true
  tags:
    managed-by: "amplifier"
    team: "your-team"
```

See [Configuration Guide](docs/CONFIGURATION.md) for all options.

## Example Scenarios

### Simple: Static Website

**Input:** "Deploy my React website"

**Output:**
- Azure Storage (static website hosting)
- Cost: ~$0.50/month
- Time: 5 minutes

### Moderate: Web App + Database

**Input:** "Deploy my Express API with PostgreSQL"

**Output:**
- App Service (B1)
- Azure Database for PostgreSQL (Basic)
- Key Vault
- Application Insights
- Cost: ~$75/month
- Time: 20 minutes

### Complex: Microservices

**Input:** "Deploy my microservices application"

**Output:**
- Azure Kubernetes Service (2 nodes)
- Container Registry
- Multiple databases
- Service mesh setup
- Cost: ~$600/month
- Time: 60 minutes

## Safety Features

### 🛡️ Cost Protection
- Pre-deployment estimates
- Budget threshold enforcement
- **BLOCKS** over-budget deployments

### 📊 Quota Validation
- Checks available vCPUs/RAM/storage
- **BLOCKS** if insufficient quota

### ⚠️ Destructive Operation Protection
- Detects delete/purge operations
- **REQUIRES** explicit approval
- Impact analysis

### 🏭 Production Safeguards
- Change ticket required
- Manual approval gates
- Rollback plan validation
- Database backup verification

## Documentation

- [Architecture Plan](ARCHITECTURE_PLAN.md) - Complete design document
- [User Guide](docs/USER_GUIDE.md) - Comprehensive usage guide (coming soon)
- [Agent Reference](docs/AGENT_REFERENCE.md) - Agent capabilities (coming soon)
- [Troubleshooting](docs/TROUBLESHOOTING.md) - Common issues (coming soon)

## Roadmap

### Phase 1: MVP (Current)
- ✅ Core agents implemented
- ✅ Architecture documented
- 🚧 Basic deployment flows
- 🚧 Testing with real projects

### Phase 2: Enhanced Safety (Next)
- Complete cost estimation
- Production safeguards
- Compliance validation

### Phase 3: Recipe Library
- Recipe generation working
- Pre-built recipe patterns
- Recipe sharing

### Phase 4: Advanced Features
- AKS microservices
- Multi-region deployments
- Infrastructure as Code export

## Contributing

Contributions welcome! Areas to help:

1. **Add project type detection** - New frameworks/languages
2. **Create recipes** - Pre-built deployment patterns
3. **Enhance safety** - Additional validation rules
4. **Improve docs** - Examples, guides, tutorials

## Support

- **Issues:** GitHub Issues (link TBD)
- **Discussions:** GitHub Discussions (link TBD)
- **Documentation:** [Link to docs site]

## License

MIT License

## Acknowledgments

Built on top of:
- [Amplifier](https://github.com/microsoft/amplifier) - AI agent framework
- [Azure MCP Server](https://github.com/microsoft/azure-mcp-server) - Azure integration
- Microsoft Semantic Kernel - AI orchestration

---

**Version:** 0.1.0-alpha  
**Status:** Early Development  
**Last Updated:** January 2026

**⚠️ Note:** This is early-stage software. Test thoroughly in dev environments before using in production.
