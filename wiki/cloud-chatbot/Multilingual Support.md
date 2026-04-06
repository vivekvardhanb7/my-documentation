# Multilingual Support

---

## How It Works

All three AI agents follow the same language rule:

> **Detect the language the user is writing in and always reply in that exact language.**

This applies to:
- Summaries
- Error messages
- Clarification questions
- All other responses

The language only changes if the user switches language in their message.

---

## Agents With Multilingual Support

| Agent | Supports Multilingual |
|-------|-----------------------|
| `INTENT CLASSIFIER2` | ✅ Yes |
| `SQL AGENT` | ✅ Yes |
| `KB AGENT1` | ✅ Yes |

---

## Example

If a user writes in **German**, all responses including follow-up questions and error messages will be in German.

```
User: "Zeig mir die heutigen Verkäufe"
Bot:  "Hier sind die heutigen Verkaufsdaten für Ihre Filiale..."
```

---

*← [Security & Access Control](Security-and-Access-Control) | Next: [Error Handling](Error-Handling) →*
