# AE Challenge — Agent Environment (Bomberman)

Multi-agent Reinforcement Learning challenge. Train agents to play Bomberman.

## Overview

- **Type:** Multi-agent RL
- **Environment:** Bomberman (6 teams, 2D maze)
- **Port:** 5003
- **Difficulty:** Beginner-friendly
- **Training Time:** Minutes to hours

## What is Bomberman?

6 teams compete simultaneously. Each team controls:
- 1 mobile **Agent** (moves, places bombs)
- 1 stationary **Base** (can be destroyed)

**Goals:**
- Collect power-ups (missions, recon resources)
- Destroy enemy bases
- Protect your own base
- Survive the longest

## Challenge Interface

### Submission Request
```json
{
  "instances": [
    {"observation": [...], "info": {}}
  ]
}
```

### Submission Response
```json
{
  "predictions": [0]  // Action index (0-5)
}
```

**Actions:**
- 0: FORWARD
- 1: BACKWARD
- 2: LEFT
- 3: RIGHT
- 4: STAY
- 5: PLACE_BOMB

## Quick Start

```bash
cd ae
pip install -e til-26-ae
python benchmarks/bench_diag.py     # See one game
python visualisers/visualise_fast.py # Interactive visualizer
```

## Implementation

### 1. Understand the Environment
- Read `til-26-ae/README.md` for detailed docs
- Run visualizer to understand game mechanics

### 2. Implement Your Manager
```python
# ae/submit/src/ae_manager.py
class AEManager:
    def __init__(self):
        # Load model, initialize agent
        pass
    
    def predict(self, instance):
        # observation -> action
        return action_index
```

### 3. Choose Your Approach
- **Imitation Learning:** Mimic benchmark agents
- **Reinforcement Learning:** Train from scratch (PPO, A2C, etc.)
- **Heuristics:** Hand-coded strategy
- **Hybrid:** Combine methods

### 4. Train & Benchmark
```bash
python benchmarks/bench_diag.py          # Quick test (1 game)
python benchmarks/bench_multi.py         # Serious benchmark (multi-seed)
python visualisers/visualise_fast.py    # Debug with viz
```

### 5. Deploy & Submit
```bash
cd submit/src
uvicorn ae_server:app --port 5003
# Test with curl/requests
```

## Key Files

- `ae/README.md` — Overview
- `ae/AGENTS.md` — Detailed reference
- `ae/til-26-ae/README.md` — Environment package docs
- `ae/submit/src/ae_manager.py` — Your implementation
- `ae/benchmarks/bench_*.py` — Testing tools
- `ae/visualisers/visualise_*.py` — Debugging tools

## Observation Space

Each agent observes:
- **agent_viewcone** — Multi-channel visual observation (agent-centered)
- **base_viewcone** — Base-centered observation
- **direction** — Agent's facing direction (0-3)
- **location** — Agent position (x, y)
- **base_location** — Team's base position
- **health** — Agent HP
- **frozen_ticks** — Ticks until respawn (0 = alive)
- **base_health** — Base HP
- **team_resources** — Bomb budget
- **team_bombs** — Bombs available
- **step** — Current step
- **action_mask** — Legal actions

## Reward Structure

- Collect mission (+5)
- Kill enemy agent (+10)
- Destroy enemy base (+50)
- Invalid action (-penalty)

## Competition Strategy

### Beginner
1. Get baseline working (random actions)
2. Add simple heuristics (move toward power-ups)
3. Test and iterate

### Intermediate
1. Implement imitation learning (copy benchmark agent)
2. Train RL agent with PPO
3. Tune hyperparameters

### Advanced
1. Multi-agent curriculum learning
2. Self-play
3. Advanced exploration strategies

## Common Issues

| Issue | Solution |
|-------|----------|
| Slow benchmark | Reduce seed count in `bench_multi.py` |
| Model too large | Quantize or distill model |
| Overfitting | Add augmentation, increase exploration |
| Obs too complex | Flatten with `FlattenDictWrapper` (default) |

## Tips for Success

1. **Visualize first** — Use visualizer to understand game
2. **Start simple** — Get baseline working before optimization
3. **Use benchmarks** — Compare against provided agents
4. **Iterate fast** — Quick train/test/improve loops
5. **Read AGENTS.md** — Detailed reference is excellent

## Resources

- **Main docs:** `../ae/README.md`
- **Developer guide:** `../ae/AGENTS.md`
- **Environment:** `../ae/til-26-ae/README.md`
- **Code examples:** `../ae/src/ae_manager_*.py`
- **Tools:** `../ae/benchmarks/` and `../ae/visualisers/`

## Next Steps

1. Read `../ae/README.md`
2. Read `../ae/AGENTS.md`
3. Run quickstart (`python benchmarks/bench_diag.py`)
4. Try visualizer
5. Edit `ae_manager.py`
6. Iterate!

---

**Estimated time to first submission:** 2-3 hours
