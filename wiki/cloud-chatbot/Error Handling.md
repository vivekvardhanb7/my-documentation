# Error Handling

---

## Error Scenarios

| Error Scenario | How It Is Handled |
|---------------|-------------------|
| PostgreSQL query fails | `onError: continueErrorOutput` → routed to `Error Handling` node |
| Monthly PDF API call fails | `onError: continueErrorOutput` → routed to `Error Handling` node |
| HTML-to-PDF (Gotenberg) fails | `onError: continueErrorOutput` → routed to `Error Handling` node |
| No data found for SQL query | `Has Transactions?2` detects empty result → returns `'NO DATA'` response |
| No data for monthly PDF | `Has Transactions?` detects `txCount = 0` → returns friendly `'no data for this period'` message |
| User sends blocked SQL keyword | `Check for Blocked Words` blocks request → returns permission error message |
| Intent classifier returns `OTHER` | `Format Response3` formats the fallback message with suggestions |
| AI returns malformed JSON | `Code in JavaScript3` wraps in try/catch and falls back to `OTHER` intent |
| PDF generation has no binary data | `Format PDF Response` checks for missing data → returns error text to user |

---

## Standard Error Message

The `Error Handling` node returns this user-friendly message:

> *"We sincerely apologize, but there is currently an issue processing your request. Please contact support at support@naf-halsbach.de."*

---

## Error Flow Architecture

```
Any failing node
      │
      │ (onError: continueErrorOutput)
      ▼
Error Handling node
      │
      ▼
Respond to Webhook1
      │
      ▼
Frontend displays error message
```

---

*← [Multilingual Support](Multilingual-Support) | Next: [Node Inventory](Node-Inventory) →*
