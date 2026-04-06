# NAF Cloud System — Chatbot Workflow Wiki

> **Platform:** n8n (Workflow Automation) | **AI Engine:** Google Gemini (via Langchain) | **Version:** 1.0 | **Classification:** Internal / Confidential

Welcome to the NAF Cloud Chatbot documentation. This wiki covers the complete architecture, components, data sources, and operational details of the n8n-based chatbot embedded in the NAF Cloud vending machine management platform.

---

## 📚 Wiki Pages

| Page | Description |
|------|-------------|
| [Project Overview](Project-Overview) | Purpose, entry point, and request payload structure |
| [System Architecture](System-Architecture) | High-level flow, pipelines, and response nodes |
| [Key Nodes & Components](Key-Nodes-and-Components) | Routing logic, menus, Q&A pipeline, reports pipeline |
| [AI Agents](AI-Agents) | Intent classifier, SQL agents, KB agent, title generator |
| [PDF Generation](PDF-Generation) | Standard, monthly tax, and dynamic chat PDF pipelines |
| [Data Sources & Databases](Data-Sources-and-Databases) | PostgreSQL, MySQL, external API, Gotenberg |
| [Response Format](Response-Format) | JSON structure, layout component types |
| [Security & Access Control](Security-and-Access-Control) | Role-based access, SQL injection protection, data privacy |
| [Multilingual Support](Multilingual-Support) | Language detection and behaviour |
| [Error Handling](Error-Handling) | All error scenarios and fallbacks |
| [Node Inventory](Node-Inventory) | Complete list of all n8n nodes |
| [Developer Notes](Developer-Notes) | Critical rules for developers |

---

## 🔑 Quick Reference

- **Webhook entry point:** `POST /webhook/chatbot-complete`
- **AI model:** Google Gemini (all agents)
- **Databases:** PostgreSQL (Knowledge Base), MySQL (Transactional)
- **PDF service:** Gotenberg (`demo.gotenberg.dev`)
- **Support email:** support@naf-halsbach.de
- **Support page:** https://vendinaf.com/de/support
