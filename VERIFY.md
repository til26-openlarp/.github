# Setup Verification Checklist

Use this to validate your TIL-26 setup before developing.

## ✅ Git & Submodules

- [ ] Repository cloned: `ls -la .git`
- [ ] Submodules initialized: `git submodule status` (no errors)
- [ ] All submodules have commits:
  ```bash
  git submodule foreach 'echo "$name: $(git rev-parse --short HEAD)"'
  ```

Expected: No "fatal" errors, all show commit hashes.

---

## ✅ Python Environment

- [ ] Python 3.10+: `python --version`
- [ ] Virtual environment created: `ls venv/` or `conda list`
- [ ] Venv activated: `which python` (should point to venv)
- [ ] Pip up-to-date: `pip --version`

---

## ✅ Dependencies

**AE:**
- [ ] `pip install -e ae/til-26-ae` succeeded
- [ ] Test: `python -c "import til_environment; print('✓')"` ✓

**CV:**
- [ ] `pip install -r cv/train/requirements.txt` succeeded
- [ ] Test: `python -c "import ultralytics; print('✓')"` ✓

**NLP:**
- [ ] `pip install -r nlp/train/requirements.txt` succeeded
- [ ] Test: `python -c "import torch; print('✓')"` ✓

---

## ✅ Documentation

For each **ae**, **cv**, **nlp**:
- [ ] `<challenge>/README.md` exists
- [ ] `<challenge>/AGENTS.md` exists
- [ ] `<challenge>/submit/README.md` exists
- [ ] `<challenge>/train/README.md` exists (if applicable)

Run this check:
```bash
for dir in ae cv nlp; do
  [ -f "$dir/README.md" ] && [ -f "$dir/AGENTS.md" ] && echo "$dir: ✓" || echo "$dir: ✗"
done
```

---

## ✅ Quickstart Tests

### AE Quickstart

```bash
cd ae
python benchmarks/bench_diag.py
# Expected: Outputs game diagnostics (should complete in <10s)
```

Check: Finished without errors? ✓ or ✗

### CV Quickstart

```bash
cd cv/train
python -c "import yaml; cfg = yaml.safe_load(open('config/yolov8l-v3.yaml')); print('✓ Config valid')"
```

Check: Printed "✓ Config valid"? ✓ or ✗

### NLP Quickstart

```bash
cd nlp/submit/src
python -c "from nlp_server import app; print('✓ Server loads')"
```

Check: Printed "✓ Server loads"? ✓ or ✗

---

## ✅ Submission Servers

For each challenge, test HTTP interface:

### Terminal 1: Start Server

```bash
# AE
cd ae/submit/src
uvicorn ae_server:app --port 5003

# OR CV
cd cv/submit/src
uvicorn cv_server:app --port 5002

# OR NLP
cd nlp/submit/src
uvicorn nlp_server:app --port 5004
```

### Terminal 2: Test Health Endpoint

```bash
curl http://localhost:5003/health
# Expected: HTTP 200 with response
```

Check: Got 200 OK? ✓ or ✗

---

## ✅ Docker (Optional)

For each challenge, test Docker build:

```bash
cd <challenge>/submit
docker build -t til-<challenge> .
# Expected: "Successfully tagged til-<challenge>"
```

Check: Build succeeded? ✓ or ✗

Test running container:

```bash
docker run --rm -p 5003:5003 til-ae
# In another terminal: curl http://localhost:5003/health
```

Check: Health endpoint responds? ✓ or ✗

---

## 🎯 Summary

Count your ✓ marks:

- **15-18 ✓**: Setup complete! Ready to code.
- **12-14 ✓**: Most setup done. Review failed items.
- **10-11 ✓**: Setup partly done. Read [SETUP.md](./SETUP.md) again.
- **<10 ✓**: Significant issues. See [TROUBLESHOOTING.md](./TROUBLESHOOTING.md).

---

## 🆘 Troubleshooting

| Issue | Check | Fix |
|-------|-------|-----|
| `git submodule` fails | SSH configured? | See [GIT_GUIDE.md](./GIT_GUIDE.md) |
| Module not found | Pip installed all? | `pip install -r <challenge>/requirements.txt` |
| Python version wrong | `python --version` | Install Python 3.10+ |
| Quickstart fails | Run from correct dir? | `cd` to challenge directory first |
| Server won't start | Port in use? | Use different port: `--port 5099` |
| Docker build fails | Docker installed? | `docker --version` |

More help: See [TROUBLESHOOTING.md](./TROUBLESHOOTING.md)

---

## Next Steps

✓ All checks passed?
→ Pick a challenge and read [DEVELOPMENT.md](./DEVELOPMENT.md)

Issues found?
→ Review [TROUBLESHOOTING.md](./TROUBLESHOOTING.md) or re-read [SETUP.md](./SETUP.md)

Ready to code?
→ Go to `<challenge>/README.md` then `<challenge>/AGENTS.md`
