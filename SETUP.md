# Detailed Setup Guide

Complete step-by-step instructions for setting up the TIL-26 environment.

## Prerequisites

- **Python 3.10 or newer** (check with `python --version`)
- **Git** with SSH configured (for cloning submodules)
- **pip** or **conda** (package managers)
- **Docker** (optional, but recommended for submission testing)
- **CUDA 11.8+** (optional, for GPU-accelerated training)

## Option A: Standard Virtual Environment (Recommended)

### 1. Clone Repository

```bash
git clone <your-repo-url>
cd TIL-26
```

### 2. Initialize Git Submodules

```bash
git submodule update --init --recursive
```

Verify with:
```bash
git submodule status
# Should show all submodules with commit hashes
```

### 3. Create Virtual Environment

```bash
python -m venv venv
source venv/bin/activate     # Linux/Mac
# OR
venv\Scripts\activate         # Windows
```

Verify:
```bash
which python  # or where python (Windows)
# Should point to venv/bin/python
```

### 4. Install Base Dependencies

```bash
pip install --upgrade pip setuptools wheel
```

### 5. Install Challenge-Specific Dependencies

**For AE (Agent Environment):**
```bash
cd ae
pip install -e til-26-ae
cd ..
```

**For CV (Computer Vision):**
```bash
pip install -r cv/train/requirements.txt
pip install -r cv/submit/requirements.txt
```

**For NLP (Natural Language Processing):**
```bash
pip install -r nlp/train/requirements.txt
pip install -r nlp/submit/requirements.txt
```

**For all challenges:**
```bash
pip install -r ae/requirements.txt
pip install -r cv/train/requirements.txt
pip install -r nlp/train/requirements.txt
```

### 6. Verify Installation

See [VERIFY.md](./VERIFY.md) for complete validation steps.

Quick check:
```bash
python -c "import til_environment; print('✓ AE')"
python -c "import ultralytics; print('✓ CV')"
python -c "import torch; print('✓ NLP')"
```

---

## Option B: Conda Environment

### 1-2. Same as above (clone and init submodules)

### 3. Create Conda Environment

```bash
conda create --name til-26 python=3.13
conda activate til-26
```

### 4-6. Same as above (install dependencies)

---

## Option C: Docker (For Submission Testing)

See [DOCKER.md](./DOCKER.md) for full instructions.

Quick test:
```bash
cd cv/submit
docker build -t til-cv .
docker run --rm -p 5002:5002 --gpus all til-cv
```

---

## Troubleshooting Setup Issues

### Issue: `git submodule` commands fail

**Solution:** Configure SSH for GitHub
```bash
git config --global url."git@github.com:".insteadOf "https://github.com/"
```

### Issue: `ModuleNotFoundError` after installation

**Solution:** Reinstall the specific package
```bash
pip install -r <challenge>/requirements.txt
pip install -e ae/til-26-ae  # For AE only
```

### Issue: Python version too old

**Solution:** Install Python 3.10+
```bash
python3.13 -m venv venv      # Use explicit version
source venv/bin/activate
```

### Issue: CUDA not found during training

**Solution:** Install NVIDIA drivers and CUDA toolkit, OR use CPU-only mode
```bash
# In training config YAML:
device: cpu
```

### Issue: Slow package installation

**Solution:** Use faster index
```bash
pip install --index-url https://pypi.org/simple/ -r requirements.txt
```

---

## Environment Variables (Optional)

Set these for convenience:

```bash
# GPU memory optimization
export PYTORCH_CUDA_ALLOC_CONF=expandable_segments:True

# Custom data paths
export DATASET_DIR=/path/to/dataset
export RUNS_DIR=/path/to/runs

# Debugging
export DEBUG=1
export CUDA_VISIBLE_DEVICES=0  # Use specific GPU
```

---

## Verify Your Setup

After installation, run:

```bash
# See VERIFY.md for complete checklist
```

Or the quick test:
```bash
# AE test
cd ae && python benchmarks/bench_diag.py && cd ..

# CV test
cd cv && python -c "import yaml; yaml.safe_load(open('train/config/yolov8l-v3.yaml'))" && cd ..

# NLP test
cd nlp && python -c "import torch; print('✓')" && cd ..
```

---

## Next Steps

- Read [CHALLENGES.md](./CHALLENGES.md) to pick a challenge
- Follow [DEVELOPMENT.md](./DEVELOPMENT.md) for your workflow
- Use [VERIFY.md](./VERIFY.md) to validate everything works
- Check [TROUBLESHOOTING.md](./TROUBLESHOOTING.md) if issues arise

---

## Alternative: Use Existing Setup

If you're on GCP Workbench:
```bash
# Environment pre-configured, just clone and init submodules
git submodule update --init --recursive
```

See [RESOURCES.md](./RESOURCES.md) for links to competition infrastructure.
