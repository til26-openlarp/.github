# CV Challenge — Computer Vision (Object Detection)

Object detection on aerial imagery. Train YOLOv8, RT-DETR, or RF-DETR models.

## Overview

- **Type:** Object Detection & Classification
- **Dataset:** Aerial images (COCO format)
- **Port:** 5002
- **Difficulty:** Intermediate
- **Training Time:** Hours to days

## Challenge Interface

### Submission Request
```json
{
  "instances": [
    {"key": 0, "b64": "BASE64_JPEG_IMAGE"}
  ]
}
```

### Submission Response
```json
{
  "predictions": [
    [
      {"bbox": [x, y, w, h], "category_id": 0}
    ]
  ]
}
```

**Format:** `bbox` is `[x, y, width, height]` (top-left + size)

## Models

### YOLOv8 / YOLOv9
**Pros:** Fast, production-ready  
**Cons:** Slightly lower accuracy  
**Use:** Speed-focused submission  
**Training:** Hours  
**Inference:** 10-50 ms/image

### RT-DETR (Real-Time DETR)
**Pros:** Good speed/accuracy balance  
**Cons:** Slower than YOLO  
**Use:** Balanced submission  
**Training:** Hours  
**Inference:** 50-100 ms/image

### RF-DETR (Rotated DETR)
**Pros:** Highest accuracy, handles rotations  
**Cons:** Slowest inference  
**Use:** Accuracy-focused submission  
**Training:** Hours-days  
**Inference:** 100-200 ms/image

## Quick Start

```bash
cd cv
pip install -r train/requirements.txt

# Train YOLO
python train/train_yolo.py --config train/config/yolov8l-v3.yaml

# OR RT-DETR
python train/train_rtdetr.py --config train/config/rtdetr-v5.yaml

# OR RF-DETR
python train/train_rfdetr.py --config train/config/rfdetr-v5.yaml
```

## Training Workflow

### 1. Prepare Dataset
```bash
cd cv/train
python resize_dataset.py  # Optional: resize for speed
# Creates training-ready dataset in ../til26/
```

### 2. Configure Model
Edit `train/config/*.yaml`:
```yaml
model:
  name: yolov8l
  pretrained: true
  
training:
  epochs: 100
  batch_size: 16
  device: cuda
  
augmentation:
  enabled: true
```

### 3. Train Model
```bash
python train/train_yolo.py --config config/yolov8l-v3.yaml
# Output: runs/yolov8l-v3-*/weights/best.pt
```

### 4. Export & Convert
```bash
bash export-rtdetr.sh   # For RT-DETR
bash export-rfdetr.sh   # For RF-DETR
# YOLO needs no export
```

### 5. Integrate into Submission
```python
# cv/submit/src/cv_server.py
from yolo_manager import CVManager  # or rtdetr_manager, rfdetr_manager
# Set CV_MODEL_PATH to your checkpoint
```

### 6. Test Locally
```bash
cd cv/submit/src
uvicorn cv_server:app --port 5002
# Test with: curl -X POST http://localhost:5002/cv ...
```

### 7. Build & Submit
```bash
cd cv/submit
docker build -t til-cv .
docker run --rm -p 5002:5002 --gpus all til-cv
```

## Implementation

### Submission Manager
```python
# cv/submit/src/cv_manager.py (or rtdetr, rfdetr versions)

class CVManager:
    def __init__(self):
        self.model = load_model("path/to/checkpoint.pt")
    
    def predict(self, base64_image):
        # Decode image
        image = decode_base64(base64_image)
        # Run model
        detections = self.model(image)
        # Format output
        return format_for_submission(detections)
```

## Key Files

- `cv/README.md` — Challenge overview
- `cv/AGENTS.md` — Developer guide
- `cv/submit/README.md` — Submission service setup
- `cv/train/README.md` — Training guide
- `cv/train/config/` — Training configs
- `cv/train/train_*.py` — Training scripts
- `cv/submit/src/*_manager.py` — Model managers
- `cv/submit/src/cv_server.py` — FastAPI server

## Training Tips

1. **Start small:** Use YOLOv8s, not YOLOv8l
2. **Monitor GPU:** `nvidia-smi dmon` to watch memory
3. **Use TensorBoard:** `tensorboard --logdir runs`
4. **Reduce batch if OOM:** Set `batch_size: 8` in config
5. **Data augmentation:** Already configured, disable if needed
6. **Early stopping:** Check val metrics, stop if not improving

## Optimization Strategies

### For Speed
- Use YOLOv8 (smallest model)
- Reduce input resolution
- Quantize model (int8)
- Use TensorRT export

### For Accuracy
- Use RF-DETR (slowest but most accurate)
- Larger batch size (if memory allows)
- More epochs
- Better augmentation

### Balanced
- Use RT-DETR
- Medium batch size
- 100+ epochs
- Standard augmentation

## Common Issues

| Issue | Solution |
|-------|----------|
| OOM during training | Reduce batch_size in config |
| Slow training | Use smaller model (yolov8s not yolov8l) |
| Low accuracy | More epochs, better augmentation |
| Server won't load model | Check checkpoint path, format |
| Inference slow | Use YOLOv8 instead of DETR |

## Metrics

Watch these during training:
- **mAP@0.5:0.95** — Official metric (higher is better)
- **Precision** — True positives / all positives
- **Recall** — True positives / all ground truth
- **Loss** — Training loss (should decrease)

## Submission Checklist

- [ ] Model trained and exported
- [ ] Checkpoint path configured
- [ ] Manager imports correct model class
- [ ] Tested locally on port 5002
- [ ] Health endpoint responds
- [ ] Test payload returns correct format
- [ ] Docker builds without errors
- [ ] Docker container starts and responds

## Resources

- **Main docs:** `../cv/README.md`
- **Developer guide:** `../cv/AGENTS.md`
- **Submission setup:** `../cv/submit/README.md`
- **Training guide:** `../cv/train/README.md`
- **YOLO docs:** https://docs.ultralytics.com/
- **RT-DETR docs:** https://github.com/PaddlePaddle/PaddleDetection
- **RF-DETR:** https://github.com/ViTAE-Transformer/RF-DETR

## Next Steps

1. Read `../cv/README.md`
2. Read `../cv/AGENTS.md`
3. Follow training guide
4. Train first model (YOLOv8 recommended)
5. Deploy in submission server
6. Iterate!

---

**Estimated time to first submission:** 4-6 hours
