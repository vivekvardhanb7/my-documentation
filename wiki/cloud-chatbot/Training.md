# 🏋️ Training

## Training Code

```python
import torch

device = 0 if torch.cuda.is_available() else 'cpu'
print(f"Using device: {device}")

results = model.train(
    data    = dataset_yaml,
    epochs  = 100,
    imgsz   = 640,
    batch   = 16,
    device  = device,
    name    = 'yolo_box_model_augmented',
    # Advanced Augmentations
    augment = True,
    mosaic  = 1.0,      # Combine 4 images into one
    mixup   = 0.1,      # Blend two images for generalization
    degrees = 15.0,     # Random rotation ±15°
    flipud  = 0.5,      # Vertical flip probability
    fliplr  = 0.5,      # Horizontal flip probability
    scale   = 0.5       # Random scaling
)
```

**Output:**
```
Using device: 0
```

All results are saved to: `/content/runs/detect/yolo_box_model_augmented/`

---

## Hyperparameters

| Parameter | Value | Parameter | Value |
|-----------|-------|-----------|-------|
| `epochs` | **100** | `imgsz` | 640 |
| `batch` | 16 | `device` | 0 (GPU) |
| `optimizer` | AdamW (auto) | `lr0` | 0.001667 |
| `momentum` | 0.9 | `weight_decay` | 0.0005 |
| `warmup_epochs` | 3.0 | `warmup_momentum` | 0.8 |
| `warmup_bias_lr` | 0.1 | `cos_lr` | False |
| `box` | 7.5 | `cls` | 0.5 |
| `dfl` | 1.5 | `iou` | 0.7 |
| `mosaic` | 1.0 | `fliplr` | 0.5 |
| `close_mosaic` | 10 | `patience` | 100 |
| `amp` | True | `pretrained` | True |
| `augment` | **True** | `mixup` | **0.1** |
| `degrees` | **15.0** | `flipud` | **0.5** |
| `scale` | **0.5** | `seed` | 0 |

---

## Optimizer

```
AdamW(lr=0.001667, momentum=0.9)
  - 77 weight groups (decay=0.0)
  - 84 weight groups (decay=0.0005)
  - 83 bias groups  (decay=0.0)
```

---

## Augmentations

### Built-in (training params)
| Augmentation | Value | Description |
|-------------|-------|-------------|
| `mosaic` | 1.0 | Combine 4 images — helps small object detection |
| `mixup` | 0.1 | Blend images for better generalization |
| `fliplr` | 0.5 | Horizontal flip |
| `flipud` | 0.5 | Vertical flip |
| `degrees` | 15.0 | Random rotation ±15° |
| `scale` | 0.5 | Random scale |

### Albumentations (auto-applied)
| Augmentation | Probability |
|-------------|-------------|
| Blur | 0.01 |
| MedianBlur | 0.01 |
| ToGray | 0.01 |
| CLAHE | 0.01 |

> Mosaic is disabled for final 10 epochs (`close_mosaic=10`).

---

## Per-Epoch Metrics (Validation Set, selected)

| Epoch | GPU Mem | box_loss | cls_loss | dfl_loss | mAP@50 | mAP@50-95 |
|-------|---------|----------|----------|----------|--------|-----------|
| 1 | 6.35G | 0.7073 | 2.126 | 1.150 | 0.056 | 0.024 |
| 10 | 6.63G | 0.8157 | 1.415 | 1.172 | 0.626 | 0.501 |
| 20 | 6.63G | 0.6461 | 1.195 | 1.079 | 0.727 | 0.644 |
| 30 | 6.63G | 0.6252 | 1.101 | 1.068 | 0.787 | 0.726 |
| 40 | 6.63G | 0.5552 | 0.954 | 1.020 | 0.796 | 0.761 |
| 43 | 6.63G | 0.5629 | 0.917 | 1.024 | **0.949** | 0.909 |
| 48 | 6.63G | 0.5111 | 0.872 | 1.007 | 0.955 | **0.909** |
| 54 | 6.63G | 0.5060 | 0.839 | 0.996 | **0.986** | 0.942 |
| 60 | 6.63G | 0.5034 | 0.763 | 0.989 | 0.995 | 0.941 |
| 66 | 6.63G | 0.4778 | 0.723 | 0.978 | **0.995** | 0.943 |
| 77 | 6.63G | 0.4426 | 0.607 | 0.955 | **0.995** | **0.941** |
| 80 | 6.63G | 0.4352 | 0.606 | 0.963 | 0.995 | 0.944 |
| 90 | 6.63G | 0.4180 | 0.536 | 0.954 | 0.985 | 0.945 |
| 96 | 6.63G | 0.3223 | 0.296 | 0.874 | 0.991 | 0.951 |
| 100 | 6.63G | 0.3076 | 0.280 | 0.872 | 0.988 | 0.954 |

> Training completed in **0.684 hours** (~41 minutes).

---

## Configuration Summary (printed after training)

```
=== CONFIGURATION APPLIED ===
✓ Model: yolov8m.pt (Medium size - better accuracy than nano/small)
✓ Epochs: 100 (Sufficient for learning box features)
✓ Image size: 640x640
✓ Batch size: 16

=== AUGMENTATIONS ACTIVE ===
✓ Mosaic (1.0): Combining 4 images to help with small object detection
✓ Mixup (0.1): Blending images for better generalization
✓ Flips: Horizontal and Vertical (0.5) enabled
```

---

## Saved Weights

| File | Description |
|------|------------|
| `weights/best.pt` | Best checkpoint (52.0 MB) |
| `weights/last.pt` | Final epoch checkpoint (52.0 MB) |
| `weights/best.onnx` | Exported ONNX model (99.1 MB) |
