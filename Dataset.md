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

---

## Reading the YAML Config

```python
import yaml

dataset_yaml = os.path.join(dataset.location, "data.yaml")
print(f"Dataset YAML found at: {dataset_yaml}")

with open(dataset_yaml, 'r') as f:
    data_info = yaml.safe_load(f)
    print(f"Classes: {data_info['names']}")
```

**Output:**
```
Dataset YAML found at: /content/boxes-2/data.yaml
Classes: ['box', 'small_box']
```

---

## Download Progress

```
Downloading Dataset Version Zip: 100%  24428/24428 [00:03<00:00, 6679.38it/s]
Extracting Dataset Version Zip:  100%  1336/1336   [00:00<00:00, 5498.65it/s]
```
