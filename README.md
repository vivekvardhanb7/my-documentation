# 📖 YOLOv8 Object Detection — Documentation

Welcome to the documentation for the **YOLOv8 Box Detection** project — a Google Colab notebook that trains, evaluates, and deploys a custom object detection model using Ultralytics YOLOv8.

---

## 🏆 Model Performance

| Metric | Value |
|--------|-------|
| mAP@50 | **0.9950** |
| mAP@50-95 | **0.9497** |
| Classes | `box`, `small_box` |
| Training Time | ~10 min (Tesla T4) |

---

## 📚 Documentation Pages

| # | Page | Description |
|---|------|-------------|
| 1 | [⚙️ Setup & Installation](wiki/Setup-and-Installation.md) | Environment setup, library installation, GPU check |
| 2 | [📦 Dataset](wiki/Dataset.md) | Roboflow dataset download, splits, and class info |
| 3 | [🧠 Model Architecture](wiki/Model-Architecture.md) | YOLOv8n layers, parameters, and transfer learning |
| 4 | [🏋️ Training](wiki/Training.md) | Hyperparameters and all 50-epoch metrics |
| 5 | [📤 Export to ONNX](wiki/Export-to-ONNX.md) | Exporting the trained model to ONNX format |
| 6 | [📊 Evaluation & Metrics](wiki/Evaluation-and-Metrics.md) | mAP@50, mAP@50-95, per-class results |
| 7 | [📷 Live Webcam Inference](wiki/Webcam-Inference.md) | Real-time detection via Colab webcam |
| 8 | [🖼️ Image Inference](wiki/Image-Inference.md) | Running detection on uploaded static images |

---

## 🚀 Quick Pipeline

```
Install Ultralytics + Roboflow  →  Download Dataset  →  Train YOLOv8n (50 epochs)
        →  Export to ONNX  →  Evaluate  →  Live Webcam / Image Inference
```

---

## 🔧 Tech Stack

`YOLOv8` · `Roboflow` · `ONNX Runtime` · `OpenCV` · `Google Colab (T4 GPU)`
