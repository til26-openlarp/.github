# Performance Optimization Guide

Tips and techniques for optimizing your TIL-26 solutions.

## Performance Priorities

**Trade-offs:**
- **Speed vs Accuracy:** Faster models are less accurate
- **Memory vs Accuracy:** Smaller models use less memory
- **Speed vs Memory:** Very efficient models are hard to build

**Choose based on your constraints:**
- **GPU limited?** → Optimize for memory
- **Latency critical?** → Optimize for speed
- **Score-focused?** → Optimize for accuracy

---

## AE Optimization

### Model Size

```python
# Large model (slow but accurate)
model = DQN(state_size=128, hidden_layers=4)

# Medium model (balanced)
model = DQN(state_size=64, hidden_layers=2)

# Small model (fast but less accurate)
model = DQN(state_size=32, hidden_layers=1)
```

### Inference Speed

```python
# Slow
action = model.predict(observation)  # Torch model

# Fast
action = model.predict_fast(observation)  # Quantized/compiled
```

### Exploration Efficiency

```python
# Too much exploration = wasted computation
epsilon = 0.1  # Reasonable exploration

# Better: Adaptive exploration
epsilon = 0.1 * (1 - progress)  # Decreases over time
```

### Benchmark Locally

```bash
cd ae
python benchmarks/bench_diag.py
python benchmarks/bench_multi.py --seeds 5  # Quick test

# Compare managers
python benchmarks/bench_multi.py --manager ae_manager --compare ae_manager_best
```

---

## CV Optimization

### Model Selection

**For Speed:**
```python
# Fastest
model = YOLO("yolov8n.pt")  # Nano - 50 FPS

# Medium
model = YOLO("yolov8s.pt")  # Small - 35 FPS

# Slowest (most accurate)
model = YOLO("yolov8l.pt")  # Large - 10 FPS
```

**For Accuracy:**
```python
# Less accurate
model = YOLO("yolov8m.pt")  # mAP ~50%

# Most accurate
model = YOLO("yolov8x.pt")  # mAP ~54%

# Even more accurate
model = RF-DETR()  # mAP ~58%
```

### Quantization

```python
# Export to FP16 (50% memory, slight accuracy loss)
model = YOLO("yolov8l.pt")
model.export(format="onnx", half=True)

# Export to INT8 (75% memory, some accuracy loss)
model.export(format="onnx", int8=True)
```

### Batch Processing

```python
# Slower - process one at a time
for img in images:
    result = model(img)

# Faster - batch process
results = model(images, batch_size=16)
```

### Input Resolution

```python
# Slower (more accurate)
results = model(image)  # Default 640x640

# Faster (less accurate)
results = model(image, imgsz=320)  # 320x320
```

### Training Optimization

```yaml
# In config.yaml
training:
  # Faster training
  batch_size: 32         # Larger batches
  epochs: 50             # Fewer epochs
  img_size: 320          # Smaller images
  
  # Better accuracy
  batch_size: 8          # Smaller batches
  epochs: 200            # More epochs
  img_size: 640          # Larger images
  augmentation: advanced # Better aug
```

---

## NLP Optimization

### Embedding Model Selection

**For Speed:**
```python
# Fastest
model = "all-MiniLM-L6-v2"     # 384-dim, 10MB
embeddings = HuggingFaceEmbeddings(model_name=model)

# Medium
model = "all-mpnet-base-v2"    # 768-dim, 130MB

# Most accurate
model = "all-mpnet-large-v2"   # 768-dim, 430MB
```

### Vector Store Optimization

```python
# In-memory (fast, limited size)
vectorstore = FAISS.from_documents(docs, embeddings)

# Persistent (survives restarts)
vectorstore = Chroma.from_documents(docs, embeddings, persist_dir="./db")
```

### Retrieval Parameters

```python
# Faster (less accurate)
results = vectorstore.similarity_search(question, k=2)

# Balanced
results = vectorstore.similarity_search(question, k=3)

# More accurate (slower)
results = vectorstore.similarity_search(question, k=5)
```

### LLM Selection

**For Speed:**
```python
# Fastest
llm = HuggingFaceHub(
    repo_id="meta-llama/Llama-2-7b-chat-hf",
    model_kwargs={"temperature": 0.5}
)

# Medium
llm = "gpt-3.5-turbo"

# Most accurate
llm = "gpt-4"
```

### Caching

```python
from langchain.cache import InMemoryCache
import langchain

# Cache LLM responses
langchain.llm_cache = InMemoryCache()

# Now repeated questions are fast
answer1 = qa.run("What is X?")  # Slow, calls LLM
answer2 = qa.run("What is X?")  # Fast, uses cache
```

---

## General Optimization Techniques

### Profiling

```python
import time
import cProfile

# Simple timing
start = time.time()
prediction = model.predict(input)
elapsed = time.time() - start
print(f"Took {elapsed:.3f}s")

# Detailed profiling
cProfile.run('model.predict(input)')

# Memory profiling
import tracemalloc
tracemalloc.start()
prediction = model.predict(input)
current, peak = tracemalloc.get_traced_memory()
print(f"Current: {current/1024/1024:.1f}MB, Peak: {peak/1024/1024:.1f}MB")
```

### Caching

```python
from functools import lru_cache

@lru_cache(maxsize=1000)
def expensive_function(input):
    return result

# First call: computed
result1 = expensive_function("query")

# Second call: cached
result2 = expensive_function("query")  # Instant
```

### Lazy Loading

```python
# Load only when needed
class Manager:
    def __init__(self):
        self.model = None  # Don't load yet
    
    def predict(self, input):
        if self.model is None:
            self.model = load_model()  # Load on first use
        return self.model(input)
```

### Batch Processing

```python
# Slow - process one by one
predictions = []
for item in items:
    pred = model.predict(item)
    predictions.append(pred)

# Fast - process in batches
predictions = model.predict_batch(items, batch_size=32)
```

---

## GPU Optimization

### Device Management

```python
import torch

# Auto-detect
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

# Specific GPU
device = torch.device("cuda:0")  # GPU 0

# CPU only (for testing)
device = torch.device("cpu")

# Move model to device
model = model.to(device)
```

### Mixed Precision

```python
from torch.cuda.amp import autocast, GradScaler

scaler = GradScaler()

for input, target in dataloader:
    optimizer.zero_grad()
    
    # Autocast: use FP16 where safe
    with autocast():
        output = model(input)
        loss = criterion(output, target)
    
    scaler.scale(loss).backward()
    scaler.step(optimizer)
    scaler.update()

# Result: ~2x speedup with minimal accuracy loss
```

### Memory Optimization

```python
# Reduce batch size if OOM
batch_size = 16  # Was 32

# Use gradient checkpointing
model = model.gradient_checkpointing_enable()

# Clear cache
torch.cuda.empty_cache()
```

---

## Inference Optimization

### Model Compilation (PyTorch 2.0+)

```python
import torch

model = load_model()

# Compile model for ~30% speedup
compiled_model = torch.compile(model)

predictions = compiled_model(input)
```

### ONNX Export

```python
import torch.onnx

# Export model
dummy_input = torch.randn(1, 3, 640, 640)
torch.onnx.export(
    model,
    dummy_input,
    "model.onnx",
    export_params=True,
    input_names=["input"],
    output_names=["output"],
    dynamic_axes={"input": {0: "batch_size"}}
)

# Load ONNX model (inference only)
import onnxruntime
session = onnxruntime.InferenceSession("model.onnx")
```

### TensorRT (NVIDIA)

```bash
# Convert ONNX to TensorRT
trtexec --onnx=model.onnx --saveEngine=model.trt --half

# Use in Python
import tensorrt
# Load and run model.trt
```

---

## Benchmarking

### Compare Models

```bash
# AE: Compare managers
python ae/benchmarks/bench_multi.py \
  --manager ae_manager \
  --compare ae_manager_best \
  --seeds 10

# CV: Compare models
python cv/train/evaluate.py \
  --model1 runs/yolov8l-v3-*/weights/best.pt \
  --model2 runs/yolov8m-v3-*/weights/best.pt

# NLP: Compare systems
python nlp/evaluate.py \
  --system1 rag_system_v1 \
  --system2 rag_system_v2 \
  --dataset test_qa.json
```

### Track Metrics Over Time

```python
import json
from datetime import datetime

metrics = {
    "timestamp": datetime.now().isoformat(),
    "model": "yolov8l-v3",
    "mAP": 0.52,
    "latency_ms": 125,
    "memory_mb": 1240
}

with open("metrics.jsonl", "a") as f:
    f.write(json.dumps(metrics) + "\n")
```

---

## Performance Checklist

Before submission:

- [ ] Profiled code to find bottlenecks
- [ ] Model size reasonable (< 5GB)
- [ ] Inference time acceptable (< 1s typical)
- [ ] Memory usage acceptable (< 8GB typical)
- [ ] Accuracy/score good enough
- [ ] Tested on various inputs
- [ ] Cached expensive operations
- [ ] Batch processing implemented where applicable
- [ ] GPU utilized properly (if available)
- [ ] Quantization considered (if appropriate)

---

## Optimization Timeline

**Week 1:** Get it working (any speed)  
**Week 2:** Make it accurate (any latency)  
**Week 3:** Optimize for speed or accuracy (choose one)  
**Week 4:** Final tuning and submission  

---

**Remember:** Profile before optimizing. Don't guess where time is spent!
