# 🧠 Model Architecture

## Initialization

```python
from ultralytics import YOLO
model = YOLO('yolov8n.pt')
```

The **YOLOv8n** (nano) pretrained model is downloaded from Ultralytics GitHub releases (~6.2 MB) and fine-tuned for the 2-class box detection task.

---

## Architecture Summary

```
Model summary: 130 layers, 3,011,238 parameters, 3,011,222 gradients, 8.2 GFLOPs
Post-fusion:   73 layers,  3,006,038 parameters, 0 gradients,            8.1 GFLOPs
```

### Layer Breakdown

| Layer | From | Count | Params | Module | Arguments |
|-------|------|-------|--------|--------|-----------|
| 0 | -1 | 1 | 464 | Conv | [3, 16, 3, 2] |
| 1 | -1 | 1 | 4,672 | Conv | [16, 32, 3, 2] |
| 2 | -1 | 1 | 7,360 | C2f | [32, 32, 1, True] |
| 3 | -1 | 1 | 18,560 | Conv | [32, 64, 3, 2] |
| 4 | -1 | 2 | 49,664 | C2f | [64, 64, 2, True] |
| 5 | -1 | 1 | 73,984 | Conv | [64, 128, 3, 2] |
| 6 | -1 | 2 | 197,632 | C2f | [128, 128, 2, True] |
| 7 | -1 | 1 | 295,424 | Conv | [128, 256, 3, 2] |
| 8 | -1 | 1 | 460,288 | C2f | [256, 256, 1, True] |
| 9 | -1 | 1 | 164,608 | SPPF | [256, 256, 5] |
| 10 | -1 | 1 | 0 | Upsample | [None, 2, 'nearest'] |
| 11 | [-1,6] | 1 | 0 | Concat | [1] |
| 12 | -1 | 1 | 148,224 | C2f | [384, 128, 1] |
| 13 | -1 | 1 | 0 | Upsample | [None, 2, 'nearest'] |
| 14 | [-1,4] | 1 | 0 | Concat | [1] |
| 15 | -1 | 1 | 37,248 | C2f | [192, 64, 1] |
| 16 | -1 | 1 | 36,992 | Conv | [64, 64, 3, 2] |
| 17 | [-1,12] | 1 | 0 | Concat | [1] |
| 18 | -1 | 1 | 123,648 | C2f | [192, 128, 1] |
| 19 | -1 | 1 | 147,712 | Conv | [128, 128, 3, 2] |
| 20 | [-1,9] | 1 | 0 | Concat | [1] |
| 21 | -1 | 1 | 493,056 | C2f | [384, 256, 1] |
| 22 | [15,18,21] | 1 | 751,702 | Detect | [2, 16, None, [64,128,256]] |

---

## Transfer Learning

```
Transferred 319/355 items from pretrained weights
Freezing layer 'model.22.dfl.conv.weight'
nc overridden: 80 → 2 (custom classes)
```

---

## AMP (Automatic Mixed Precision)
AMP checks pass automatically for the Tesla T4 GPU, enabling faster training with reduced memory usage.

```
AMP: running Automatic Mixed Precision (AMP) checks...
AMP: checks passed ✅
```
