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

---

### Text Field Clipping in QR Code Reports
**Symptoms:**
- Order number field displays "OVEC-00075-" instead of full "OVEC-00075-2026"
- Text appears cut off at the right edge

**Cause:**
- TextField width too narrow (140px) for dynamic content
- Center alignment doesn't prevent clipping

**Workaround/Solution:**
- Increase width (140 → 162px for order numbers)
- Adjust x-position if needed to keep centered
- See COFIA_CCFF_MainQRCode.jrxml line 481 (No Orden field)

**Date discovered:** 2026-02-08

---

### Frame with Static Content Always Visible
**Symptoms:**
- Frame with borders and title shows even when subreport is empty
- isRemoveLineWhenBlank="true" not working as expected

**Cause:**
- StaticText elements in frame prevent it from being "blank"
- Frame only removes when ALL content is empty/removed

**Workaround/Solution:**
- Move static titles/borders INTO the subreport
- Main report frame should only contain subreport element
- Subreport includes its own title band with borders
- Use whenNoDataType="NoPages" in subreport

**Date discovered:** 2026-02-08

---

### Git Push Requires Authentication
**Symptoms:**
- Command line git push fails with "could not read Username"
- Error: "No such device or address"

**Cause:**
- HTTPS remote requires authentication
- CLI not configured with credentials or tokens

**Workaround/Solution:**
- Use Eclipse for git push (has credentials configured)
- Alternatively: set up SSH keys or personal access token
- For team: always use Eclipse → Team → Push to Upstream

**Date discovered:** 2026-02-08
