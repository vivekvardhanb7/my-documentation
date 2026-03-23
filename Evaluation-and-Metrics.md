# 📊 Evaluation & Metrics

## Evaluate Code

```python
trained_model = YOLO('/content/runs/detect/yolo_box_model/weights/best.pt')

print("Calculating Metrics on Validation Set...")
metrics = trained_model.val()
print(f"mAP@50-95: {metrics.box.map:.4f}")
print(f"mAP@50:    {metrics.box.map50:.4f}")
```

---

## Validation Results

### Overall

| Metric | Value |
|--------|-------|
| Images | 18 |
| Instances | 18 |
| Box Precision (P) | 0.812 |
| Recall (R) | 0.958 |
| **mAP@50** | **0.9950** |
| **mAP@50-95** | **0.9497** |

### Per-Class

| Class | Images | Instances | P | R | mAP@50 | mAP@50-95 |
|-------|--------|-----------|---|---|--------|-----------|
| `box` | 7 | 7 | 0.623 | 1.000 | 0.995 | 0.951 |
| `small_box` | 11 | 11 | 1.000 | 0.916 | 0.995 | 0.948 |

---

## Inference Speed (Validation Set)

| Step | Time |
|------|------|
| Preprocess | 7.4 ms/image |
| Inference | 14.2 ms/image |
| Loss | 0.0 ms/image |
| Postprocess | 1.6 ms/image |

> Results saved to `/content/runs/detect/val/`

---

## Metric Definitions

| Metric | Description |
|--------|-------------|
| **mAP@50** | Mean Average Precision at IoU threshold 0.50 |
| **mAP@50-95** | Mean AP averaged over IoU thresholds 0.50–0.95 (step 0.05) |
| **P (Precision)** | TP / (TP + FP) |
| **R (Recall)** | TP / (TP + FN) |

---

## Model Summary (Fused)

```
Model summary (fused): 73 layers, 3,006,038 parameters, 0 gradients, 8.1 GFLOPs
```
