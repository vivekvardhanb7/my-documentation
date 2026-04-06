# System Architecture

The workflow is split into multiple parallel pipelines, each responsible for one type of user interaction. All pipelines start at the webhook and end at a **Respond to Webhook** node.

---

## 2.1 High-Level Flow

| Stage | Node | What It Does |
|-------|------|--------------|
| **Entry** | `Chat Webhook1` | Receives POST request from the frontend |
| **Pre-processing** | `Detect Message Type1` | Reads the payload, normalises the role, determines the intent type |
| **Routing** | `Route Request (Main)` | Splits flow into: Menu, Questions, Direct Chat, or Reports |
| **Menu Pipeline** | `Generate Main Menu1` | Returns role-specific navigation menu to the user |
| **Questions Pipeline** | `Map Context1 → Postgres → Process Logic` | Fetches and filters Q&A from knowledge base by role and module |
| **Reports Pipeline** | `Route Request (Reports) → Report Nodes` | Shows report categories and sub-questions |
| **Direct Chat / AI** | `INTENT CLASSIFIER2 → SQL or KB Agent` | Classifies free-text and routes to the right AI agent |
| **Response** | `Respond to Webhook 1/2/3` | Sends JSON back to the frontend to render |

---

## 2.2 Pipeline Diagram

```
POST /webhook/chatbot-complete
          │
          ▼
  Detect Message Type1
          │
    ┌─────┴──────────────────────────────────────┐
    │            │              │                 │
    ▼            ▼              ▼                 ▼
 show_menu  show_questions  direct_question  show_report_*
    │            │              │                 │
    ▼            ▼              ▼                 ▼
Generate    Map Context1   INTENT           Route Request
Main Menu1  → Postgres    CLASSIFIER2       (Reports)
    │        → Process        │                 │
    │          Logic      ┌───┴────┐       ┌────┴────┐
    │                     │        │       │         │
    │                  SQL AGENT  KB    Report    Tax PDF
    │                  (Gemini)  AGENT   PDFs    Pipeline
    │
    ▼
Respond to Webhook 1 / 2 / 3
```

---

## 2.3 The Three Response Webhooks

Three different **Respond to Webhook** nodes exist because some pipelines are completely independent sub-flows. They all respond to the same incoming webhook:

| Node | Used By |
|------|---------|
| `Respond to Webhook1` | Menu, standard Q&A, report menus, PDF generation (old path), error handling |
| `Respond to Webhook2` | Direct chat (SQL Agent, KB Agent), PDF download responses |
| `Respond to Webhook3` | Intent classifier `OTHER` fallback (when the user's message is incomplete or unclear) |

---

*← [Project Overview](Project-Overview) | Next: [Key Nodes & Components](Key-Nodes-and-Components) →*
