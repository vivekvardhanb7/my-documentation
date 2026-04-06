# Security and Access Control

---

## 8.1 Role-Based Access

- Every menu item, Q&A entry, and SQL query is filtered by the user's role.
- The `allowed_roles` column in the knowledge base controls who can see each answer.
- The main menu items are hard-coded per role in `Generate Main Menu1`.
- The AI agent always receives the `franchiseId` and uses it as a **mandatory filter** on all transaction queries.

### Role Hierarchy

```
Super Admin  →  Full access (all menus + Franchise management)
    │
Admin        →  Full access except Franchise menu
    │
Employee     →  Limited (Home, Manage without Users, Maintenance)
```

---

## 8.2 SQL Injection Protection

### Blocked Keywords

The `Check for Blocked Words` node blocks any message containing these keywords **before it reaches the SQL agent**:

| Blocked Keywords |
|-----------------|
| `DELETE` |
| `UPDATE` |
| `INSERT` |
| `DROP` |
| `ALTER` |
| `TRUNCATE` |
| `COPY` |
| `GRANT` |
| `REVOKE` |

### Additional Protections

- All AI-generated queries are `SELECT`-only by instruction enforced in the system prompt.
- The SQL agents are explicitly instructed **never to generate DML** (data-modifying) statements.

---

## 8.3 Data Privacy

| Rule | Detail |
|------|--------|
| **Franchise isolation** | `franchise_id` is never exposed in any response — used only internally for DB filtering |
| **ID exclusion** | Internal IDs and system identifiers are excluded from all summaries and responses |
| **Data scoping** | The chatbot only returns data belonging to the user's own franchise |
| **AI instruction** | All AI agents are instructed never to reveal franchise IDs, internal IDs, or database field names in their responses |

---

*← [Response Format](Response-Format) | Next: [Multilingual Support](Multilingual-Support) →*
