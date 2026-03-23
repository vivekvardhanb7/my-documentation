# 📤 Export to ONNX

## Export Code

```python
print("Exporting to ONNX...")
path = model.export(format='onnx')
print(f"Success! Exported to: {path}")
files.download(path)   # downloads best.onnx to local machine
```

---

## Export Details

| Property | Value |
|----------|-------|
| Input shape | (1, 3, 640, 640) — BCHW |
| Output shape | (1, 6, 8400) |
| ONNX opset | 20 |
| Source checkpoint | `best.pt` (6.0 MB) |
| Output ONNX file | `best.onnx` (11.7 MB) |
| Export time | ~10.1 s |
| Optimized with | `onnxslim` 0.1.89 |

---

## Saved Files

```
/content/runs/detect/yolo_box_model/weights/
├── best.pt       ← PyTorch checkpoint (6.0 MB)
├── last.pt       ← Last epoch checkpoint
└── best.onnx     ← ONNX export (11.7 MB)
```

---

## Auto-Installed Dependencies

The following packages are installed automatically during export if missing:

```
onnx>=1.12.0,<2.0.0
onnxslim>=0.1.71
onnxruntime-gpu
```

> ⚠️ After auto-update, Ultralytics recommends restarting the runtime or re-running the export command for the updates to take effect.

---

## CLI Usage (after export)

```bash
# Run predictions with the ONNX model
yolo predict task=detect model=/content/runs/detect/yolo_box_model/weights/best.onnx imgsz=640

# Validate the ONNX model
yolo val task=detect model=/content/runs/detect/yolo_box_model/weights/best.onnx \
         imgsz=640 data=/content/boxes-2/data.yaml
```

---

## Visualize the ONNX Graph

Open the exported `best.onnx` at [https://netron.app](https://netron.app) to inspect the computation graph interactively.
