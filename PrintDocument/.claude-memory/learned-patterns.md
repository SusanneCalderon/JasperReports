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
