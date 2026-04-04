# 🧠 Model Architecture

## Initialization

```python
from ultralytics import YOLO
model = YOLO('yolov8m.pt')
```

The **YOLOv8m** (medium) pretrained model is used and fine-tuned for the 2-class box detection task. This is an upgrade from the nano (n) model, providing significantly better accuracy.

---

## Architecture Summary

```
Model summary: 170 layers, 25,857,478 parameters, 25,857,462 gradients, 79.1 GFLOPs
Post-fusion:   93 layers, 25,840,918 parameters, 0 gradients,            78.7 GFLOPs
```

### Layer Breakdown

| Layer | From | Params | Module | Arguments |
|-------|------|--------|--------|-----------|
| 0 | -1 | 1,392 | Conv | [3, 48, 3, 2] |
| 1 | -1 | 41,664 | Conv | [48, 96, 3, 2] |
| 2 | -1 | 111,360 | C2f | [96, 96, 2, True] |
| 3 | -1 | 166,272 | Conv | [96, 192, 3, 2] |
| 4 | -1 | 813,312 | C2f | [192, 192, 4, True] |
| 5 | -1 | 664,320 | Conv | [192, 384, 3, 2] |
| 6 | -1 | 3,248,640 | C2f | [384, 384, 4, True] |
| 7 | -1 | 1,991,808 | Conv | [384, 576, 3, 2] |
| 8 | -1 | 3,985,920 | C2f | [576, 576, 2, True] |
| 9 | -1 | 831,168 | SPPF | [576, 576, 5] |
| 10 | -1 | 0 | Upsample | [None, 2, 'nearest'] |
| 11 | [-1,6] | 0 | Concat | [1] |
| 12 | -1 | 1,993,728 | C2f | [960, 384, 2] |
| 13 | -1 | 0 | Upsample | [None, 2, 'nearest'] |
| 14 | [-1,4] | 0 | Concat | [1] |
| 15 | -1 | 517,632 | C2f | [576, 192, 2] |
| 16 | -1 | 332,160 | Conv | [192, 192, 3, 2] |
| 17 | [-1,12] | 0 | Concat | [1] |
| 18 | -1 | 1,846,272 | C2f | [576, 384, 2] |
| 19 | -1 | 1,327,872 | Conv | [384, 384, 3, 2] |
| 20 | [-1,9] | 0 | Concat | [1] |
| 21 | -1 | 4,207,104 | C2f | [960, 576, 2] |
| 22 | [15,18,21] | 3,776,854 | Detect | [2, 16, None, [192, 384, 576]] |

---

## vs YOLOv8n Comparison

| Property | YOLOv8n (old) | YOLOv8m (new) |
|----------|---------------|---------------|
| Layers | 130 | 170 |
| Parameters | 3.0M | **25.8M** |
| GFLOPs | 8.2 | **79.1** |
| Model size | 6.0 MB | **49.6 MB** |
| Accuracy | Higher speed | **Higher accuracy** |

---

## Transfer Learning

```
Transferred 469/475 items from pretrained weights
Freezing layer 'model.22.dfl.conv.weight'
nc overridden: 80 → 2 (custom classes)
```

---

## AMP (Automatic Mixed Precision)

```
AMP: running Automatic Mixed Precision (AMP) checks...
AMP: checks passed ✅
```
