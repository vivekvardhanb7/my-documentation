# 📷 Live Webcam Inference

## Overview

Live object detection runs in Google Colab using the exported ONNX model. A JavaScript DOM element captures webcam frames; Python decodes and runs inference every 3rd frame to prevent lag.

---

## Full Code

```python
from IPython.display import display, Javascript
from google.colab.output import eval_js
from base64 import b64decode, b64encode
import numpy as np
import cv2
import time
import torch
from ultralytics import YOLO

# 1. Load ONNX model (GPU via ONNX Runtime if available)
onnx_model_path = '/best (3).onnx'
model = YOLO(onnx_model_path, task='detect')

# 2. Detection filter helper
def filter_detections(results, class_keywords=["box"], min_confidence=0.4):
    for result in results:
        if result.boxes is not None and len(result.boxes) > 0:
            class_names  = result.names
            target_indices = [idx for idx, name in class_names.items()
                              if any(k.lower() in name.lower() for k in class_keywords)]
            if target_indices:
                conf_mask  = result.boxes.conf >= min_confidence
                class_mask = torch.tensor(
                    [cls.item() in target_indices for cls in result.boxes.cls],
                    device=result.boxes.cls.device
                )
                result.boxes = result.boxes[conf_mask & class_mask]
            else:
                result.boxes = result.boxes[result.boxes.conf > 1.1]  # reject all
    return results

# 3. Initialize webcam DOM
def init_camera():
    js = Javascript('''
    async function createDom() {
        const div   = document.createElement('div');
        const video  = document.createElement('video');
        const output = document.createElement('img');
        const stats  = document.createElement('div');
        video.style.display = 'block';
        video.width = 480;  video.height = 360;
        video.setAttribute('playsinline', '');
        output.id = 'detection_output';
        stats.id  = 'detection_stats';
        document.body.appendChild(div);
        div.appendChild(video); div.appendChild(output); div.appendChild(stats);
        const stream = await navigator.mediaDevices.getUserMedia(
            {video: {facingMode: "environment"}}
        );
        video.srcObject = stream;
        await video.play();
        const canvas = document.createElement('canvas');
        canvas.width = 480; canvas.height = 360;
        window.captureFrame = function() {
            canvas.getContext('2d').drawImage(video, 0, 0, 480, 360);
            return canvas.toDataURL('image/jpeg', 0.6);
        };
        window.updateOutput = function(imgData, statsText) {
            output.src = imgData;
            document.getElementById('detection_stats').innerText = statsText;
        };
        window.stopStream = function() {
            stream.getTracks().forEach(track => track.stop());
            div.remove();
        };
        window.cameraReady = true;
    }
    createDom();
    ''')
    display(js)

print("Initializing optimized camera...")
init_camera()

# Wait for camera ready
for _ in range(10):
    try:
        if eval_js('window.cameraReady === true'): break
    except: pass
    time.sleep(1)

# 4. Inference loop (every 3rd frame)
print("Starting fast live inference (skipping frames to prevent hanging)...")
box_count_history  = []
frame_count        = 0
last_annotated_data = None

try:
    while True:
        img_data = eval_js('window.captureFrame()')
        if not img_data: break

        frame_count += 1
        if frame_count % 3 == 0:          # process every 3rd frame
            binary = b64decode(img_data.split(',')[1])
            img    = cv2.imdecode(np.frombuffer(binary, np.uint8), cv2.IMREAD_COLOR)

            results         = model.predict(source=img, conf=0.4, imgsz=640, verbose=False)
            filtered_results = filter_detections(results)
            annotated_img   = filtered_results[0].plot()

            num_boxes = len(filtered_results[0].boxes) if filtered_results[0].boxes is not None else 0
            box_count_history.append(num_boxes)
            avg_boxes  = np.mean(box_count_history[-20:])
            stats_text = f"Boxes: {num_boxes} | Avg: {avg_boxes:.1f} (Optimized)"

            _, buffer = cv2.imencode('.jpg', annotated_img)
            last_annotated_data = f"data:image/jpeg;base64,{b64encode(buffer).decode()}"
            eval_js(f'window.updateOutput("{last_annotated_data}", "{stats_text}")')

        time.sleep(0.01)

except KeyboardInterrupt:
    print("Stopped.")
finally:
    try: eval_js('window.stopStream()')
    except: pass
```

---

## Parameters

| Parameter | Value | Notes |
|-----------|-------|-------|
| ONNX model | `/best (3).onnx` | Pre-exported ONNX from training |
| ONNX Runtime | 1.24.4 with **CUDAExecutionProvider** | GPU-accelerated |
| Confidence threshold | **0.4** | Rejects weak detections |
| Image size | 640 × 640 | |
| Frame skip | every **3rd** frame | Prevents Colab hanging |
| Canvas resolution | 480 × 360 | |
| JPEG quality | 0.6 | Balances speed vs. quality |
| Class filter | `"box"` keyword | Matches both `box` and `small_box` |
| Rolling average window | last **20** frames | Smoothed box count display |

---

## filter_detections Logic

1. Get class indices that contain `"box"` (matches `box` and `small_box`)
2. Apply confidence threshold mask (≥ 0.4)
3. Apply class mask (only target classes)
4. If no target classes found → reject everything (`conf > 1.1` — always False)

---

## Output

An annotated JPEG is sent back to the browser overlay image element. Stats are displayed below showing:
- Current frame box count
- 20-frame rolling average

---

## Console Output

```
Initializing optimized camera...
Starting fast live inference (skipping frames to prevent hanging)...
Loading /best (3).onnx for ONNX Runtime inference...
Using ONNX Runtime 1.24.4 with CUDAExecutionProvider
Stopped.
```
