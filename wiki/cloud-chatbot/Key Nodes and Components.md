# Key Nodes and Components

---

## 3.1 Detect Message Type1

This is the **brain of the routing layer**. It runs immediately after the webhook and determines what the user wants. It outputs one of these `type` values:

| Output Type | Triggered When | Routed To |
|-------------|---------------|-----------|
| `show_menu` | Empty message, `'init'` action, or greetings (hi, hello, etc.) | `Generate Main Menu1` |
| `show_questions` | User clicks a menu item (`selectedMenu` set, no `selectedIntent`) | `Map Context1 → Postgres` |
| `answer_intent` | User clicks a specific Q&A button (`selectedIntent` set) | `Map Context1 → Postgres` |
| `direct_question` | User types a free-text question (not a button click) | `INTENT CLASSIFIER2` |
| `show_report_categories` | User clicks 'Reports' or 'View' from the main menu | `Route Request (Reports)` |
| `show_report_questions` | User clicks a report sub-type (Today, Weekly, Monthly, etc.) | `Route Request (Reports)` |
| `answer_report` | User clicks a specific report question | `Route Request (Reports)` |

---

## 3.2 Role Handling

The node reads the raw role from the API and maps it to a clean internal role:

| Raw Role Value | Internal Role |
|----------------|---------------|
| Anything containing `'super'` | Super Admin |
| Anything containing `'admin'` | Admin |
| Anything containing `'employee'` | Employee |
| Anything else / missing | Employee (default) |

---

## 3.3 Generate Main Menu1

Returns a role-specific menu structure. Each role sees a different set of navigation items:

| Role | Available Menu Items |
|------|---------------------|
| **Super Admin** | Home, View (Sales Analytics, Transactions, Reports), Manage (full), Taxes, Maintenance, Franchise, Other |
| **Admin** | Home, View (Sales Analytics, Transactions, Reports), Manage (full), Taxes, Maintenance, Other |
| **Employee** | Home, Manage (no Users), Maintenance, Other |

---

## 3.4 Standard Q&A Pipeline (Postgres-Based)

When a user clicks a menu item or a question button, this pipeline runs:

1. **`Map Context1`** — converts the incoming menu name to a module key (e.g. `'Manage'` → `'machines'`) and extracts role, query, and request type.
2. **`Postgres: Get Module Data1`** — fetches relevant Q&A rows from the `chat_knowledge_base` table filtered by module and intent.
3. **`Process Logic (Standard)`** — filters rows by `allowed_roles`, then returns either a questions list or a text answer based on the request type.

### `chat_knowledge_base` Table Schema

| Column | Description |
|--------|-------------|
| `intent` | Unique ID for each Q&A item (e.g. `'view_sales_report'`) |
| `button_label` | Text shown on the button in the chat UI |
| `answer` | The HTML answer displayed when the user clicks the question |
| `allowed_roles` | Array of roles that can see this answer |
| `triggers` | Keywords that can match the question via free-text |
| `module` | Which section this belongs to (home, machines, inventory, etc.) |

---

## 3.5 Reports Pipeline

Triggered when a user navigates to the Reports section. The `Route Request (Reports)` node splits flow into three branches:

| Branch | Trigger | Action |
|--------|---------|--------|
| **Branch 0** — `show_report_categories` | User opens Reports | Returns report type menu: Today, Weekly Reports, Monthly Reports, Transactions, Taxes |
| **Branch 1** — `show_report_questions` | User selects a report category | Returns pre-defined questions for that category |
| **Branch 2** — `answer_report` | User clicks a specific report question | Routes through `Is Tax Related?` node |

- **Tax-related questions** → Monthly Tax PDF pipeline
- **Non-tax report questions** → SQL Agent + PDF generation pipeline

---

*← [System Architecture](System-Architecture) | Next: [AI Agents](AI-Agents) →*
