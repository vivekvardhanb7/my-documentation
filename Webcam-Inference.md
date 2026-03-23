# 📷 Live Webcam Inference (Google Colab)

## Overview
Real-time object detection is performed by capturing webcam frames from the browser via JavaScript, running inference with the ONNX model on the Colab backend, and pushing annotated frames back to the browser display.

---

## Model Loading

```python
import os
from ultralytics import YOLO

model_path = '/content/best.onnx'
if not os.path.exists(model_path):
    model_path = '/content/runs/detect/yolo_box_model/weights/best.onnx'

print(f"Loading model from: {model_path}")
model = YOLO(model_path, task='detect')
```

**Runtime output:**
```
Loading /content/best.onnx for ONNX Runtime inference...
Using ONNX Runtime 1.24.4 with CUDAExecutionProvider
```

---

## Webcam Initialization (JavaScript)

The `init_camera()` function injects JavaScript into the Colab output cell to:
1. Create a `<video>` element and start the camera stream (`facingMode: 'environment'`)
2. Draw each frame onto a hidden `<canvas>` (`640×480`)
3. Expose `window.captureFrame()` — returns the frame as a base64 JPEG
4. Expose `window.updateOutput(imgData)` — updates the `<img>` tag with annotated result
5. Set `window.cameraReady = true` when ready

```python
from IPython.display import display, Javascript
from google.colab.output import eval_js

def init_camera():
    js = Javascript('''
        async function createDom() {
            // ... creates video + canvas + img elements
            window.captureFrame = function() { ... return canvas.toDataURL('image/jpeg', 0.8); };
            window.updateOutput = function(imgData) { ... outImg.src = imgData; };
            window.stopStream   = function() { stream.getTracks().forEach(t => t.stop()); ... };
            window.cameraReady  = true;
        }
        createDom();
    ''')
    display(js)
```

---

## Camera Ready Check

```python
for i in range(10):
    try:
        if eval_js('window.cameraReady === true'):
            print("Camera ready!")
            break
    except:
        pass
    time.sleep(1)
else:
    print("Camera initialization timed out.")
```

---

## Inference Loop

```python
while True:
    # 1. Capture frame from browser
    img_data = eval_js('window.captureFrame()')
    if not img_data:
        break

    # 2. Decode base64 JPEG → NumPy array
    binary = b64decode(img_data.split(',')[1])
    img    = cv2.imdecode(np.frombuffer(binary, np.uint8), cv2.IMREAD_COLOR)

    # 3. Run YOLOv8 prediction
    results = model.predict(source=img, conf=0.3, imgsz=640, verbose=False)

    # 4. Annotate frame
    annotated_img = results[0].plot()

    # 5. Encode back to base64 and push to browser
    _, buffer     = cv2.imencode('.jpg', annotated_img)
    encoded_data  = f'data:image/jpeg;base64,{b64encode(buffer).decode()}'
    eval_js(f'window.updateOutput("{encoded_data}")')
```

---

## Stopping the Stream

```python
# Interrupt cell with Ctrl+C or kernel interrupt
except KeyboardInterrupt:
    print('Stopped by user.')
finally:
    eval_js('window.stopStream()')   # releases camera track
```

---

## Key Parameters

| Parameter | Value | Description |
|-----------|-------|-------------|
| `conf` | 0.3 | Confidence threshold for detections |
| `imgsz` | 640 | Inference image size |
| `verbose` | False | Suppress per-frame logs |
| Frame size | 640×480 | Browser canvas resolution |
