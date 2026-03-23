# 🖼️ Image Inference

## Overview
Upload one or more images to Google Colab and run YOLOv8 detection using the trained checkpoint (`best.pt`).

---

## Inference Code

```python
from google.colab import files
import matplotlib.pyplot as plt

print('Please upload an image to test (e.g., .jpg, .png):')
uploaded = files.upload()

if uploaded:
    img_path = list(uploaded.keys())[0]

    # Run detection (confidence threshold = 0.25)
    results = trained_model.predict(source=img_path, conf=0.25, save=True)

    # Visualize
    res          = results[0]
    res_plotted  = res.plot()[:, :, ::-1]   # BGR → RGB for matplotlib

    plt.figure(figsize=(12, 8))
    plt.imshow(res_plotted)
    plt.axis('off')
    plt.title("YOLOv8 Detection Result")
    plt.show()
```

---

## Sample Inference Output

```
Saving 3.jpeg to 3.jpeg

image 1/1 /content/3.jpeg: 640×384  1 box, 46.2ms
Speed: 2.4ms preprocess, 46.2ms inference, 2.4ms postprocess per image at shape (1, 3, 640, 384)
Results saved to /content/runs/detect/predict
```

---

## Inference Parameters

| Parameter | Value | Description |
|-----------|-------|-------------|
| `conf` | 0.25 | Confidence threshold — detections below this are discarded |
| `save` | True | Save annotated images to `runs/detect/predict/` |
| `imgsz` | (auto) | Maintains aspect ratio; longest side padded to 640 |

---

## Speed Breakdown (Sample Image 640×384)

| Step | Time |
|------|------|
| Preprocess | 2.4 ms |
| Inference | 46.2 ms |
| Postprocess | 2.4 ms |
| **Total** | **~51 ms** |

---

## Output Location

Annotated images are saved at:
```
/content/runs/detect/predict/
```

---

## Notes

- `res.plot()` returns a **BGR** NumPy array; convert to RGB with `[:, :, ::-1]` before displaying with matplotlib.
- Upload multiple files with `files.upload()` and iterate `uploaded.keys()` to process in batch.
- The `save=True` flag also writes a labels `.txt` file alongside each annotated image when `save_txt=True` is set.
