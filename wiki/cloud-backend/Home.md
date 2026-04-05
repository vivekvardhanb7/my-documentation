# 🏠 Object Detection Model (YOLOv8m) — Wiki Home

Welcome to the project wiki for **Object Detection with YOLOv8** — a Google Colab notebook that trains, evaluates, exports, and runs live inference on a custom box-detection dataset using Ultralytics YOLOv8m.

---

## 📚 Wiki Pages

| Page | Description |
|------|-------------|
| [Setup & Installation](Setup-and-Installation.md) | Environment setup, library installation, GPU checks |
| [Dataset](Dataset.md) | Roboflow dataset download, structure, and class info |
| [Model Architecture](Model-Architecture.md) | YOLOv8m architecture, parameters, and pretrained weights |
| [Training](Training.md) | Training configuration, augmentations, and per-epoch metrics |
| [Export to ONNX](Export-to-ONNX.md) | Exporting the trained model to ONNX format |
| [Evaluation & Metrics](Evaluation-and-Metrics.md) | Validation metrics (mAP@50, mAP@50-95) per class |
| [Live Webcam Inference](Webcam-Inference.md) | Real-time detection using ONNX model via Colab webcam |
| [Image Inference](Image-Inference.md) | Running detection on uploaded static images |

---

## 🚀 Quick Overview

```
Install Ultralytics + Roboflow
        ↓
Download & Verify Dataset (Roboflow: boxes-2, 662 images)
        ↓
Initialize YOLOv8m (pretrained, 25.8M params)
        ↓
Train for 100 epochs (GPU T4, imgsz=640, advanced augmentations)
        ↓
Export to ONNX (dynamic=True, 99.1 MB)
        ↓
Evaluate on Validation Set
        ↓
Live Webcam / Static Image Inference
```

---

## 🏷️ Classes

| ID | Class |
|----|-------|
| 0  | `box` |
| 1  | `small_box` |

---

## 🏆 Final Model Performance

| Metric | Value |
|--------|-------|
| mAP@50 | **0.9881** |
| mAP@50-95 | **0.9470** |

---

## 🔗 Key Technologies

- **Ultralytics YOLOv8m** — `yolov8m.pt` (medium model, 25.8M params)
- **Roboflow** — Dataset management and download
- **ONNX / ONNX Runtime** — Cross-platform model export (dynamic input)
- **OpenCV + NumPy** — Image/video processing
- **Google Colab** — GPU-accelerated training (Tesla T4)
