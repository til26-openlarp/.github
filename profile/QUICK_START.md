# Quick Start — 5 Minutes to Code

Get TIL-26 running in under 5 minutes.

## Step 1: Clone & Initialize (1 min)

```bash
git clone <your-repo-url>
cd TIL-26
git submodule update --init
```

## Step 2: Create Environment (2 min)

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

## Step 3: Pick a Challenge (1 min)

```bash
# Agent Environment (Bomberman RL)
cd ae
pip install -e til-26-ae
python benchmarks/bench_diag.py  # Run a quick test

# OR Computer Vision (Object Detection)
cd cv
pip install -r train/requirements.txt
python -c "import yaml; yaml.safe_load(open('train/config/yolov8l-v3.yaml'))"

# OR NLP (Question Answering)
cd nlp
pip install -r train/requirements.txt
python -c "import torch; print('✓ Ready')"
```

## Step 4: Start Developing (1 min)

- **Read:** `../<challenge>/README.md` (interface spec)
- **Learn:** `../<challenge>/AGENTS.md` (dev guide)
- **Code:** `<challenge>/submit/src/<challenge>_manager.py`
- **Train:** `<challenge>/train/train_*.py`

## Next Steps

- Full setup: Read [SETUP.md](./SETUP.md)
- All challenges: Read [CHALLENGES.md](./CHALLENGES.md)
- Challenge details: See [CHALLENGES/](./CHALLENGES) folder
- Submission: Read [SUBMISSION.md](./SUBMISSION.md)

**Time spent so far: ~5 minutes. You're ready to code!**
