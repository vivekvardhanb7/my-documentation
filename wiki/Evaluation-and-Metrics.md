# 📊 Evaluation & Metrics

## Evaluation Code

```python
import os
from ultralytics import YOLO

weights_path = '/content/runs/detect/yolo_box_model_augmented/weights/best.pt'
if not os.path.exists(weights_path):
    weights_path = '/content/runs/detect/yolo_box_model/weights/best.pt'

print(f"Loading model for validation: {weights_path}")
trained_model = YOLO(weights_path)

print("Calculating Metrics on Validation Set...")
metrics = trained_model.val()
print(f"mAP@50-95: {metrics.box.map:.4f}")
print(f"mAP@50:    {metrics.box.map50:.4f}")
```

---

## Validation Results

```
Model summary (fused): 93 layers, 25,840,918 parameters, 0 gradients, 78.7 GFLOPs

val: Scanning /content/boxes-2/valid/labels.cache... 18 images, 0 backgrounds, 0 corrupt: 100%
```

### Summary Metrics

| Metric | Value |
|--------|-------|
| **mAP@50** | **0.9881** |
| **mAP@50-95** | **0.9470** |
| Precision | 0.9200 |
| Recall | 0.9550 |

### Per-Class Breakdown

| Class | Images | Instances | Precision | Recall | mAP@50 | mAP@50-95 |
|-------|--------|-----------|-----------|--------|--------|-----------|
| **all** | 18 | 18 | 0.920 | 0.955 | 0.988 | 0.947 |
| `box` | 7 | 7 | 0.840 | 1.000 | 0.995 | 0.924 |
| `small_box` | 11 | 11 | 0.999 | 0.909 | 0.981 | 0.970 |

---

## Inference Speed (on validation set)

| Phase | Time per image |
|-------|---------------|
| Preprocess | 5.2 ms |
| Inference | 35.7 ms |
| Loss | 0.0 ms |
| Postprocess | 0.8 ms |

> Benchmarked on Tesla T4 GPU (CUDA:0, 14913 MiB).

---

## End-of-Training Validation (Best Checkpoint)

```
Validating /content/runs/detect/yolo_box_model_augmented/weights/best.pt...
Model summary (fused): 93 layers, 25,840,918 parameters, 0 gradients, 78.7 GFLOPs

Class       Images  Instances  Box(P    R       mAP50  mAP50-95)
all              18        18  0.979    0.961   0.995     0.945
box               7         7  0.958    1.000   0.995     0.916
small_box        11        11  1.000    0.922   0.995     0.973

Speed: 0.2ms preprocess, 33.0ms inference, 0.0ms loss, 0.8ms postprocess per image
Results saved to /content/runs/detect/yolo_box_model_augmented
```

---

## Saved Results

All artifacts are under: `/content/runs/detect/yolo_box_model_augmented/`

Includes:
- Confusion matrix
- Precision–Recall curves
- F1-score curve
- Validation batch images with predictions
