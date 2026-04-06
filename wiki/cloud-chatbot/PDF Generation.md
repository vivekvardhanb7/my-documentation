# PDF Generation

There are three distinct PDF generation pipelines depending on where the request originates.

---

## 5.1 Standard Report PDF (SQL-Driven)

Triggered when a user selects a report question from the **Reports section** (non-tax).

### Flow

```
SQL AGENT1
    │ Generates comprehensive SELECT query
    ▼
Execute SQL Query (MySQL)
    │
    ▼
Has Transactions?2
    ├── No data → Return 'NO DATA' response
    └── Data found ↓
    ▼
Merge (SQL data + AI-generated title from Report title node)
    │
    ▼
Generate PDF (builds styled HTML table)
    │
    ▼
IF node (checks for HTML generation errors)
    │
    ▼
HTTP Request → Gotenberg (demo.gotenberg.dev)
    │ Converts HTML to PDF
    ▼
Format PDF Response
    │ Encodes as base64 data URI
    ▼
Respond to Webhook1
```

---

## 5.2 Monthly Tax PDF (API-Driven)

When the user asks for a **monthly tax or transaction report**, this dedicated pipeline runs.

### Flow

```
Extract Month1
    │ Parses month/year from user message
    │ Supports: 'January', 'jan', 'last month', specific year
    ▼
Check Transactions Exist (MySQL COUNT)
    │
    ▼
Has Transactions?
    ├── No data → Friendly 'no data for this period' message
    └── Data found ↓
    ▼
Call Monthly Report API1
    │ GET https://staging-api.naf-cloudsystem.de/api/andriod/download-pdf
    │     ?franchiseId=X&month=YYYY-MM
    │ Returns pre-built binary PDF
    ▼
Format Monthly PDF Response1
    │ Wraps PDF in expected JSON structure (pdf_download layout type)
    ▼
Respond to Webhook1
```

---

## 5.3 Dynamic Chat PDF (From Direct Chat)

When a user in **direct chat (free text)** asks for a report or mentions `'report'`, `'pdf'`, or `'download'`:

### Flow

```
SQL AGENT (chat agent)
    │ Detects report keyword → returns report: true with columns + rows
    ▼
Check Report Intent
    │ Verifies the word 'report' is in the message
    ▼
Parse Response and Generate HTML
    │ Builds full styled HTML report page
    ▼
Convert to PDF (Gotenberg)
    │
    ▼
Format PDF Response1
    │ Returns base64 PDF to frontend
    ▼
Respond to Webhook2
```

---

## PDF Conversion Service

All HTML-to-PDF conversion uses **Gotenberg** (open-source headless Chrome PDF service):

```
POST https://demo.gotenberg.dev/forms/chromium/convert/html
```

HTML is submitted as multipart form data. The response is a binary PDF file.

> ⚠️ **Production Note:** The current setup uses the **public demo server**. For production, deploy a **private Gotenberg instance**.

---

*← [AI Agents](AI-Agents) | Next: [Data Sources & Databases](Data-Sources-and-Databases) →*
