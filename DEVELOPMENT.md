# Development Workflow Guide

Best practices for developing your TIL-26 solution.

## Typical Development Cycle

```
1. Understand       (read docs)
2. Implement       (write code)
3. Test            (validate locally)
4. Benchmark       (compare performance)
5. Optimize        (improve score)
6. Submit          (deploy)
7. Iterate         (go to step 5)
```

## Phase 1: Understanding

**Before writing code:**

1. Read challenge overview
2. Understand the interface (input/output format)
3. Study existing implementations
4. Run example code
5. Visualize/test the system

**Time:** 30-60 minutes per challenge

## Phase 2: Implementation

### AE (Agent Environment)

```python
# Step 1: Load environment
from til_environment.bomberman_env import basic_env
env = basic_env()

# Step 2: Implement manager
class AEManager:
    def predict(self, instance):
        observation = instance["observation"]
        # Your logic here
        return action_index

# Step 3: Test locally
manager = AEManager()
results = manager.predict(test_obs)
```

### CV (Computer Vision)

```python
# Step 1: Load pretrained model
from ultralytics import YOLO
model = YOLO("yolov8l.pt")

# Step 2: Create manager
class CVManager:
    def predict(self, base64_image):
        image = decode_base64(base64_image)
        detections = self.model(image)
        return format_detections(detections)

# Step 3: Train and export
model.train(data="dataset.yaml", epochs=100)
```

### NLP (Natural Language Processing)

```python
# Step 1: Load embedding + LLM
from langchain.embeddings import HuggingFaceEmbeddings
from langchain.vectorstores import FAISS

# Step 2: Create RAG manager
class NLPManager:
    def load_documents(self, docs):
        self.vectorstore = FAISS.from_documents(docs, self.embeddings)
    
    def answer(self, question):
        results = self.vectorstore.similarity_search(question, k=3)
        return self.llm.generate(question, results)

# Step 3: Test locally
manager = NLPManager()
manager.load_documents(test_docs)
answer = manager.answer(test_question)
```

## Phase 3: Local Testing

### Before Server Testing

```bash
# Import test
python -c "from module import Manager; Manager()"

# Unit test
python -m pytest tests/ -v

# Integration test
python test_locally.py
```

### Server Testing

```bash
# Start server
cd <challenge>/submit/src
uvicorn <challenge>_server:app --port <PORT>

# Test health
curl http://localhost:<PORT>/health

# Test endpoint
curl -X POST http://localhost:<PORT>/<challenge> \
  -H "Content-Type: application/json" \
  -d @test_payload.json
```

## Phase 4: Benchmarking

### Compare Against Baselines

```bash
# AE: Compare managers
python ae/benchmarks/bench_multi.py --manager ae_manager --compare ae_manager_best

# CV: Compare models
python cv/train/evaluate.py --model runs/yolov8l-v3-*/weights/best.pt

# NLP: Evaluate RAG
python nlp/evaluate.py --system nlp_manager --dataset test_data.json
```

### Track Metrics

| Challenge | Metric | Goal |
|-----------|--------|------|
| AE | Average score | > 1000 |
| CV | mAP@0.5:0.95 | > 0.50 |
| NLP | BLEU/ROUGE | > 0.40 |

## Phase 5: Optimization

### AE Optimization
- **Agent Policy:** Better exploration/exploitation
- **Model Size:** Compress for faster inference
- **Hyperparameters:** Tune learning rate, discount, etc.

### CV Optimization
- **Model Selection:** YOLO vs DETR vs RF-DETR
- **Training:** More epochs, better augmentation
- **Inference:** Quantization, pruning
- **Ensemble:** Combine multiple models

### NLP Optimization
- **Retrieval:** Better chunking, hybrid search
- **Generation:** Prompt engineering, better LLM
- **Reranking:** Cross-encoder for better results

## Phase 6: Submission

### Pre-Submission Checklist

```bash
# 1. Test locally
cd <challenge>/submit/src
uvicorn <challenge>_server:app --port <PORT>
# Test in another terminal: curl http://localhost:<PORT>/health

# 2. Build Docker
cd ..
docker build -t til-<challenge> .
docker run --rm -p <PORT>:<PORT> --gpus all til-<challenge>

# 3. Validate output format
curl -X POST http://localhost:<PORT>/<challenge> \
  -d @test_payload.json > output.json
python validate_output.py output.json

# 4. Submit
# Follow competition instructions
```

## Phase 7: Iteration

After submission:

1. **Review results** — What worked, what didn't?
2. **Analyze errors** — Where did system fail?
3. **Plan improvements** — What to optimize?
4. **Go to Phase 5** — Optimize and resubmit

## Development Tools

### Debugging
```python
# Print debugging
print(f"Shape: {obs.shape}, Type: {obs.dtype}")

# Logging
import logging
logging.basicConfig(level=logging.DEBUG)
logger = logging.getLogger(__name__)

# Profiling
import cProfile
cProfile.run("your_function()")

# Memory profiling
%load_ext memory_profiler
%memit function()
```

### Testing
```bash
# Pytest
pytest test_file.py -v

# With coverage
pytest --cov=. test_file.py

# Specific test
pytest test_file.py::test_function -v
```

### Code Quality
```bash
# Linting
pylint your_file.py
flake8 your_file.py

# Formatting
black your_file.py
autopep8 --in-place your_file.py

# Type checking
mypy your_file.py
```

## Performance Profiling

### Identify Bottlenecks
```python
import time

start = time.time()
# Your code
elapsed = time.time() - start
print(f"Took {elapsed:.3f}s")
```

### Memory Usage
```python
import psutil
process = psutil.Process()
print(process.memory_info().rss / 1024 / 1024, "MB")
```

### GPU Usage
```bash
nvidia-smi dmon -c 10  # Monitor 10 seconds
```

## Common Patterns

### Configuration Management
```python
# config.yaml
model:
  name: yolov8l
  path: models/yolov8l.pt
  
inference:
  batch_size: 16
  confidence: 0.5

# Load in code
import yaml
with open("config.yaml") as f:
    config = yaml.safe_load(f)
model_name = config["model"]["name"]
```

### Checkpointing
```python
import torch

# Save checkpoint
torch.save({
    "epoch": epoch,
    "model_state": model.state_dict(),
    "optimizer_state": optimizer.state_dict(),
}, "checkpoint.pt")

# Load checkpoint
checkpoint = torch.load("checkpoint.pt")
model.load_state_dict(checkpoint["model_state"])
epoch = checkpoint["epoch"]
```

### Logging Training
```python
import json
from pathlib import Path

metrics_file = Path("metrics.jsonl")
for epoch in range(num_epochs):
    train_loss = train()
    val_loss = validate()
    
    metrics_file.write_text(json.dumps({
        "epoch": epoch,
        "train_loss": train_loss,
        "val_loss": val_loss
    }) + "\n", append=True)
```

## Best Practices

1. **Start simple** — Get baseline working before optimization
2. **Version your code** — Use Git, tag milestones
3. **Document assumptions** — What did you assume about the data?
4. **Test edge cases** — What if input is malformed?
5. **Monitor baselines** — Track metrics over time
6. **Commit frequently** — Small, focused commits
7. **Use branches** — For experimental features
8. **Code review** — If working in team

## Git Workflow

```bash
# Create feature branch
git checkout -b feature/improved-model

# Make changes
# Test locally
# Commit
git add <files>
git commit -m "Improve model accuracy"

# Push
git push origin feature/improved-model

# Submit for review (if team)
# Then merge to main
git checkout main
git merge feature/improved-model
```

## Deployment Readiness

Before submitting:

- [ ] Code tested locally
- [ ] Docker image builds
- [ ] Server starts without errors
- [ ] Health endpoint responds
- [ ] Test payload produces correct output
- [ ] Performance acceptable
- [ ] Error handling in place
- [ ] Reproducible (all seeds set, dependencies frozen)

---

**Remember:** Iterate early and often. Submit, learn, improve, repeat!
