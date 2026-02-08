# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Environment

Reports are edited using **TIBCO Jaspersoft Studio 6.17.0** on:
- Kubuntu 22.04
- Ubuntu 25.04
- Windows 11

## Overview

This repository contains JasperReports (.jrxml) templates for a multi-country electronic invoicing and payroll system built on top of **Adempiere ERP**. Reports target **El Salvador** (COFIA electronic invoicing), **Chile**, and **Honduras**, using PostgreSQL databases with both relational SQL and JSONB data sources.

## No Build System

There is no build tool, test framework, or linter. Reports are `.jrxml` (JasperReports XML source) files compiled into `.jasper` (binary) files by JasperReports/Jaspersoft Studio. The `.xml` files are JDBC data source adapter configurations pointing to PostgreSQL databases.

## Architecture

### Report Categories

- **COFIA_\*.jrxml** — El Salvador electronic invoicing (DTE). "COFIA" = Comprobante Fiscal por Internet Autorizado. Subtypes:
  - `CCFF` = Comprobante de Crédito Fiscal (tax credit voucher)
  - `ConsumidorFinal` = final consumer invoice
  - `FacturaExportacion` = export invoice
  - `NotaCred` = credit note
  - `MainQRCode` suffix = includes QR code for tax authority verification
- **inf_HR_\*.jrxml** — Human Resources / payroll reports (payment receipts, employment verification, monthly payroll concepts)
- **Print_OdC\*.jrxml / Print_OrdenDeVenta.jrxml** — Purchase orders (Orden de Compra) and sales orders
- **rpt_SalesInvoice_80_mm_\*.jrxml** — POS thermal printer (80mm) invoice formats
- **shw_LibroDeCompras\*.jrxml** — Purchase book (accounting ledger)

### Subreport Composition

Main COFIA reports compose reusable `Sub_*.jasper` subreports, connected via the `RECORD_ID` parameter:

| Subreport | Purpose |
|-----------|---------|
| `Sub_emisor` | Issuer/company details (extracted from JSON `emisor` field) |
| `Sub_receptor` / `Sub_receptor_FE` / `Sub_receptor_ConsumidorFinal` | Receiver/customer details (from JSON `receptor` field) |
| `Sub_identificador` | Document identification + QR code generation |
| `Sub_Resumen` / `Sub_Resumen_FE` | Totals/summary section |
| `Sub_MontoLetras` / `Sub_ResumenMontoLetras` | Amount in words (letras) |
| `Sub_docsrelacionados` | Related documents (used by credit notes) |
| `Sub_QRCode` | QR code rendering |
| `FE_Subreport1` | General electronic invoice support |

HR reports use `inf_HR_PaymentReceipt_SubReport.jasper` and `inf_HR_PaymentReceipt_SubReportNP.jasper`.

### Data Sources

- **SQL (PostgreSQL)** — Primary. Queries against Adempiere tables: `C_Invoice`, `C_InvoiceLine`, `C_Order`, `C_BPartner`, `HR_*` tables.
- **JSON** — `COFIACCFF_Json.jrxml` uses JasperReports JSON query language against JSONB columns stored in:
  - `e_electronicInvoice_Json` — full electronic invoice JSON data
  - `Json_CuerpoDocumento` — invoice line items in JSON
  - `JSON_Encabezado` — header data in JSON
- **Groovy** — Used in HR reports for scripting calculations.

### Database Connection Configs (.xml files)

Each `.xml` file defines a JDBC PostgreSQL adapter for a specific environment (all use driver `org.postgresql.Driver`, user `adempiere`):

| File | Database / Environment |
|------|----------------------|
| `COFIA_Local.xml` / `CofiaLocal.xml` | COFIA on localhost |
| `Chile_Local.xml` / `adempiereChile_Local.xml` | Chile on localhost |
| `AdempiereChile_remote.xml` | Chile remote |
| `SAPROD_Local.xml` / `SAPRODLocal.xml` | SAP production |
| `SA_Remote.xml` | Remote adempiere (192.168.0.94) |
| `PimentelLokal.xml` | Client-specific (Pimentel) |

## Team Shared Memory

The `.claude-memory/` directory contains shared knowledge across team members:
- **recent-work.md** — Latest changes, ongoing work, current context
- **known-issues.md** — Bugs, gotchas, workarounds discovered
- **learned-patterns.md** — Best practices, tips for JasperReports/Jaspersoft

**When to update:**
- After solving a non-trivial problem
- When discovering database quirks or JSONB patterns
- After significant changes to report structure
- When finding workarounds for Jaspersoft Studio issues

Always check these files at the start of work for recent team context.

## Key Conventions

- Main COFIA reports reference compiled subreports by `.jasper` extension (not `.jrxml`), so subreports must be compiled before the main report renders.
- The `RECORD_ID` parameter is the primary key passed from the calling Adempiere application to select which invoice/document to print.
- JSON fields are extracted from PostgreSQL JSONB columns using `::jsonb` casting and `->>` operators within SQL queries, then passed to subreports.
- Report language is primarily Spanish; commit messages mix German ("Anpassungen") and English.
- Both `.jrxml` and `.jasper` files are committed to git to maintain version synchronization and avoid compilation mismatches.
