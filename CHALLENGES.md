# All Challenges Explained

TIL-26 consists of 4 distinct challenge tracks. Pick one (or all!) to work on.

## 🤖 AE — Agent Environment (Bomberman)

**Type:** Multi-agent Reinforcement Learning  
**Goal:** Train agents to play Bomberman using deep RL  
**Port:** 5003

### What You'll Do
- Train agents using PettingZoo AEC environment
- Implement agent strategy (policies, action selection)
- Benchmark against other agents
- Submit trained model for evaluation

### Key Technologies
- Python, PyTorch, PettingZoo
- Reinforcement learning (PPO, DQN, etc.)
- Multi-agent environments

### Difficulty Level
🟢 Beginner (good starting point — visual, concrete)

### Quick Start
```bash
cd ae
pip install -e til-26-ae
python benchmarks/bench_diag.py  # See it in action!
```

**Full guide:** [CHALLENGES/ae.md](./CHALLENGES/ae.md)

---

## 👁️ CV — Computer Vision (Object Detection)

**Type:** Image Classification & Localization  
**Goal:** Detect and classify objects in aerial imagery  
**Port:** 5002

### What You'll Do
- Train object detection models (YOLOv8, RT-DETR, RF-DETR)
- Work with COCO-format annotations
- Optimize for speed and accuracy trade-off
- Submit trained model for evaluation

### Key Technologies
- Python, PyTorch, Ultralytics
- Computer vision (YOLO, DETR)
- Image processing, data augmentation

### Difficulty Level
🟡 Intermediate (good balance of complexity)

### Quick Start
```bash
cd cv
pip install -r train/requirements.txt
python train/train_yolo.py --config train/config/yolov8l-v3.yaml
```

**Full guide:** [CHALLENGES/cv.md](./CHALLENGES/cv.md)

---

## 📚 NLP — Natural Language Processing (RAG QA)

**Type:** Question Answering with Retrieval-Augmented Generation  
**Goal:** Build a RAG system to answer questions about documents  
**Port:** 5004

### What You'll Do
- Implement retrieval (document search/ranking)
- Implement generation (LLM-based answering)
- Handle document loading and chunking
- Submit RAG system for evaluation

### Key Technologies
- Python, LangChain, HuggingFace
- LLMs, embeddings, vector search
- Text processing, information retrieval

### Difficulty Level
🔴 Advanced (requires LLM knowledge)

### Quick Start
```bash
cd nlp
pip install -r train/requirements.txt
# Read AGENTS.md for RAG implementation guide
```

**Full guide:** [CHALLENGES/nlp.md](./CHALLENGES/nlp.md)

---

## 🏆 Finals — Integration Challenge

**Type:** Multi-task Integration  
**Goal:** Combine agents from all challenges into a unified system  
**Port:** N/A (orchestration)

### What You'll Do
- Integrate AE, CV, NLP components
- Handle inter-system communication
- Optimize for overall performance
- Submit integrated system

### Key Technologies
- Everything from above challenges
- System integration, API design
- Performance optimization

### Difficulty Level
🔴 Advanced (requires mastery of other challenges)

### Quick Start
```bash
cd til-26-openlarp
# See README.md for integration guide
```

**Full guide:** [CHALLENGES/finals.md](./CHALLENGES/finals.md)

---

## 📊 Challenge Comparison

| Feature | AE | CV | NLP | Finals |
|---------|----|----|-----|--------|
| **Type** | RL | Vision | Language | Integration |
| **Difficulty** | 🟢 | 🟡 | 🔴 | 🔴 |
| **Port** | 5003 | 5002 | 5004 | - |
| **Training Time** | Minutes | Hours | Minutes | - |
| **GPU Required** | No | Yes | Maybe | Depends |
| **Startup Cost** | Low | Medium | Low | High |
| **Learning Curve** | Easy | Medium | Hard | Hard |
| **Implementation** | From scratch | Existing models | From scratch | Combine |

---

## 🎯 Which Challenge Should I Pick?

### I'm new to the competition
→ Start with **AE** (Bomberman is visual, concrete, quick to grasp)

### I know computer vision
→ Pick **CV** (YOLO/DETR are well-documented)

### I know NLP/LLMs
→ Pick **NLP** (RAG systems are cutting-edge)

### I'm experienced in all areas
→ Pick **Finals** (integrate everything)

### I want quick wins
→ Start with **AE**, then add **CV**

### I want the challenge
→ Pick **NLP** (most complex)

---

## 📈 Timeline Suggestion

**Week 1:** Pick one challenge, get baseline working  
**Week 2:** Improve your model/agent  
**Week 3:** Optimize and submit  
**Week 4:** Add a second challenge (if time permits)

**Advanced:** Start with Finals integration early

---

## 🚀 One-Challenge Path

Pick your challenge:

| Challenge | Next Steps |
|-----------|-----------|
| **AE** | 1. Read [CHALLENGES/ae.md](./CHALLENGES/ae.md) 2. Read `../ae/AGENTS.md` 3. Start in `ae/submit/src/ae_manager.py` |
| **CV** | 1. Read [CHALLENGES/cv.md](./CHALLENGES/cv.md) 2. Read `../cv/AGENTS.md` 3. Run training in `cv/train/` 4. Deploy in `cv/submit/` |
| **NLP** | 1. Read [CHALLENGES/nlp.md](./CHALLENGES/nlp.md) 2. Read `../nlp/AGENTS.md` 3. Implement RAG in `nlp/submit/src/nlp_manager.py` |
| **Finals** | 1. Read [CHALLENGES/finals.md](./CHALLENGES/finals.md) 2. Complete at least one other challenge first |

---

## 📚 Reading Order by Challenge

### For AE
1. This file (you are here)
2. [CHALLENGES/ae.md](./CHALLENGES/ae.md)
3. `../ae/README.md`
4. `../ae/AGENTS.md`
5. Start coding in `../ae/submit/src/`

### For CV
1. This file (you are here)
2. [CHALLENGES/cv.md](./CHALLENGES/cv.md)
3. `../cv/README.md`
4. `../cv/AGENTS.md`
5. Start training in `../cv/train/`

### For NLP
1. This file (you are here)
2. [CHALLENGES/nlp.md](./CHALLENGES/nlp.md)
3. `../nlp/README.md`
4. `../nlp/AGENTS.md`
5. Start coding in `../nlp/submit/src/`

### For Finals
1. Complete at least one challenge above first
2. This file (you are here)
3. [CHALLENGES/finals.md](./CHALLENGES/finals.md)
4. `../til-26-openlarp/README.md`
5. `../til-26-openlarp/AGENTS.md`

---

## 💡 Pro Tips

- **Read first, code second** — Each challenge has excellent docs
- **Test locally** — Run VERIFY.md before submitting
- **Use the Wiki** — https://github.com/til-ai/til-26/wiki has specs
- **Ask Discord** — #hackoverflow has helpful community
- **Start simple** — Get baseline working, then optimize

---

## 🔗 Quick Links

- **AE Details:** [CHALLENGES/ae.md](./CHALLENGES/ae.md)
- **CV Details:** [CHALLENGES/cv.md](./CHALLENGES/cv.md)
- **NLP Details:** [CHALLENGES/nlp.md](./CHALLENGES/nlp.md)
- **Finals Details:** [CHALLENGES/finals.md](./CHALLENGES/finals.md)
- **Development Guide:** [DEVELOPMENT.md](./DEVELOPMENT.md)
- **Submission Guide:** [SUBMISSION.md](./SUBMISSION.md)

---

Ready to pick? → Read your challenge's details file above!
