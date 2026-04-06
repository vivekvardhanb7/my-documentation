# Data Sources and Databases

---

## 6.1 PostgreSQL — Knowledge Base

Used for the **standard Q&A chatbot** (menu-driven conversations).

- **Main table:** `chat_knowledge_base`
- **Query style:** Parameterised placeholders (`$1`, `$2`) via the n8n Postgres node
- **Scheduled refresh:** A `Schedule Trigger1` runs every 2 days at 1 AM to keep the knowledge base content fresh

---

## 6.2 MySQL — Transactional Data

Used for all **analytics, reporting, and live data queries**. Contains all operational tables:

| Table Group | Tables |
|-------------|--------|
| **Transactions** | `transactions`, `transaction_product` |
| **Machines** | `vending_machines`, `machine_config`, `machine_failures`, `temperature_humidity`, `temperature_readings` |
| **Products** | `products`, `categories`, `tags`, `product_tags` |
| **Users & Franchises** | `user`, `franchise` |
| **Promotions** | `coupons`, `coupon_vending_machines`, `discount`, `vending_machine_discount` |
| **Advertising** | `advertisement`, `advertisement_media_mapping`, `advertisement_vending_machine_ids` |
| **Membership** | `membership_cards`, `membership_transactions` |
| **Tax** | `tax_details`, `tax_report` |

---

## 6.3 External API — Monthly PDF Reports

The NAF staging API is called for monthly transaction PDF reports:

```
GET https://staging-api.naf-cloudsystem.de/api/andriod/download-pdf
    ?franchiseId=X&month=YYYY-MM
```

This returns a **binary PDF file** directly, which is forwarded to the frontend.

---

## 6.4 PDF Conversion Service — Gotenberg

Gotenberg is an open-source headless Chrome PDF service used to convert HTML reports to PDF:

```
POST https://demo.gotenberg.dev/forms/chromium/convert/html
```

- HTML is submitted as multipart form data
- Response is a binary PDF file

> ⚠️ For production, replace with a privately hosted Gotenberg instance.

---

*← [PDF Generation](PDF-Generation) | Next: [Response Format](Response-Format) →*
