# Response Format

All responses from the chatbot follow a consistent JSON structure that the React frontend reads to decide what to render.

---

## 7.1 Standard Chatbot Response

Used for menus, Q&A lists, and text answers:

```json
{
  "type": "questions_list" | "menu" | "text",
  "questions": [...],
  "menu": [...],
  "text": "..."
}
```

---

## 7.2 AI Agent / Analytics Response

Used for SQL Agent and KB Agent responses:

```json
{
  "output": "{
    \"summary\": \"...\",
    \"layout\": [...],
    \"suggestions\": [...]
  }"
}
```

---

## 7.3 Layout Component Types

The `layout` array inside the AI response can contain any of these visual component types:

| Type | Renders As |
|------|-----------|
| `kpi_card` | A single metric card with title, value, and trend indicator |
| `bar_chart` | Vertical bar chart for category comparison |
| `line_chart` | Line chart for trends over time |
| `pie_chart` / `donut_chart` | Pie or donut chart for percentage distributions |
| `table` | A data table with all rows and columns |
| `leaderboard` | Ranked list with gold/silver/bronze badges |
| `status_grid` | Grid of machine statuses (healthy/error/warning) |
| `gauge_chart` | Gauge dial for temperature or health scores |
| `pdf_download` | A downloadable PDF button with base64 data embedded |
| `text` / `text (layout)` | Plain text or HTML response |

---

## 7.4 PDF Response Structure

When a PDF is returned, the `pdf_download` layout type is used:

```json
{
  "output": "{
    \"layout\": [
      {
        \"type\": \"pdf_download\",
        \"data\": \"data:application/pdf;base64,...\"
      }
    ]
  }"
}
```

---

*← [Data Sources & Databases](Data-Sources-and-Databases) | Next: [Security & Access Control](Security-and-Access-Control) →*
