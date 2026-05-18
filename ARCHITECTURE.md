# Repository Architecture

Understanding the TIL-26 repository structure.

## Overview

```
TIL-26/
├── readme/                          # Documentation hub (you are here)
├── GETTING_STARTED.md              # Main project overview
├── VERIFY_SETUP.md                 # Setup validation
│
├── ae/                             # Agent Environment challenge
│   ├── README.md                   # Challenge interface
│   ├── AGENTS.md                   # Developer guide
│   ├── src/                        # Manager implementations
│   ├── submit/                     # Submission service (submodule)
│   ├── til-26-ae/                  # Environment package (submodule)
│   ├── benchmarks/                 # Testing tools
│   └── visualisers/                # Debugging tools
│
├── cv/                             # Computer Vision challenge
│   ├── README.md                   # Challenge interface
│   ├── AGENTS.md                   # Developer guide
│   ├── config/                     # Training configs
│   ├── submit/                     # Submission service (submodule)
│   ├── train/                      # Training pipeline (submodule)
│   └── template/                   # Starter template
│
├── nlp/                            # NLP challenge
│   ├── README.md                   # Challenge interface
│   ├── AGENTS.md                   # Developer guide
│   ├── submit/                     # Submission service (submodule)
│   └── train/                      # Training pipeline (submodule)
│
└── til-26-openlarp/                # Finals integration
    ├── README.md                   # Integration guide
    ├── AGENTS.md                   # Developer guide
    ├── ae/, cv/, nlp/              # Subdirectories for each challenge
    ├── asr/, noise/                # Additional challenges
    └── requirements-dev.txt        # Development dependencies
```

## Directory Types

### Challenge Directories (ae/, cv/, nlp/)

Each challenge has:

```
<challenge>/
├── README.md                       # Challenge specs + interface
├── AGENTS.md                       # Developer guide + how-to
├── submit/                         # Submission service
│   ├── README.md                   # Setup for submission
│   ├── Dockerfile                  # Container definition
│   ├── requirements.txt            # Dependencies
│   ├── src/
│   │   ├── <challenge>_server.py   # FastAPI server
│   │   └── <challenge>_manager.py  # Your implementation
│   └── .git/                       # Separate git repo (submodule)
├── train/                          # Training pipeline (AE/NLP skip this)
│   ├── README.md                   # Training guide
│   ├── requirements.txt            # Training dependencies
│   ├── train_*.py                  # Training scripts
│   ├── config/                     # Config templates
│   └── .git/                       # Separate git repo (submodule)
├── config/                         # Training configs (CV only)
├── benchmarks/                     # Testing tools (AE only)
├── visualisers/                    # Debug tools (AE only)
└── <submodule>/                    # Environment package (AE only)
```

## Git Submodule Structure

```
TIL-26 (root repo)
├── ae/submit           → Points to external repo
├── ae/til-26-ae        → Points to external repo (environment)
├── cv/submit           → Points to external repo
├── cv/train            → Points to external repo
├── nlp/submit          → Points to external repo
├── nlp/train           → Points to external repo (or local)
└── til-26-openlarp/    → Separate repo
    ├── til-26-ae       → Points to external repo
    └── til-26-finals   → Points to external repo
```

Each submodule is a **separate git repository** tracked independently.

## Workflow Directories

### Submission Service Structure

```
<challenge>/submit/
├── Dockerfile                      # Container spec
├── requirements.txt                # Python deps (FastAPI, models)
├── requirements-export.txt         # Export deps (CV only)
├── src/
│   ├── <challenge>_server.py       # FastAPI server (don't change)
│   ├── <challenge>_manager.py      # Your implementation (EDIT THIS)
│   ├── <model>_manager.py          # Alt implementations
│   └── __pycache__/                # Compiled Python
├── build.sh                        # Build script
├── export-*.sh                     # Export scripts
├── pyproject.toml                  # Project metadata
└── .git/                           # Git history
```

**Key file to edit:** `src/<challenge>_manager.py`

### Training Pipeline Structure

```
<challenge>/train/
├── README.md                       # Training guide
├── requirements.txt                # Python deps
├── train/
│   ├── train_yolo.py               # Trainer script
│   ├── train_rtdetr.py             # Alternative trainer
│   └── train_rfdetr.py             # Alternative trainer
├── config/
│   ├── yolov8l-v3.yaml             # Config for YOLO
│   ├── rtdetr-v5.yaml              # Config for RT-DETR
│   └── rfdetr-v5.yaml              # Config for RF-DETR
├── setup-env.sh                    # Environment setup
├── setup-data.sh                   # Data preparation
├── setup-gpu-watch.sh              # GPU monitoring setup
├── setup-reset.sh                  # Reset script
├── resize_dataset.py               # Image resizing utility
└── .git/                           # Git history
```

**Key files to customize:** `config/*.yaml`

## Communication Paths

### HTTP Interfaces

Each service runs on a specific port:

```
Client Request
    ↓
HTTP POST /health              (Health check)
HTTP POST /<challenge>         (Main endpoint)
    ↓
FastAPI Server (<challenge>_server.py)
    ↓
Manager Class (<challenge>_manager.py)
    ↓
Model/Agent
    ↓
Response (JSON)
```

**Ports:**
- AE: 5003
- CV: 5002
- NLP: 5004

### Request/Response Format

```
Request:
POST /<challenge>
{
  "instances": [
    { "observation": [...] }  # AE
    OR { "b64": "..." }       # CV
    OR { "question": "..." }  # NLP
  ]
}

Response:
{
  "predictions": [...]         # Shape matches instances
}
```

## Data Flow

### AE Data Flow
```
PettingZoo Environment
    ↓
Generate Observation
    ↓
Convert to JSON
    ↓
HTTP Request → Server
    ↓
AEManager.predict(obs)
    ↓
Return action index (0-5)
    ↓
Apply action in environment
```

### CV Data Flow
```
Input Image
    ↓
Encode to Base64
    ↓
HTTP Request → Server
    ↓
CVManager.predict(b64_image)
    ↓
Decode image
    ↓
Run model (YOLO/DETR)
    ↓
Format detections
    ↓
Return JSON
```

### NLP Data Flow
```
Step 1: Load Documents
  Documents → Server → NLPManager.load_documents()
           → Create embeddings → Build vector store
           → Return "loaded"

Step 2: Answer Questions
  Questions → Server → NLPManager.answer(question)
           → Retrieve relevant chunks
           → Call LLM with context
           → Return answer
```

## Configuration Hierarchy

```
Default Configs
    ↓
Challenge config (requirements.txt, config.yaml)
    ↓
User overrides (env vars, command-line args)
    ↓
Runtime values
```

**Example (CV):**
```bash
# Default: config/yolov8l-v3.yaml
# Override: BATCH_SIZE=8 python train_yolo.py
# Command: python train_yolo.py --batch-size 8
```

## Dependency Management

```
requirements.txt (pip)
    ├── Frozen versions for reproducibility
    ├── Lock files (uv.lock, poetry.lock) optional
    └── Dev dependencies in requirements-dev.txt

Environment Setup
    ├── Python 3.10+
    ├── Virtual environment (venv/conda)
    ├── pip install -r requirements.txt
    └── Challenge-specific: pip install -e ./path/
```

## Validation Pipeline

```
Code → Linting → Type Check → Unit Tests → Integration Tests
                                    ↓
                            Local Quickstart Tests
                                    ↓
                            Docker Build & Run
                                    ↓
                            HTTP Endpoint Test
                                    ↓
                            Submission
```

## Reproducibility

Each component is reproducible:

```
Same Code + Same Config + Same Seed = Same Results
```

**Considerations:**
- Fix random seeds (`np.random.seed(0)`, `torch.manual_seed(0)`)
- Freeze dependency versions
- Document environment (Python version, CUDA version)
- Provide configuration files

## Storage & Checkpoints

```
Local Development
└── ae/
    ├── submit/src/          # Code
    ├── til-26-ae/           # Environment
    ├── benchmarks/          # Tests
    └── models/ (optional)   # Local checkpoints

Training Artifacts
└── cv/train/
    ├── runs/                # Trained models
    ├── logs/                # TensorBoard logs
    └── models/              # Exported checkpoints
```

## Security Considerations

**What NOT to commit:**
- Model checkpoints (too large)
- API keys or credentials
- Training data
- `.venv/` directory
- Large binary files

**What TO commit:**
- Source code
- Configuration files
- Scripts and utilities
- Documentation

## Scaling Notes

For production deployment:

```
Single Machine (Development)
└── AE: 5003, CV: 5002, NLP: 5004
    All on localhost

Distributed (Production)
├── Load Balancer
├── AE Service Cluster (multiple replicas)
├── CV Service Cluster (GPU-enabled)
├── NLP Service Cluster
└── Shared Storage (models, checkpoints)
```

---

**Key Takeaway:** Each challenge is modular. You can develop and deploy them independently or integrate them together for Finals.
