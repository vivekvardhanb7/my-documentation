# 🖼️ Image Inference

## Overview

Upload any JPEG/PNG image to Google Colab and run the trained YOLOv8m model to detect `box` and `small_box` objects in it.

---

## Code

```python
from ultralytics import YOLO
from google.colab import files
from PIL import Image
import matplotlib.pyplot as plt
import os

# Load best trained weights
weights_path = '/content/runs/detect/yolo_box_model_augmented/weights/best.pt'
model = YOLO(weights_path)
print("Loaded best trained weights.")

# Upload image
print("Please upload an image to test (e.g., .jpg, .png):")
uploaded = files.upload()

for filename, content in uploaded.items():
    # Save locally
    with open(filename, 'wb') as f:
        f.write(content)

    # Run inference
    results = model.predict(
        source   = filename,
        conf     = 0.5,
        imgsz    = 640,
        save     = True,
        show     = False,
        show_conf = True
    )

    # Display annotated result
    result_img_path = results[0].path
    plt.figure(figsize=(12, 8))
    plt.imshow(Image.open(result_img_path))
    plt.axis('off')
    plt.title(f'Detections: {filename}')
    plt.show()

    print(f"\nDetection results for {filename}:")
    for box in results[0].boxes:
        cls_id    = int(box.cls)
        cls_name  = results[0].names[cls_id]
        conf_val  = float(box.conf)
        x1, y1, x2, y2 = box.xyxy[0].tolist()
        print(f"  {cls_name}: conf={conf_val:.2f}  bbox=[{x1:.0f},{y1:.0f},{x2:.0f},{y2:.0f}]")
```

---

## Console Output Example

```
Loaded best trained weights.
Please upload an image to test (e.g., .jpg, .png):

Saving 2.jpeg to 2 (2).jpeg

image 1/1 /content/2 (2).jpeg: 640x384 1 box, 25.6ms
Speed: 2.1ms preprocess, 25.6ms inference, 1.2ms postprocess per image at shape (1, 3, 640, 384)
Results saved to /content/runs/detect/predict3
```

---

## Parameters

| Parameter | Value | Description |
|-----------|-------|-------------|
| `conf` | 0.5 | Confidence threshold — reject detections below this |
| `imgsz` | 640 | Resize input image to 640 × 640 |
| `save` | True | Save annotated result to disk |
| `show_conf` | True | Show confidence score in labels |

---

## Output Location

Annotated images are saved to:
```
/content/runs/detect/predict*/   (predict, predict2, predict3, …)
```

Each run increments the folder number automatically.
