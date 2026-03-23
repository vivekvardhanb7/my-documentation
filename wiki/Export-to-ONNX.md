# 📤 Export to ONNX

## Export Code

```python
import os
from ultralytics import YOLO
from google.colab import files

# Load the best checkpoint from augmented training run
weights_path = '/content/runs/detect/yolo_box_model_augmented/weights/best.pt'
if not os.path.exists(weights_path):
    weights_path = '/content/runs/detect/yolo_box_model/weights/best.pt'

print(f"Loading weights for export: {weights_path}")
export_model = YOLO(weights_path)

print("Exporting to ONNX...")
onnx_path = export_model.export(format='onnx', dynamic=True)   # ← dynamic=True is new

print(f"Success! Exported to: {onnx_path}")
files.download(onnx_path)   # downloads best.onnx to local machine
```

---

## Export Details

| Property | Value |
|----------|-------|
| Input shape | (1, 3, 640, 640) — BCHW |
| Output shape | (1, 6, 8400) |
| ONNX opset | 20 |
| Dynamic axes | **Yes** (`dynamic=True`) — supports variable input sizes |
| Source checkpoint | `best.pt` (49.6 MB) |
| Output ONNX file | `best.onnx` (**99.1 MB**) |
| Export time | ~7.2 s |
| Optimized with | `onnxslim` 0.1.90 |

---

## Saved Files

```
/content/runs/detect/yolo_box_model_augmented/weights/
├── best.pt       ← PyTorch checkpoint (52.0 MB)
├── last.pt       ← Last epoch checkpoint (52.0 MB)
└── best.onnx     ← ONNX export (99.1 MB, dynamic)
```

---

## Auto-Installed Dependencies

```
onnx>=1.12.0,<2.0.0
onnxslim>=0.1.71
onnxruntime-gpu
```

---

## CLI Usage (after export)

```bash
# Run predictions with the ONNX model
yolo predict task=detect model=/content/runs/detect/yolo_box_model_augmented/weights/best.onnx imgsz=640

# Validate the ONNX model
yolo val task=detect model=/content/runs/detect/yolo_box_model_augmented/weights/best.onnx \
         imgsz=640 data=/content/boxes-2/data.yaml
```

---

## Visualize the ONNX Graph

Open the exported `best.onnx` at [https://netron.app](https://netron.app) to inspect the computation graph interactively.
