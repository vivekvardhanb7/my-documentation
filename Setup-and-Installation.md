# ⚙️ Setup & Installation

## Overview
The notebook runs on **Google Colab** with a **Tesla T4 GPU** and requires two primary libraries: `ultralytics` (for YOLOv8) and `roboflow` (for dataset management).

---

## Step 1 — Install Dependencies

```python
print("Installing Ultralytics for YOLO...")
!pip install -q ultralytics roboflow
import ultralytics
ultralytics.checks()
```

**Output:**
```
Ultralytics 8.4.24 🚀 Python-3.12.12 torch-2.10.0+cu128 CUDA:0 (Tesla T4, 14913MiB)
Setup complete ✅ (2 CPUs, 12.7 GB RAM, 43.7/112.6 GB disk)
```

> **Note:** The `ultralytics.checks()` call verifies the environment and reports GPU availability, Python version, and disk space.

---

## Step 2 — Import Libraries

```python
import os
from ultralytics import YOLO
from roboflow import Roboflow
from google.colab import files
import matplotlib.pyplot as plt
from PIL import Image
```

| Import | Purpose |
|--------|---------|
| `YOLO` | Load, train, predict with YOLOv8 models |
| `Roboflow` | Authenticate and download dataset |
| `files` | Upload/download files in Colab |
| `matplotlib.pyplot` | Visualize detection results |
| `PIL.Image` | Image handling |

---

## Environment Summary

| Property | Value |
|----------|-------|
| Python | 3.12.12 |
| PyTorch | 2.10.0+cu128 |
| CUDA Device | Tesla T4 |
| VRAM | 14,913 MiB |
| RAM | 12.7 GB |
| Ultralytics | 8.4.24 |

---

## Additional Packages (Auto-installed at ONNX Export)
When exporting to ONNX, the following are automatically installed if missing:

```
onnx==1.20.1
onnxruntime-gpu==1.24.4
onnxslim==0.1.89
colorama==0.4.6
```
