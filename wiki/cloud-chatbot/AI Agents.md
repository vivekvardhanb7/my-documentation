# AI Agents

All AI agents in this workflow are powered by **Google Gemini** via the n8n Langchain integration.

---

## 4.1 INTENT CLASSIFIER2

Runs for all **free-text messages** (`direct_question` type). It reads the user's message and outputs one of three intents:

| Intent | Meaning | Next Step |
|--------|---------|-----------|
| `DATABASE` | User wants live data from MySQL (sales, transactions, users, etc.) | Route to `Check for Blocked Words` → `SQL AGENT1` |
| `KNOWLEDGE_BASE` | User wants help/instructions from docs (how-to, spare parts, guide) | Route to `KB AGENT1` |
| `OTHER` | Message is unclear or missing required context (e.g. date) | Route to `Format Response3` with clarification message |

### ⚠️ Critical Gatekeeper Rule

Before classifying as `DATABASE`, the classifier checks if the request needs a time period (month/year). If it does and no date is provided, it classifies as `OTHER` and asks the user which month/year they want.

### Session Memory

The classifier uses **Simple Memory2** (context window: 3 turns). If a user answers `'January'` to a follow-up question, it correctly maps this to the original database request.

---

## 4.2 SQL AGENT (Direct Chat)

The main analytics AI agent. It is backed by Google Gemini and has access to a MySQL tool. Given the user's question and franchise ID, it:

- Generates the correct MySQL `SELECT` query from a library of ~65 validated reference queries.
- Executes the query via the **MySQL Tool** node.
- Returns a structured JSON response with a `layout` array of visual components (`kpi_card`, `bar_chart`, `table`, `leaderboard`, etc.) that the frontend renders.
- Can also detect if the user wants a PDF/report, returning `report: true` with `columns` and `rows` only.

### Security

The classifier **blocks** any prompt containing `DELETE`, `UPDATE`, `INSERT`, `DROP`, `ALTER`, or `TRUNCATE` keywords before reaching this agent.

---

## 4.3 SQL AGENT1 (PDF Report Generation)

A separate, specialised Gemini agent that generates comprehensive multi-column SQL queries specifically for PDF reports.

Unlike `SQL AGENT` (which focuses on visual dashboard data), this one **always**:

- Returns a minimum of **6 columns**.
- Includes `grossAmount`, `netAmount`, `taxAmount`, and `totalTransactions` in every financial query.
- Appends a **GRAND TOTAL row** using `UNION ALL`.
- Never collapses data by grouping on a single time dimension.

---

## 4.4 KB AGENT1 (Knowledge Base Agent)

Answers how-to and informational questions by querying the PostgreSQL knowledge base using a fuzzy search tool.

| Feature | Detail |
|---------|--------|
| Search method | Full-text search + trigram similarity (`pg_trgm`) |
| Role filtering | Results filtered by the user's role — restricted content is never shown |
| Multilingual | Responds in the user's detected language |
| Response format | Same JSON structure as SQL Agent for consistent frontend rendering |

---

## 4.5 AI Agent1 (Report Title Generator)

A lightweight Gemini agent used **only during PDF generation**. Given the user's original question, it extracts and returns a clean, professional title to use as the PDF filename and heading.

**Example:** `'Show me sales today'` → `'Sales Report Today'`

---

*← [Key Nodes & Components](Key-Nodes-and-Components) | Next: [PDF Generation](PDF-Generation) →*
