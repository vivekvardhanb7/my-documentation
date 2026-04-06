# Project Overview

## What Is This?

The NAF Cloud Chatbot is an n8n-based conversational interface embedded in the NAF Cloud vending machine management platform. It allows users to query live sales data, manage machine insights, generate PDF reports, and get platform guidance — all through natural language or button-driven conversation.

---

## 1.1 Purpose

The system was designed and built to:

- Give **non-technical users** (admins, employees, franchise owners) instant access to live vending machine data through plain language questions.
- **Replace manual report generation** with automated, AI-driven PDF and analytics reports.
- Serve **role-based menus and answers** so each user only sees what they are permitted to access.
- Provide a **knowledge base assistant** for guidance on how to use the platform.

---

## 1.2 Webhook Entry Point

The entire workflow is triggered by a single POST webhook:

```
POST /webhook/chatbot-complete
```

This is the **only entry point**. The React frontend sends all chat messages to this endpoint with user context in the request body.

---

## 1.3 Request Payload Structure

| Field | Type | Description |
|-------|------|-------------|
| `message` / `chatInput` | string | The user's typed message or selected button text |
| `userAuthority` / `userRole` | string | User's role (`ROLE_ADMIN`, `ROLE_SUPER_ADMIN`, `Employee`, etc.) |
| `userName` | string | Display name of the logged-in user |
| `franchiseId` | string/number | The franchise the user belongs to — used for all DB filtering |
| `selectedMenu` | string | Menu item clicked by the user (e.g. `'Manage'`, `'Reports'`) |
| `selectedIntent` | string | Sub-item / specific question clicked by the user |
| `sessionId` | string | Unique session ID for conversation memory |
| `action` | string | `'init'` — triggers the initial welcome menu on app load |

---

*← [Back to Home](Home) | Next: [System Architecture](System-Architecture) →*
