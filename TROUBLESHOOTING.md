# Troubleshooting Guide

Common issues and solutions.

## Setup & Installation

### Issue: `git submodule` commands hang or fail

**Error:**
```
fatal: unable to access 'https://github.com/...': Could not resolve host
```

**Cause:** Network issue or incorrect credentials

**Solutions:**
```bash
# Try SSH instead
git config --global url."git@github.com:".insteadOf "https://github.com/"

# Or set up credentials
git config --global credential.helper store

# Retry
git submodule update --init --recursive
```

### Issue: Python version too old

**Error:**
```
This project requires Python 3.10+
```

**Solutions:**
```bash
# Check version
python --version

# Install newer Python
# On Mac: brew install python@3.11
# On Linux: apt-get install python3.11
# On Windows: Download from python.org

# Create venv with specific version
python3.11 -m venv venv
```

### Issue: `pip install` fails

**Error:**
```
ERROR: Could not find a version that satisfies the requirement
```

**Solutions:**
```bash
# Upgrade pip first
pip install --upgrade pip

# Try installing from file
pip install -r requirements.txt --verbose

# Install individually
pip install torch
pip install numpy

# Check for conflicts
pip check
```

### Issue: Module not found after installation

**Error:**
```
ModuleNotFoundError: No module named 'torch'
```

**Cause:** Venv not activated or package not installed

**Solutions:**
```bash
# Verify venv is activated
which python  # Should point to venv

# Reinstall
pip install torch ultralytics

# Check installation
python -c "import torch; print(torch.__version__)"
```

---

## Local Testing

### Issue: Server won't start

**Error:**
```
Address already in use
```

**Solution:**
```bash
# Use different port
uvicorn ae_server:app --port 5099

# Or find and stop existing process
lsof -i :5003  # Find process on port 5003
kill -9 <PID>
```

### Issue: Import error when starting server

**Error:**
```
ModuleNotFoundError: No module named 'ae_server'
```

**Cause:** Wrong working directory

**Solution:**
```bash
# Must be in submit/src directory
cd <challenge>/submit/src
uvicorn <challenge>_server:app --port <PORT>
```

### Issue: Model file not found

**Error:**
```
FileNotFoundError: model.pt not found
```

**Solutions:**
```python
# Option 1: Hardcode path
model = load_model("../models/best.pt")

# Option 2: Use environment variable
import os
model_path = os.getenv("MODEL_PATH", "../models/best.pt")
model = load_model(model_path)

# Option 3: Download if missing
from pathlib import Path
if not Path("model.pt").exists():
    download_model()
```

### Issue: CUDA out of memory

**Error:**
```
RuntimeError: CUDA out of memory
```

**Solutions:**
```python
# Reduce batch size
batch_size = 8  # Was 16

# Reduce model size
model = YOLO("yolov8s.pt")  # Small instead of large

# Clear cache
import torch
torch.cuda.empty_cache()
```

### Issue: Wrong output format

**Error:**
```
predictions not found in response
```

**Solution:**
1. Check your manager returns correct format
2. Compare with spec in `<challenge>/README.md`
3. Test locally with known input:
   ```python
   manager = <Challenge>Manager()
   result = manager.predict(test_obs)
   assert "predictions" in result
   assert isinstance(result["predictions"], list)
   ```

---

## Docker Issues

### Issue: Docker build fails

**Error:**
```
ERROR: failed to solve with frontend dockerfile.v0
```

**Solutions:**
```bash
# Check Dockerfile syntax
docker build --no-cache -t til-test .

# See detailed error
docker build -t til-test . 2>&1 | head -50

# Verify requirements.txt exists
ls -la <challenge>/submit/requirements.txt
```

### Issue: Container starts but server doesn't respond

**Error:**
```
Connection refused on localhost:5003
```

**Solutions:**
```bash
# Check container is running
docker ps | grep til-

# Check logs
docker logs <container-id>

# Ensure port is exposed
docker run -p 5003:5003 til-ae

# Test health endpoint
curl http://localhost:5003/health
```

### Issue: GPU not detected in container

**Error:**
```
CUDA is not available
```

**Solutions:**
```bash
# Use --gpus flag
docker run --gpus all -p 5003:5003 til-ae

# Verify NVIDIA runtime
docker run --rm --gpus all nvidia/cuda:11.8.0 nvidia-smi
```

### Issue: Container uses too much memory

**Error:**
```
Container memory limit exceeded
```

**Solutions:**
```bash
# Set memory limit
docker run -m 4g til-ae

# Reduce batch size via env var
docker run -e BATCH_SIZE=4 til-ae

# Profile memory usage
docker stats <container-id>
```

---

## Performance Issues

### Issue: Slow inference

**Problem:** Takes > 1 second per image/query

**Solutions:**
```python
# Profile code
import time
start = time.time()
result = model.predict(input)
print(f"Took {time.time() - start:.3f}s")

# Optimize:
# 1. Use smaller model
# 2. Enable batch processing
# 3. Quantize model (int8)
# 4. Reduce input size
```

### Issue: Training is too slow

**Solutions:**
```python
# Use GPU
device = torch.device("cuda:0")

# Mixed precision training
from torch.cuda.amp import autocast
with autocast():
    loss = model(input)

# Reduce dataset size for testing
dataset = dataset[:1000]  # Quick test

# Use smaller model for prototyping
model = YOLO("yolov8s")  # Small, not large
```

---

## Submission Issues

### Issue: Submission rejected - invalid format

**Error:**
```
Response format invalid
```

**Solution:**
```bash
# Validate locally first
curl -X POST http://localhost:5002/cv \
  -d @test_payload.json > response.json

python << 'EOF'
import json
with open("response.json") as f:
    data = json.load(f)
    
assert "predictions" in data, "Missing predictions key"
assert isinstance(data["predictions"], list), "Predictions must be list"
print("✓ Format valid")
EOF
```

### Issue: Submission times out

**Error:**
```
Submission exceeded time limit
```

**Solutions:**
```python
# Optimize inference:
# 1. Reduce model size
# 2. Lower batch size
# 3. Use quantization
# 4. Cache expensive computations

# Example - cache embeddings
from functools import lru_cache

@lru_cache(maxsize=1000)
def get_embedding(text):
    return model.encode(text)
```

### Issue: Wrong predictions

**Error:**
```
Accuracy too low on leaderboard
```

**Solutions:**
```python
# Debug locally
manager = MyManager()
pred = manager.predict(test_obs)

# Compare with baseline
baseline_pred = baseline_manager.predict(test_obs)

# If different, debug your logic
# If same, then training issue
```

---

## Debugging Techniques

### Add Logging

```python
import logging

logging.basicConfig(level=logging.DEBUG)
logger = logging.getLogger(__name__)

logger.debug(f"Input shape: {input.shape}")
logger.info(f"Prediction: {pred}")
logger.error(f"Error: {e}")
```

### Use Print Debugging

```python
def predict(self, input):
    print(f"Input: {type(input)}, {len(input)}")
    
    result = self.model(input)
    print(f"Model output: {type(result)}, {result.shape}")
    
    formatted = self.format(result)
    print(f"Formatted: {formatted}")
    
    return formatted
```

### Test in Steps

```bash
# Step 1: Can we import?
python -c "from my_manager import Manager; print('✓')"

# Step 2: Can we load model?
python -c "mgr = Manager(); print('✓')"

# Step 3: Can we predict?
python -c "mgr = Manager(); r = mgr.predict({}); print('✓')"

# Step 4: Is format correct?
python -c "import json; assert 'predictions' in json.load(open('output.json'))"
```

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Venv not activated | Run `source venv/bin/activate` |
| Working in wrong dir | `cd` to challenge directory |
| Model path hardcoded | Use env var or config file |
| Forgot to commit code | `git add .` then `git commit` |
| Docker image too large | Use `.dockerignore` to exclude files |
| Forgot to test locally | Always test before submitting |
| Wrong port number | Check `<challenge>/submit/README.md` |
| Batch size too large | Reduce in config or env var |

---

## Getting Help

**Can't find answer here?**

1. **Check documentation:**
   - `<challenge>/README.md` — Interface specs
   - `<challenge>/AGENTS.md` — Developer guide
   - Discord #hackoverflow

2. **Search online:**
   - GitHub issues for similar projects
   - Stack Overflow for error messages
   - Project documentation

3. **Ask for help:**
   - Discord #hackoverflow channel
   - Include: error message, code snippet, what you've tried

---

**Remember:** Describe the problem clearly, include error messages, and explain what you've already tried!
