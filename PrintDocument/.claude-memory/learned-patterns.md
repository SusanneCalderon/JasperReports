# Learned Patterns

Best practices, tips, and patterns discovered while working with JasperReports/Jaspersoft Studio.

## Format
```
### Pattern: [Name]
**Context:**
- When this applies

**Approach:**
- How to do it

**Example:**
- Code/file reference

**Date learned:** YYYY-MM-DD
```

---

## JasperReports Patterns

### JSONB Data Extraction Pattern
**Context:**
- COFIA reports store electronic invoice data in PostgreSQL JSONB columns
- Need to extract nested JSON fields for report display

**Approach:**
- Use PostgreSQL JSONB operators in SQL queries:
  - `::jsonb` for casting
  - `->>` for text extraction
  - `->` for nested object access
- Extract at SQL level and pass to subreports as parameters

**Example:**
See `Sub_receptor.jasper` - extracts receptor details from JSON
See `Sub_emisor.jasper` - extracts emisor details from JSON

**Date learned:** 2026-02-08

---

### Subreport Parameter Passing
**Context:**
- Main COFIA reports use modular subreports for reusability
- All subreports need access to the same invoice/document record

**Approach:**
- Use `RECORD_ID` as primary parameter from Adempiere
- Pass to all subreports consistently
- Subreports query database independently using RECORD_ID

**Example:**
All COFIA_*.jrxml main reports → Sub_*.jasper subreports

**Date learned:** 2026-02-08

---

## Database Patterns

### Multi-Environment Database Configs
**Context:**
- Same reports need to run against different database instances
- Local development, remote testing, production, client-specific

**Approach:**
- Separate .xml JDBC adapter files per environment
- All use same credentials (adempiere/adempiere) but different hosts
- Naming convention: `[Client]_[Environment].xml`

**Example:**
- COFIA_Local.xml → localhost COFIA database
- AdempiereChile_remote.xml → remote Chile instance
- PimentelLokal.xml → client-specific instance

**Date learned:** 2026-02-08

## JasperReports Layout Patterns (Added 2026-02-08)

### Dynamic Subreport Sizing (Zero Height When Empty)
**Context:**
- Need subreports that take 0 pixels when they have no data
- Common for optional sections like "Observaciones" or "Notes"

**Approach:**
1. In main report:
   - Wrap subreport in frame with `isRemoveLineWhenBlank="true"`
   - Frame should ONLY contain the subreport element (no static text/borders)
   - Use `positionType="Float"` so it adjusts based on content above
2. In subreport:
   - Set `whenNoDataType="NoPages"` on jasperReport element
   - Include title/borders INSIDE the subreport (not in main report)

**Example:**
See COFIA_CCFF_MainQRCode.jrxml (Sub_extension subreport in summary band)

**Date learned:** 2026-02-08

---

### Border Alignment Between Components
**Context:**
- Multiple frames/components stacked vertically need aligned borders
- Misaligned borders (even 1px) look unprofessional

**Approach:**
- Use same x-position and width for all vertically stacked components
- Common pattern: x="1" width="571" for full-width components
- Use frames with borders for consistency

**Example:**
COFIA_CCFF_MainQRCode.jrxml: Emisor/Receptor and orderdescription aligned at x="1" width="571"

**Date learned:** 2026-02-08

---

### Preventing Content Overlap from Stretching Subreports
**Context:**
- Subreports with stretchType="ContainerHeight" can extend beyond frame height

**Approach:**
- Add 10-15px vertical spacing after stretching frames
- Use positionType="Float" on following components

**Date learned:** 2026-02-08

---

### PostgreSQL MCP Server Setup
**Context:**
- Enable direct database queries in Claude Code for debugging

**Approach:**
```bash
npm install -g @modelcontextprotocol/server-postgres
claude mcp add [name] -- node [server-path] postgresql://user:pass@host:port/db
```

**Date learned:** 2026-02-08
