# 🚀 TIL-26 Documentation Hub

> **Your complete guide to mastering the DSTA BrainHack TIL-AI 2026 competition**

<div align="center">

![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat-square)
![Files](https://img.shields.io/badge/Documentation-23%20Files-blue?style=flat-square)
![Size](https://img.shields.io/badge/Size-261%20KB-blue?style=flat-square)
![Updated](https://img.shields.io/badge/Updated-2026--05--19-blue?style=flat-square)

**[Quick Start](#-quick-start) • [Challenges](#-challenges) • [Resources](#-resources) • [API Docs](#-api) • [Help](#-troubleshooting)**

</div>

---

## 📖 What is This?

Your **complete, modern documentation hub** for TIL-26. Everything you need to succeed in the competition — from first-time setup to final submission.

### ✨ Key Highlights

- ⚡ **5-minute quick start** to get running
- 🎯 **4 independent challenges** with detailed guides
- 📚 **100+ pages** of comprehensive documentation
- 🔧 **API specifications** for all services
- 🐳 **Docker deployment** guides
- 🎓 **Best practices** and optimization tips
- 🆘 **Troubleshooting** solutions
- 🔗 **External resources** and links

---

## 🎯 Quick Start

**Get up and running in 5 minutes:**

<table>
<tr>
<td width="50%">

### 1️⃣ Clone & Setup
```bash
git clone <repo>
cd TIL-26
git submodule update --init
python -m venv venv
source venv/bin/activate
```

</td>
<td width="50%">

### 2️⃣ Install Dependencies
```bash
# Pick your challenge
pip install -r ae/requirements.txt
# OR
pip install -r cv/train/requirements.txt
# OR
pip install -r nlp/train/requirements.txt
```

</td>
</tr>
</table>

👉 **Full guide:** [QUICK_START.md](./QUICK_START.md)

---

## 🏆 Challenges

Choose your adventure. Each challenge is **independent** and **fully documented**.

### 🤖 Agent Environment (AE)
> **Multi-agent RL • Bomberman • Port 5003**

Train agents to play Bomberman using deep reinforcement learning. Visual, concrete, perfect for learning.

- **Difficulty:** 🟢 Beginner
- **Training Time:** Minutes to hours  
- **Best For:** Learning RL concepts
- **Quick Start:** `python ae/benchmarks/bench_diag.py`

👉 [Read AE Guide](./CHALLENGES/ae.md)

---

### 👁️ Computer Vision (CV)
> **Object Detection • YOLOv8/DETR • Port 5002**

Train object detection models on aerial imagery. Production-ready models, real-world applications.

- **Difficulty:** 🟡 Intermediate
- **Training Time:** Hours
- **Best For:** CV/ML engineers
- **Quick Start:** `python cv/train/train_yolo.py --config cv/train/config/yolov8l-v3.yaml`

👉 [Read CV Guide](./CHALLENGES/cv.md)

---

### 📚 Natural Language Processing (NLP)
> **RAG • Question Answering • Port 5004**

Build a retrieval-augmented generation system to answer questions. Cutting-edge LLM techniques.

- **Difficulty:** 🔴 Advanced
- **Training Time:** Minimal (mostly implementation)
- **Best For:** NLP/LLM experts
- **Quick Start:** Implement RAG system

👉 [Read NLP Guide](./CHALLENGES/nlp.md)

---

### 🏁 Finals
> **Integration • Multi-task • All ports**

Combine AE, CV, and NLP into a unified system. Advanced integration and optimization.

- **Difficulty:** 🔴 Advanced
- **Prerequisites:** Complete at least 1-2 challenges first
- **Best For:** System architects

👉 [Read Finals Guide](./CHALLENGES/finals.md)

---

## 📚 Documentation Map

### 🚀 **Getting Started**

| 📄 | Topic | Time | Level |
|---|-------|------|-------|
| [QUICK_START.md](./QUICK_START.md) | 5-minute setup | 2 min | 🟢 Easy |
| [SETUP.md](./SETUP.md) | Detailed environment | 10 min | 🟢 Easy |
| [VERIFY.md](./VERIFY.md) | Validation checklist | 10 min | 🟢 Easy |
| [CHALLENGES.md](./CHALLENGES.md) | All challenges | 10 min | 🟢 Easy |

### 💻 **Development**

| 📄 | Topic | Time | Level |
|---|-------|------|-------|
| [DEVELOPMENT.md](./DEVELOPMENT.md) | Workflow & best practices | 15 min | 🟡 Medium |
| [ARCHITECTURE.md](./ARCHITECTURE.md) | Repository structure | 10 min | 🟡 Medium |
| [GIT_GUIDE.md](./GIT_GUIDE.md) | Git & submodules | 10 min | 🟡 Medium |

### 🚢 **Deployment**

| 📄 | Topic | Time | Level |
|---|-------|------|-------|
| [SUBMISSION.md](./SUBMISSION.md) | How to submit | 10 min | 🟡 Medium |
| [DOCKER.md](./DOCKER.md) | Container deployment | 15 min | 🟡 Medium |
| [API.md](./API.md) | API specifications | 20 min | 🟡 Medium |

### ⚙️ **Advanced**

| 📄 | Topic | Time | Level |
|---|-------|------|-------|
| [PERFORMANCE.md](./PERFORMANCE.md) | Optimization | 15 min | 🔴 Hard |
| [TROUBLESHOOTING.md](./TROUBLESHOOTING.md) | Problem solving | Variable | 🔴 Hard |
| [RESOURCES.md](./RESOURCES.md) | External links | 5 min | 🟢 Easy |

---

## 🎓 Pick Your Path

### 🟢 I'm New Here
```
START → QUICK_START → SETUP → VERIFY → CHALLENGES → Development
```
**Time:** ~40 minutes to first code

### 🟡 I'm Experienced
```
START → CHALLENGES → Development → SUBMISSION
```
**Time:** ~20 minutes to first code

### 🔴 I'm an Expert
```
CHALLENGES → Code → SUBMISSION
```
**Time:** Immediate

---

## 🚀 Popular Starting Points

<table>
<tr>
<td width="25%" align="center">

### ⚡ Fast Track
[QUICK_START.md](./QUICK_START.md)

5 minutes to running code

</td>
<td width="25%" align="center">

### 🎯 Choose Challenge  
[CHALLENGES.md](./CHALLENGES.md)

Pick your focus area

</td>
<td width="25%" align="center">

### 💻 Start Developing
[DEVELOPMENT.md](./DEVELOPMENT.md)

Best practices & workflow

</td>
<td width="25%" align="center">

### 🆘 Get Help
[TROUBLESHOOTING.md](./TROUBLESHOOTING.md)

Solutions & answers

</td>
</tr>
</table>

---

## 📊 By The Numbers

<div align="center">

| Metric | Value |
|--------|-------|
| 📄 Documentation Files | **23** |
| 📦 Total Size | **261 KB** |
| 🕐 Reading Time (complete) | **~170 min** |
| ⚡ Quick Start Time | **5 min** |
| 💡 Code Examples | **40+** |
| 🆘 Troubleshooting Tips | **100+** |
| 🔗 External Resources | **50+** |

</div>

---

## 🎨 Documentation Features

### ✨ Well-Organized
- Clear categories and sections
- Consistent formatting
- Easy navigation
- Cross-linked references

### 📖 Comprehensive
- From setup to submission
- All challenges covered
- Advanced topics included
- Real-world examples

### 🎯 Practical
- Step-by-step guides
- Copy-paste commands
- Code examples
- Common mistakes

### 🆘 Supportive
- Troubleshooting guide
- FAQ sections
- Error solutions
- External resources

---

## 🔗 Directory Structure

```
readme/
├── README.md                    ← You are here
├── _INDEX.md                    ← File index
├── QUICK_START.md              ⚡ Start here (5 min)
├── SETUP.md                    📦 Environment setup
├── VERIFY.md                   ✓ Validation
├── CHALLENGES.md               🎯 Pick a challenge
│
├── CHALLENGES/                 📁 Challenge guides
│   ├── ae.md                   🤖 Agent Environment
│   ├── cv.md                   👁️ Computer Vision
│   ├── nlp.md                  📚 NLP
│   └── finals.md               🏁 Finals
│
├── DEVELOPMENT.md              💻 How to develop
├── ARCHITECTURE.md             🏗️ Repository structure
├── SUBMISSION.md               🚢 How to submit
├── DOCKER.md                   🐳 Docker deployment
├── API.md                      🔌 API specs
├── GIT_GUIDE.md                🔀 Git workflow
├── PERFORMANCE.md              ⚡ Optimization
├── TROUBLESHOOTING.md          🆘 Problem solving
└── RESOURCES.md                🔗 External links
```

---

## ⏱️ Time Investment

| Stage | Time | Files |
|-------|------|-------|
| **Setup** | 15 min | QUICK_START, SETUP, VERIFY |
| **Learning** | 20 min | CHALLENGES, CHALLENGES/* |
| **Development** | 20 min | DEVELOPMENT, ARCHITECTURE |
| **Submission** | 15 min | SUBMISSION, DOCKER |
| **Optimization** | 30 min | PERFORMANCE, TROUBLESHOOTING |
| **Total (optional)** | 100+ min | All |

**Minimum required:** 30-40 minutes to first working code ⚡

---

## 🎓 Learning Outcomes

After following this documentation, you'll be able to:

- ✅ Set up your TIL-26 environment correctly
- ✅ Understand all 4 challenges deeply
- ✅ Develop and test solutions locally
- ✅ Build Docker containers for submission
- ✅ Deploy and submit your work
- ✅ Optimize for performance
- ✅ Solve problems independently
- ✅ Integrate multiple challenges

---

## 🌟 Pro Tips

### 💡 Success Strategy
1. **Read first** — Documentation is short and saves time
2. **Start simple** — Get baseline working before optimization
3. **Test locally** — Always test before submitting
4. **Iterate fast** — Submit early, improve often
5. **Check Discord** — Community can help solve issues

### ⚡ Time Savers
- Use QUICK_START.md for fastest path
- Reference TROUBLESHOOTING.md for common issues
- Check PERFORMANCE.md for optimization ideas
- Search RESOURCES.md for external help

### 🎯 Pro Approach
```
Week 1: Get baseline working, submit
Week 2: Improve score, submit update
Week 3: Optimize, submit final version
Week 4: Final refinements (if time permits)
```

---

## 🔍 Search This Hub

**Looking for something specific?**

- **Setup issues?** → [TROUBLESHOOTING.md](./TROUBLESHOOTING.md)
- **API format?** → [API.md](./API.md)
- **Docker help?** → [DOCKER.md](./DOCKER.md)
- **Git questions?** → [GIT_GUIDE.md](./GIT_GUIDE.md)
- **Slow code?** → [PERFORMANCE.md](./PERFORMANCE.md)
- **External links?** → [RESOURCES.md](./RESOURCES.md)

---

## 🤝 Community

### Get Help
- 💬 **Discord:** #hackoverflow channel
- 📖 **Wiki:** https://github.com/til-ai/til-26/wiki
- 📊 **Leaderboard:** Strategist's Handbook (Notion)

### Learn More
- 📚 **Full specs:** Competition Wiki
- 🎓 **Curriculum:** Google Drive (shared)
- 🌐 **Resources:** See [RESOURCES.md](./RESOURCES.md)

---

## ✅ Checklist Before You Start

- [ ] Read this README
- [ ] Pick a quick start guide
- [ ] Set up environment
- [ ] Validate setup
- [ ] Choose a challenge
- [ ] Read challenge guide
- [ ] Start coding!

---

<div align="center">

### 🚀 Ready to Get Started?

**[Go to QUICK_START.md →](./QUICK_START.md)**

*or choose your path above*

---

**Made with ❤️ for TIL-26 participants**

Generated: 2026-05-19 | Last Updated: 2026-05-19

</div>
