# Recent Work

Track recent changes, ongoing work, and current context here.

## Format
```
### YYYY-MM-DD - [Developer Name]
**What was done:**
- Brief description of changes

**Context/Notes:**
- Important details for next person picking up work
```

---

### 2026-02-08 - Initial Setup
**What was done:**
- Created CLAUDE.md with codebase architecture documentation
- Set up .claude-memory/ structure for team collaboration
- Documented TIBCO Jaspersoft Studio 6.17.0 as development environment

**Context/Notes:**
- CLAUDE.md now auto-loads context about COFIA reports, subreports, and database configs
- Team memory system established for cross-machine collaboration

---

### 2026-02-08 - COFIA_CCFF_MainQRCode Report Fixes + MCP Setup
**What was done:**
- Set up PostgreSQL MCP servers (kaltmann-db, cofia-db) for direct database access in Claude Code
- Fixed "No Orden:" field width - increased from 140 to 162px to display full order numbers
- Aligned orderdescription field borders with Emisor/Receptor frame (x=1, width=571)
- Fixed vertical overlap by adjusting orderdescription y-position from 331 to 345
- Added Sub_extension subreport to summary band with dynamic sizing (0 pixels when empty)
- Created "Observaciones" section in Sub_extension.jrxml with title and borders

**Context/Notes:**
- MCP servers use deprecated @modelcontextprotocol/server-postgres but work correctly
- For dynamic subreport sizing: use `isRemoveLineWhenBlank="true"` on frame + `whenNoDataType="NoPages"` on subreport
- Subreport content must include borders/title - main report frame should only contain subreport element
- Use frames with `positionType="Float"` for elements that need to adjust based on content above
- Git push requires Eclipse (CLI auth not configured) - always pull before pushing
