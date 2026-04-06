# Developer Notes

Critical rules and gotchas for developers working on the NAF Chatbot workflow.

---

## ⚠️ Tax Calculation

```
final_price  =  pre-tax base amount
Tax Amount   =  final_price × 0.07
Net Amount   =  final_price × 0.93
```

> ❌ **NEVER** divide by `1.07` to calculate tax — this is incorrect.

---

## ⚠️ Date Range Queries

Always use **half-open intervals** on `DATETIME` columns:

```sql
-- ✅ Correct
WHERE created_at >= '2024-01-01' AND created_at < '2024-01-02'

-- ❌ Never use BETWEEN on DATETIME (inclusive of end boundary)
WHERE created_at BETWEEN '2024-01-01' AND '2024-01-01'

-- ❌ Never use LAST_DAY()
WHERE created_at <= LAST_DAY('2024-01-01')
```

---

## ⚠️ Franchise Isolation

> Every MySQL query that touches `transactions` **must** include `franchise_id` as a `WHERE` filter.

```sql
-- ✅ Correct
SELECT * FROM transactions WHERE franchise_id = ? AND status <> 'Failed'

-- ❌ Missing franchise filter — never do this
SELECT * FROM transactions WHERE status <> 'Failed'
```

---

## ⚠️ Status Filtering

Different tables use different casing for the failed status:

| Table | Correct Filter |
|-------|---------------|
| `transactions` | `status <> 'Failed'` (capital F) |
| `transaction_product` | `status <> 'FAILED'` (all caps) |

---

## ⚠️ PDF Service (Gotenberg)

The current Gotenberg endpoint is the **public demo server**:

```
https://demo.gotenberg.dev/forms/chromium/convert/html
```

> 🚨 For **production**, deploy a **private Gotenberg instance**. The demo server should not be used for live data.

---

## ⚠️ Session Memory

Session memory (`Simple Memory2`, `Window Buffer Memory`, `Window Buffer Memory1`) is:

- **In-memory only** — not persisted to any database
- **Resets between sessions**
- Scoped to a single `sessionId`

---

## ⚠️ AI Response Privacy Rules

All AI agents are instructed to **never** reveal:

- Franchise IDs
- Internal IDs
- Database field names

Do not remove or weaken these instructions in system prompts.

---

## ℹ️ 'Other' Menu Item

The `Other` menu item always routes the user to an **external support page**:

```
https://vendinaf.com/de/support
```

---

## ℹ️ SQL Agent Query Library

The SQL AGENT is backed by a library of approximately **65 validated reference queries**. When updating or expanding analytics capabilities, add new validated queries to this library and update the agent's system prompt accordingly.

---

*← [Node Inventory](Node-Inventory) | [Back to Home](Home) →*
