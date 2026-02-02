# Azure ZAP Bundle Test

## Quick Test to Verify Bundle Works

### Test 1: Load the Bundle

```bash
cd /mnt/c/Users/malicata/source/azure-zap
amplifier run --bundle ./bundle.md "What agents are available?"
```

**Expected:** Session starts, lists 6 agents available

---

### Test 2: Use Project Analyzer

Create a test project:
```bash
mkdir -p /tmp/test-react-app
cd /tmp/test-react-app
echo '{"name":"test-app","dependencies":{"react":"^18.0.0"}}' > package.json
mkdir -p public src build
echo '<html><body>Test</body></html>' > public/index.html
```

Run analysis:
```bash
amplifier run --bundle /mnt/c/Users/malicata/source/azure-zap/bundle.md "Use the project-analyzer agent to analyze the project at /tmp/test-react-app"
```

**Expected:** Agent examines project, recommends Azure Storage for static hosting

---

### Test 3: Get Azure MCP Guidance

```bash
amplifier run --bundle /mnt/c/Users/malicata/source/azure-zap/bundle.md "Use azure-mcp-expert to explain what tools I need to create a storage account"
```

**Expected:** Expert lists `azmcp_storage_account_create` tool with parameters

---

### Test 4: Create Simple Deployment Plan

```bash
amplifier run --bundle /mnt/c/Users/malicata/source/azure-zap/bundle.md "Use azure-task-planner to create a plan for deploying a static website to Azure Storage. Project name: test-site, environment: dev"
```

**Expected:** Multi-phase task plan with cost estimate

---

### Test 5: Validate a Plan

```bash
amplifier run --bundle /mnt/c/Users/malicata/source/azure-zap/bundle.md "Use azure-task-watchdog to validate this plan: [paste plan from Test 4]"
```

**Expected:** Safety validation report with APPROVED/BLOCKED verdict

---

## Troubleshooting Bundle Load Issues

If bundle fails to load, check:

1. **YAML frontmatter syntax:**
   ```bash
   # Validate YAML is parseable
   python3 -c "import yaml; yaml.safe_load(open('bundle.md').read().split('---')[1])"
   ```

2. **Agent files exist:**
   ```bash
   ls -la agents/
   # Should show all 6 agent .md files
   ```

3. **Tool sources are accessible:**
   - All GitHub URLs should be valid
   - Network access required

4. **Context file exists:**
   ```bash
   ls -la context/azure-mcp-tools-reference.md
   ```

---

## What Makes a Working Bundle

✅ **bundle.md** - Main definition with valid YAML frontmatter  
✅ **agents/*.md** - All agent files present with valid schema  
✅ **context/*.md** - Context files referenced by agents  
✅ **tools** - Tool modules from valid Git sources  

The bundle should be loadable with:
```bash
amplifier run --bundle /mnt/c/Users/malicata/source/azure-zap/bundle.md
```
