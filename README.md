# 🌐 Cloud System Documentation

Welcome to the comprehensive documentation for the Cloud System. This repository includes detailed guides and technical documentation for backend services (object detection), chatbot architecture, and cloud application help modules.

---


## 📁 Wiki Structure

```
wiki/
├── cloud-frontend/
├── cloud-backend/
├── cloud-chatbot/
├── cloud-help-documentation/
```

## 📚 Documentation Sections

- [Cloud Frontend](./cloud-frontend)
- [Cloud Backend](./cloud-backend)
- [Cloud Chatbot](./cloud-chatbot)
- [Cloud Help Documentation](./cloud-help-documentation)

---

## 📚 Backend Documentation (Object Detection)

| # | Page | Description |
|---|------|-------------|
| 1 | [⚙️ Setup & Installation](./cloud-backend/Setup-and-Installation.md) | Environment setup, library installation, GPU check |
| 2 | [📦 Dataset](./cloud-backend/Dataset.md) | Roboflow dataset download, splits, and class info |
| 3 | [🧠 Model Architecture](./cloud-backend/Model-Architecture.md) | YOLOv8n layers, parameters, and transfer learning |
| 4 | [🏋️ Training](./cloud-backend/Training.md) | Hyperparameters and all 50-epoch metrics |
| 5 | [📤 Export to ONNX](./cloud-backend/Export-to-ONNX.md) | Exporting the trained model to ONNX format |
| 6 | [📊 Evaluation & Metrics](./cloud-backend/Evaluation-and-Metrics.md) | mAP@50, mAP@50-95, per-class results |
| 7 | [📷 Live Webcam Inference](./cloud-backend/Webcam-Inference.md) | Real-time detection via Colab webcam |
| 8 | [🖼️ Image Inference](./cloud-backend/Image-Inference.md) | Running detection on uploaded static images |

---



## 🤖 Chatbot Documentation

| Page | Description |
|------|-------------|
| [Home](./cloud-chatbot/Home.md) | Entry point and overview of chatbot |
| [Project Overview](./cloud-chatbot/Project-Overview.md) | Purpose, entry point, and request payload structure |
| [System Architecture](./cloud-chatbot/System-Architecture.md) | High-level flow, pipelines, and response nodes |
| [Key Nodes & Components](./cloud-chatbot/Key-Nodes-and-Components.md) | Routing logic, menus, Q&A pipeline, reports pipeline |
| [AI Agents](./cloud-chatbot/AI-Agents.md) | Intent classifier, SQL agents, KB agent, title generator |
| [PDF Generation](./cloud-chatbot/PDF-Generation.md) | Standard, monthly tax, and dynamic chat PDF pipelines |
| [Data Sources & Databases](./cloud-chatbot/Data-Sources-and-Databases.md) | PostgreSQL, MySQL, external API, Gotenberg |
| [Response Format](./cloud-chatbot/Response-Format.md) | JSON structure, layout component types |
| [Security & Access Control](./cloud-chatbot/Security-and-Access-Control.md) | Role-based access, SQL injection protection, data privacy |
| [Multilingual Support](./cloud-chatbot/Multilingual-Support.md) | Language detection and behaviour |
| [Error Handling](./cloud-chatbot/Error-Handling.md) | All error scenarios and fallbacks |
| [Node Inventory](./cloud-chatbot/Node-Inventory.md) | Complete list of all n8n nodes |
| [Developer Notes](./cloud-chatbot/Developer-Notes.md) | Critical rules for developers |

---

## 📘 Help Documentation

| Page | Description |
|------|-------------|
| [Home](./cloud-help-documentation/Home.md) | Overview of the cloud system and navigation guide |
| [Maintenance](./cloud-help-documentation/Maintenance.md) | System maintenance and settings management |
| [Manage](./cloud-help-documentation/Manage.md) | Managing users, data, and configurations |
| [Taxes](./cloud-help-documentation/Taxes.md) | Tax-related information and usage |
| [View](./cloud-help-documentation/View.md) | Viewing reports, dashboards, and data |
| [Help](./cloud-help-documentation/Help.md) | Support, FAQs, and contact information |


## 🔧 Tech Stack

Backend · Chatbot · APIs · Databases · Cloud Services
