# Known Issues

Document bugs, gotchas, workarounds, and debugging tips here.

## Format
```
### Issue: [Brief description]
**Symptoms:**
- What happens

**Cause:**
- Why it happens

**Workaround/Solution:**
- How to fix it

**Date discovered:** YYYY-MM-DD
```

---

## General Gotchas

### Logo Overlapping Text
**Symptoms:**
- Company logo overlaps invoice text in some reports

**Cause:**
- Fixed positioning without proper spacing calculation

**Workaround/Solution:**
- Recent commit (23a2b66) addressed this - check positioning in band elements

**Date discovered:** 2026-02-08 (from git history)

---

### Subreport Compilation Order
**Symptoms:**
- Main report fails to render with "subreport not found" error

**Cause:**
- .jasper files must exist before compiling main report
- Main reports reference compiled .jasper (not .jrxml) subreports

**Workaround/Solution:**
- Always compile subreports (Sub_*.jrxml) BEFORE compiling main reports
- Keep .jasper files committed in git to ensure version sync

**Date discovered:** 2026-02-08 (architecture analysis)
