# 📦 Dataset

## Source
The dataset is hosted on **Roboflow** and downloaded using the Roboflow Python SDK.

```python
from roboflow import Roboflow

rf = Roboflow(api_key="ssjfBTDoXfNhHTWBNjjy")
project = rf.workspace("object-detection-e45hw").project("boxes-ca9dp")
version  = project.version(2)
dataset  = version.download("yolov8")
```

---

## Dataset Details

| Property | Value |
|----------|-------|
| Workspace | `object-detection-e45hw` |
| Project | `boxes-ca9dp` |
| Version | 2 |
| Format | YOLOv8 |
| Local path | `/content/boxes-2/` |
| Config file | `/content/boxes-2/data.yaml` |

---

## Classes

```
Classes: ['box', 'small_box']
```

| Class ID | Class Name |
|----------|------------|
| 0 | `box` |
| 1 | `small_box` |

---

## Dataset Split

| Split | Images | Backgrounds | Corrupt |
|-------|--------|-------------|---------|
| Train | 642 | 0 | 0 |
| Valid | 18 | 0 | 0 |
| Test  | 2  | 0 | 0 |
| **Total** | **662** | — | — |

---

## Dataset Verification

The updated notebook includes a dedicated verification step to confirm classes and image counts:

```python
import os

def count_images(directory):
    if not os.path.exists(directory):
        return 0
    return len([f for f in os.listdir(directory) if f.lower().endswith(('.png', '.jpg', '.jpeg'))])

print("=== DATASET VERIFICATION ===")
print(f"Dataset location: {dataset.location}")
print(f"Classes found: {data_info['names']}")
print(f"Number of classes: {len(data_info['names'])}")

train_path = os.path.join(dataset.location, 'train/images')
valid_path = os.path.join(dataset.location, 'valid/images')
test_path  = os.path.join(dataset.location, 'test/images')

print(f"Training Images:   {count_images(train_path)}")   # 642
print(f"Validation Images: {count_images(valid_path)}")   # 18
print(f"Testing Images:    {count_images(test_path)}")    # 2
print(f"Total Images:      {count_images(train_path) + count_images(valid_path) + count_images(test_path)}")  # 662
```

**Output:**
```
=== DATASET VERIFICATION ===
Dataset location: /content/boxes-2
Classes found: ['box', 'small_box']
Number of classes: 2

=== DATASET SIZE ===
Training Images:   642
Validation Images: 18
Testing Images:    2
Total Images:      662

Class Details:
  Class 0: box
  Class 1: small_box

Recommendation:
✓ Found: 'box' and 'small boxes' classes
  → Both classes are box-related. Model will detect both types.
  → Filtering will show detections from both classes together
```

---

## Reading the YAML Config

```python
import yaml

dataset_yaml = os.path.join(dataset.location, "data.yaml")
with open(dataset_yaml, 'r') as f:
    data_info = yaml.safe_load(f)
    print(f"Classes: {data_info['names']}")
```
