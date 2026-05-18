# TIL-26 Setup Verification Checklist

Use this checklist to verify your repository is properly set up and ready for development.

---

## ✅ Git & Submodules

- [ ] Repository cloned: `ls -la .git`
- [ ] Submodules initialized: `git submodule status` (no missing entries)
- [ ] All submodules point to valid commits:
  ```bash
  git submodule foreach 'echo "$name: $(git rev-parse --short HEAD)"'
  ```

**Expected output (should show commits, not errors):**
```
ae/submit: 0d3e936
ae/til-26-ae: 39ae862
cv/submit: 49409a0
cv/train: 9633a06
nlp/submit: 5d91695
nlp/train: ... (or part of main repo)
til-26-openlarp/til-26-ae: ...
til-26-openlarp/til-26-finals: ...
```

---

## ✅ Python Environment

- [ ] Python 3.10+: `python --version`
- [ ] Virtual environment created: `ls -la venv/` or `conda list`
- [ ] Virtual environment activated: `which python` points to venv
- [ ] Pip up-to-date: `pip --version`

---

## ✅ Documentation Files

### Main Documentation

- [ ] `GETTING_STARTED.md` exists and is readable
- [ ] `VERIFY_SETUP.md` exists (this file)
- [ ] `AUDIT_REPORT.md` exists with setup status

### Challenge Documentation

For each of **ae**, **cv**, **nlp**:

- [ ] `<challenge>/README.md` — Challenge overview
- [ ] `<challenge>/AGENTS.md` — Developer guide
- [ ] `<challenge>/submit/README.md` — Submission setup
- [ ] `<challenge>/train/README.md` — Training guide (if applicable)

Run this to check:

```bash
for dir in ae cv nlp; do
  echo "=== $dir ===" 
  [ -f "$dir/README.md" ] && echo "✓ README.md" || echo "✗ README.md MISSING"
  [ -f "$dir/AGENTS.md" ] && echo "✓ AGENTS.md" || echo "✗ AGENTS.md MISSING"
  [ -f "$dir/submit/README.md" ] && echo "✓ submit/README.md" || echo "✗ submit/README.md MISSING"
done
```

---

## ✅ Dependencies Installation

### Install for AE

```bash
cd ae
pip install -e til-26-ae
cd ..
```

Check: `python -c "import til_environment; print('✓ AE environment loaded')"` ✓ or ✗

### Install for CV (Training)

```bash
pip install -r cv/train/requirements.txt
```

Check: `python -c "import ultralytics; print('✓ YOLO loaded')"` ✓ or ✗

### Install for CV (Submission)

```bash
pip install -r cv/submit/requirements.txt
```

Check: `python -c "import fastapi; print('✓ FastAPI loaded')"` ✓ or ✗

### Install for NLP

```bash
pip install -r nlp/train/requirements.txt
```

Check: `python -c "import torch; print('✓ PyTorch loaded')"` ✓ or ✗

---

## ✅ Challenge Quickstarts

### AE (Agent Environment)

```bash
cd ae
python benchmarks/bench_diag.py
```

Expected: Game simulation runs and prints results (should take <10 seconds) ✓ or ✗

### CV (Computer Vision)

Verify training setup (no GPU required for validation):

```bash
cd cv/train
python -c "import yaml; yaml.safe_load(open('config/yolov8l-v3.yaml'))" && echo "✓ Config loads"
```

Expected: "✓ Config loads" message ✓ or ✗

### NLP (Natural Language Processing)

Check submission server can import:

```bash
cd nlp/submit/src
python -c "from nlp_server import app; print('✓ NLP server loads')"
```

Expected: "✓ NLP server loads" message ✓ or ✗

---

## ✅ Submission Servers (HTTP)

For each challenge, start the server and test health endpoint.

### Terminal 1: Start AE Server

```bash
cd ae/submit/src
uvicorn ae_server:app --port 5003
```

### Terminal 2: Test AE

```bash
curl http://localhost:5003/health
# Expected: 200 OK with some response
```

### Repeat for CV and NLP

```bash
# CV (port 5002)
cd cv/submit/src
uvicorn cv_server:app --port 5002

# NLP (port 5004)
cd nlp/submit/src
uvicorn nlp_server:app --port 5004
```

All should respond to `curl http://localhost:<PORT>/health` ✓ or ✗

---

## ✅ Docker (Optional but Recommended)

For each challenge:

```bash
cd <challenge>/submit
docker build -t til-<challenge> .
# Should complete without errors
```

Expected: "Successfully tagged til-<challenge>" ✓ or ✗

Test running container:

```bash
docker run --rm -p 5003:5003 til-ae
# Ctrl+C to stop

# In another terminal:
curl http://localhost:5003/health
```

Expected: Server starts and responds to health check ✓ or ✗

---

## ✅ Git History (Verify Commits Exist)

All submodules should have recent commits:

```bash
for dir in ae cv nlp; do
  echo "=== $dir ===" 
  cd "$dir"
  echo "Commit count: $(git rev-list --all --count)"
  echo "Latest: $(git log -1 --oneline)"
  cd ..
done
```

Expected: Each shows "Commit count: > 5" and recent commits ✓ or ✗

---

## 🔧 Troubleshooting Guide

| Issue | Check | Fix |
|-------|-------|-----|
| `git submodule` commands fail | SSH key setup | `git config --global url."git@github.com:".insteadOf "https://github.com/"` |
| Module not found errors | Python path | `pip install -e <dir>` for AE; `pip install -r <dir>/requirements.txt` for others |
| Port already in use | `lsof -i :<PORT>` or `netstat -ano` | Kill process or use different port |
| Docker build fails | Docker installed? | `docker --version` |
| CUDA not found | GPU drivers | `nvidia-smi` (NVIDIA) or skip GPU (CPU-only) |
| Slow training | GPU memory | Reduce batch size in config YAML |

---

## 📊 Summary Template

Use this to record your setup status:

```markdown
### TIL-26 Setup Status

Date: ____

**Environment**
- [ ] Python 3.10+
- [ ] Venv activated
- [ ] Git submodules initialized

**Documentation**
- [ ] GETTING_STARTED.md read
- [ ] Challenge READMEs reviewed

**Dependencies**
- [ ] AE installed
- [ ] CV installed
- [ ] NLP installed

**Testing**
- [ ] AE quickstart ran
- [ ] CV config validates
- [ ] NLP server imports
- [ ] Submission servers start (5003, 5002, 5004)

**Next Steps**
- [ ] Pick a challenge
- [ ] Start developing
- [ ] Submit when ready
```

---

## 🆘 Still Stuck?

1. Check the **Wiki**: https://github.com/til-ai/til-26/wiki
2. Ask in **Discord**: #hackoverflow
3. Review **AGENTS.md** in the challenge directory
4. Re-read **GETTING_STARTED.md** (this repo's overview)

Good luck! 🚀
