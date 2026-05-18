# Submission Guide

How to submit your solutions for evaluation.

## Pre-Submission Checklist

### Code Quality
- [ ] Code tested locally
- [ ] No debug prints left
- [ ] Error handling in place
- [ ] Follows style guidelines
- [ ] Git history clean (meaningful commits)

### Functionality
- [ ] Server starts without errors
- [ ] Health endpoint responds (`/health`)
- [ ] Main endpoint responds (`/<challenge>`)
- [ ] Output format correct (matches spec)
- [ ] Handles edge cases (empty input, malformed data)

### Performance
- [ ] Inference time acceptable
- [ ] Memory usage reasonable
- [ ] Works with expected batch sizes
- [ ] GPU/CPU behavior documented

### Testing
- [ ] Local tests pass
- [ ] Test payload produces correct output
- [ ] Docker image builds
- [ ] Docker container runs
- [ ] Container responds to requests

---

## Submission Steps

### Step 1: Prepare Your Code

```bash
# AE
cd ae/submit/src
# Ensure ae_server.py and ae_manager.py are ready

# CV
cd cv/submit/src
# Ensure cv_server.py and cv_manager.py are ready

# NLP
cd nlp/submit/src
# Ensure nlp_server.py and nlp_manager.py are ready
```

### Step 2: Test Locally

```bash
# Start server
cd <challenge>/submit/src
uvicorn <challenge>_server:app --port <PORT>

# In another terminal, test health
curl http://localhost:<PORT>/health
# Expected: HTTP 200

# Test with sample payload
curl -X POST http://localhost:<PORT>/<challenge> \
  -H "Content-Type: application/json" \
  -d @test_payload.json
# Expected: HTTP 200 with valid JSON response
```

### Step 3: Validate Output Format

```bash
# Save response
curl -X POST http://localhost:<PORT>/<challenge> \
  -d @test_payload.json > response.json

# Check format
python << 'EOF'
import json
with open("response.json") as f:
    data = json.load(f)
    
# Must have 'predictions' key
assert "predictions" in data
# Predictions must be a list
assert isinstance(data["predictions"], list)
# Length must match input
print(f"✓ Format valid")
EOF
```

### Step 4: Build Docker Image

```bash
cd <challenge>/submit
docker build -t til-<challenge>:latest .
# Expected: "Successfully tagged til-<challenge>:latest"

# List images
docker image ls | grep til-<challenge>
```

### Step 5: Test Docker Container

```bash
# Run container
docker run --rm -p <PORT>:<PORT> --gpus all til-<challenge>:latest
# For CPU-only: docker run --rm -p <PORT>:<PORT> til-<challenge>:latest

# In another terminal, test
curl http://localhost:<PORT>/health

# Stop container (Ctrl+C)
```

### Step 6: Tag & Push (if using registry)

```bash
# Tag for registry
docker tag til-<challenge>:latest myregistry.com/til-<challenge>:latest

# Push (requires credentials)
docker push myregistry.com/til-<challenge>:latest
```

### Step 7: Submit via Competition Platform

**Follow official competition instructions:**

1. Go to competition submission portal
2. Select your challenge (AE, CV, NLP)
3. Upload Docker image OR provide registry link
4. Enter optional metadata (model name, hyperparameters, etc.)
5. Submit

---

## Troubleshooting Submission

### Issue: Docker Build Fails

**Error:** `ModuleNotFoundError`, `ImportError`, etc.

**Solution:**
```bash
# Check requirements.txt
cat <challenge>/submit/requirements.txt

# Verify all dependencies are listed
# Common missing:
# - fastapi, uvicorn
# - model libraries (torch, ultralytics, etc.)
# - utilities (numpy, PIL, etc.)

# Add to requirements.txt
echo "missing-package>=1.0.0" >> <challenge>/submit/requirements.txt

# Rebuild
docker build -t til-<challenge> .
```

### Issue: Container Starts But Server Doesn't Respond

**Error:** `Connection refused`, `No response`

**Solution:**
```bash
# Check container logs
docker run --rm til-<challenge> 2>&1 | tail -50

# Ensure correct port exposed
# In Dockerfile: EXPOSE <PORT>

# Check manager imports
docker run --rm til-<challenge> python -c "from src.<challenge>_manager import <Challenge>Manager; print('✓')"
```

### Issue: Model File Not Found

**Error:** `FileNotFoundError: model.pt`

**Solution:**
```bash
# Option 1: Include model in Docker image
# Add to Dockerfile: COPY model.pt /app/model.pt

# Option 2: Download at startup
# In manager __init__: download_model_if_needed()

# Option 3: Pass path as env var
docker run ... -e MODEL_PATH=/path/to/model.pt ...
```

### Issue: Response Format Invalid

**Error:** Wrong output shape, type, etc.

**Solution:**
```bash
# Test locally first
python << 'EOF'
from <challenge>_manager import <Challenge>Manager

manager = <Challenge>Manager()
response = manager.predict(test_input)

print(type(response))        # Should be dict
print(response.keys())       # Should have 'predictions'
print(type(response["predictions"]))  # Should be list
EOF

# Compare with spec
# See <challenge>/README.md for expected format
```

---

## Submission Tips

### For AE

- Test with multiple seeds (benchmarks do this)
- Ensure action is valid (0-5)
- Check agent doesn't crash on edge cases
- Submit early version, then iterate

### For CV

- Test on various image sizes
- Ensure bbox format is correct [x, y, w, h]
- Check category_id is valid
- Handle empty detections (return [])

### For NLP

- Test document loading with various text lengths
- Ensure "loaded" response is valid
- Test with questions not in documents
- Handle malformed input gracefully

---

## Iterative Submission

You can submit multiple times:

1. **First submission:** Early baseline
2. **Subsequent submissions:** Improvements
3. **Final submission:** Best version

**Strategy:**
- Submit early to establish baseline
- Iterate based on feedback/leaderboard
- Final push before deadline

---

## Submission Timeline

**Recommended:**
- Week 1: Get baseline working, submit
- Week 2: Improve model, submit update
- Week 3: Final optimizations, submit
- Week 4: Final day submissions (if improvements found)

---

## Common Submission Issues

| Issue | Cause | Fix |
|-------|-------|-----|
| Build fails | Missing dependency | Add to requirements.txt |
| Server won't start | Import error | Test locally first |
| Output format wrong | Mismatch with spec | Compare with README.md |
| Inference too slow | Model too large | Quantize, use smaller model |
| Out of memory | Batch too large | Reduce in container env var |
| Wrong predictions | Bug in manager | Add logging, test locally |

---

## Post-Submission

After submitting:

1. **Check leaderboard** — Where do you rank?
2. **Review feedback** — Did evaluators provide comments?
3. **Analyze performance** — Where can you improve?
4. **Plan next iteration** — What to work on?
5. **Resubmit** — Submit improved version

---

## Submission Metrics

You'll typically see:

- **Score:** Your challenge's metric (mAP for CV, wins for AE, etc.)
- **Rank:** Position on leaderboard
- **Time:** When evaluated
- **Status:** Accepted, error, timeout, etc.

---

## Final Checklist Before Deadline

- [ ] Code passes all local tests
- [ ] Docker builds and runs
- [ ] Output format valid
- [ ] Model loading works
- [ ] Performance acceptable
- [ ] No hardcoded paths
- [ ] Environment variables documented
- [ ] README updated with results
- [ ] Final submission made

---

**Good luck with your submission! 🚀**
