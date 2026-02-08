# JasperReports PrintDocument

JasperReports templates for multi-country electronic invoicing and payroll system built on Adempiere ERP.

## Development Environment

### Required Tools
- **TIBCO Jaspersoft Studio 6.17.0**
- **PostgreSQL** (for database connections)
- Supported OS: Kubuntu 22.04, Ubuntu 25.04, Windows 11

### Database Connections
Database connection configurations are in `.xml` files (JDBC adapters):
- `COFIA_Local.xml` - COFIA database (localhost)
- `Chile_Local.xml` - Chile database (localhost)
- `AdempiereChile_remote.xml` - Remote Chile instance
- `SAPROD_Local.xml` - SAP production
- And others for specific environments

Update these files with your local database credentials and connection details.

## Report Structure

- **COFIA_\*.jrxml** - El Salvador electronic invoicing (Factura Electrónica)
- **inf_HR_\*.jrxml** - Human Resources / payroll reports
- **Print_OdC\*.jrxml** - Purchase orders
- **Sub_\*.jrxml** - Reusable subreports
- **\*.jasper** - Compiled binaries (committed alongside .jrxml)

⚠️ **Important**: Both `.jrxml` (source) and `.jasper` (compiled) files are committed to maintain version synchronization.

## Working with Claude Code

This repository is optimized for use with [Claude Code](https://claude.ai/code).

### Automatic Context Loading

When you run `claude` in this directory, it automatically loads:
- **CLAUDE.md** - Repository architecture, conventions, and patterns
- **.claude-memory/** - Shared team knowledge and recent work

### Setup for New Team Members

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd PrintDocument
   ```

2. **Install Claude Code** (if not already installed):
   ```bash
   npm install -g @anthropic/claude-code
   ```

3. **(Optional) Create personal Claude instructions:**

   Copy this template to `~/.claude/instructions.md` for proactive suggestions:

   ```markdown
   # Global Instructions for Claude Code

   ## Proactive Suggestions
   - Suggest `.claude-memory/` if missing in git repos
   - Suggest `CLAUDE.md` if missing in git repos
   - Recommend MCP servers when database work is detected
   - Point out opportunities for tests, automation, refactoring

   ## Git Practices
   - Do not commit automatically
   - Show git commands for user to run manually

   ## Project-Specific Context
   - Always check for and read `CLAUDE.md`
   - Always check for and read `.claude-memory/` files
   ```

4. **(Optional) Setup PostgreSQL MCP Server:**

   For direct database access in Claude Code, add to `~/.claude/config.json`:

   ```json
   {
     "mcpServers": {
       "postgres-cofia": {
         "command": "npx",
         "args": [
           "-y",
           "@modelcontextprotocol/server-postgres",
           "postgresql://adempiere:adempiere@localhost:5432/COFIA"
         ]
       }
     }
   }
   ```

   Adjust connection strings for your environment.

### Team Collaboration with Shared Memory

After solving problems or making discoveries:

1. Update relevant files in `.claude-memory/`:
   - `recent-work.md` - What you worked on
   - `known-issues.md` - Bugs and workarounds
   - `learned-patterns.md` - Best practices discovered

2. Commit and push:
   ```bash
   git add .claude-memory/
   git commit -m "Update team memory: [description]"
   git push
   ```

3. Before starting work, always pull latest:
   ```bash
   git pull
   ```

See `.claude-memory/README.md` for detailed usage instructions.

## Report Compilation

Subreports must be compiled before main reports:
1. Compile all `Sub_*.jrxml` files first
2. Then compile main reports (which reference `Sub_*.jasper`)

Main reports reference compiled `.jasper` files, not `.jrxml` sources.
