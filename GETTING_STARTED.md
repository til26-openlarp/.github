# DSTA TIL-26 — Getting Started Guide

Welcome to the TIL-26 (Training Intelligence Learning) competition! This repository contains four challenge tracks, each with training and submission components.

## 📋 Project Overview

```
TIL-26/
├── ae/                   Agent Environment (Bomberman multi-agent RL)
├── cv/                   Computer Vision (Object detection on aerial imagery)
├── nlp/                  Natural Language Processing (RAG question answering)
├── til-26-openlarp/      Finals integration (combines all challenges)
└── GETTING_STARTED.md    This file
```

Each challenge is a **separate git repository** with its own:
- Training pipeline (`train/` submodule)
- Submission service (`submit/` submodule)
- Documentation (README.md, AGENTS.md)

---

## 🚀 Quick Start (All Challenges)

### 1. Clone and Initialize

```bash
git clone <your-repo-url>
cd TIL-26
git submodule update --init
```

This pulls all training and submission submodules.

### 2. Create Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

Or with conda:

```bash
conda create --name til-26 python=3.13
conda activate til-26
```

### 3. Install Dependencies

For development across all challenges:

```bash
# Install all challenge requirements
pip install -r ae/requirements.txt
pip install -r cv/train/requirements.txt
pip install -r nlp/train/requirements.txt
```

Or install one challenge at a time as you work on it.

---

## 🎯 Challenge Quick Starts

### Agent Environment (AE)

**Goal:** Train agents to play Bomberman using reinforcement learning.

```bash
cd ae
pip install -e til-26-ae  # Install environment package
python benchmarks/bench_diag.py  # Run a single-game benchmark
```

📖 **Docs:**
- [ae/README.md](ae/README.md) — Challenge overview and interface
- [ae/AGENTS.md](ae/AGENTS.md) — Environment reference and agent guide
- [ae/submit/README.md](ae/submit/README.md) — Submission server setup

**Submission server port:** 5003

---

### Computer Vision (CV)

**Goal:** Train object detection models on aerial imagery.

```bash
cd cv
pip install -r train/requirements.txt
python train/train_yolo.py --config train/config/yolov8l-v3.yaml
```

📖 **Docs:**
- [cv/README.md](cv/README.md) — Challenge overview and interface
- [cv/AGENTS.md](cv/AGENTS.md) — Training guide with model details
- [cv/submit/README.md](cv/submit/README.md) — Submission service setup
- [cv/train/README.md](cv/train/README.md) — Training pipeline explained

**Submission server port:** 5002

**Models:** YOLOv8/v11, RT-DETR, RF-DETR

---

### Natural Language Processing (NLP)

**Goal:** Build a RAG question-answering system.

```bash
cd nlp
pip install -r train/requirements.txt
python train/train.py  # Or use your own training script
```

📖 **Docs:**
- [nlp/README.md](nlp/README.md) — Challenge overview and interface
- [nlp/AGENTS.md](nlp/AGENTS.md) — RAG system development guide
- [nlp/submit/README.md](nlp/submit/README.md) — Submission service setup
- [nlp/train/README.md](nlp/train/README.md) — Training pipeline

**Submission server port:** 5004

---

### Finals / Integration (TIL-26-OpenLarp)

**Goal:** Combine agents from all challenges into a unified system.

```bash
cd til-26-openlarp
pip install -r requirements-dev.txt
# See README.md for specific instructions
```

📖 **Docs:**
- [til-26-openlarp/README.md](til-26-openlarp/README.md) — Integration guide

---

## 📂 Repository Structure Explained

### Training Directories (`train/`)

Each challenge has a `train/` submodule containing:
- Training scripts (`train_*.py`)
- Configuration files (`config/*.yaml`)
- Dataset utilities
- Model evaluation scripts

**To work on training:**

```bash
cd <challenge>/train
python train_*.py --config config/<model>.yaml
```

### Submission Directories (`submit/`)

Each challenge has a `submit/` submodule containing:
- FastAPI server (`src/*_server.py`)
- Model managers (`src/*_manager.py`)
- Dockerfile for containerized deployment
- Requirements for the submission environment

**To run a submission server locally:**

```bash
cd <challenge>/submit/src
uvicorn <challenge>_server:app --port <PORT>
```

### Documentation Files

Every directory with code has:

- **README.md** — Quick start, interface specs, example usage
- **AGENTS.md** — Detailed developer guide and API reference
- **requirements.txt** — Python dependencies for that component

---

## 🐳 Docker Deployment

### Build a Challenge Container

```bash
cd <challenge>/submit
docker build -t til-<challenge> .
```

### Run Locally

```bash
docker run --rm -p <PORT>:<PORT> --gpus all til-<challenge>
```

**Port mapping:**
- AE: 5003
- CV: 5002
- NLP: 5004

### Submit to Cloud

See [til-26/wiki](https://github.com/til-ai/til-26/wiki) for official submission instructions on GCP.

---

## 📝 Common Workflows

### Develop a New Agent / Model

1. Pick a challenge directory
2. Read `AGENTS.md` to understand the requirements
3. Edit the manager class in `submit/src/*_manager.py`
4. Test locally with the submission server
5. Build Docker image and test in container
6. Submit via the competition platform

### Train a Model

1. Go to the challenge's `train/` directory
2. Install training dependencies
3. Configure `config/*.yaml` as needed
4. Run training script: `python train_*.py --config config/<model>.yaml`
5. Monitor with TensorBoard or logs
6. Export and integrate checkpoint into submission

### Test Locally Before Submitting

```bash
# Terminal 1: Run submission server
cd <challenge>/submit/src
uvicorn <challenge>_server:app --port <PORT>

# Terminal 2: Send test request
curl -X POST http://localhost:<PORT>/<challenge> \
  -H "Content-Type: application/json" \
  -d @test_payload.json
```

---

## 🔗 Key Links

- **Wiki (Detailed Specs):** https://github.com/til-ai/til-26/wiki
- **Challenge Specs:** https://github.com/til-ai/til-26/wiki/Challenge-specifications
- **Discord (#hackoverflow):** For support and Q&A
- **Leaderboard:** https://tribegroup.notion.site/... (Strategist's Handbook)
- **Curriculum:** Google Drive (shared in competition email)

---

## ✅ Pre-Submission Checklist

Before submitting, ensure:

- [ ] All submodules are initialized (`git submodule update --init`)
- [ ] Python 3.10+ is installed
- [ ] Dependencies installed: `pip install -r requirements.txt` (in each subdir)
- [ ] Your manager class correctly imports and runs
- [ ] Health check passes: `curl http://localhost:<PORT>/health`
- [ ] Test request returns correct JSON format
- [ ] Docker image builds without errors
- [ ] Docker container runs and accepts requests on the correct port
- [ ] Model files are included or loaded from the correct path

---

## 🛠️ Troubleshooting

### `git submodule update` fails

Ensure you have permissions to access submodule repositories. If using SSH:

```bash
git config --global url."git@github.com:".insteadOf "https://github.com/"
```

### Module not found errors

Reinstall the challenge's package:

```bash
pip install -e <challenge>/til-26-<challenge>  # For AE
pip install -r <challenge>/requirements.txt    # For others
```

### Port already in use

Change port in your launch command:

```bash
uvicorn <challenge>_server:app --port 5099
```

### GPU not detected in Docker

Ensure NVIDIA Docker runtime is installed and pass `--gpus all`:

```bash
docker run --gpus all -p <PORT>:<PORT> til-<challenge>
```

### Out of memory during training

Reduce batch size in `config/*.yaml`:

```yaml
training:
  batch_size: 8  # was 16
```

---

## 📚 Learning Resources

Each challenge directory contains:

- **README.md** — Start here for interface specs
- **AGENTS.md** — Detailed developer guide
- Training/submission subdirectories with their own READMEs

For general TIL-26 info, see the **Wiki** linked above.

---

## 🎓 First Steps Recommendation

**If you're new to the competition:**

1. Read this file (you're doing it! ✓)
2. Pick one challenge (AE is simplest to visualize)
3. Read `<challenge>/README.md`
4. Read `<challenge>/AGENTS.md`
5. Run the quickstart commands
6. Read the submission service's README
7. Try the HTTP interface locally
8. Build a Docker image
9. Move on to the next challenge

**Good luck!** 🚀
