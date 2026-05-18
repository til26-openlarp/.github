# Finals Challenge — Integration & Multi-Task Learning

Combine AE, CV, and NLP components into a unified system.

## Overview

- **Type:** System Integration & Multi-task learning
- **Goal:** Integrate agents from all challenges
- **Difficulty:** Advanced (requires mastery of all challenges)
- **Prerequisites:** Complete at least AE + CV or AE + NLP

## What is Finals?

**Standard track:** Build the best agent/model for your chosen challenge

**Finals track:** Integrate multiple challenges into a cohesive system

**Example scenario:**
- CV detects objects in an aerial image
- AE agent uses CV detections to navigate
- NLP system generates reports from agent observations

## Architecture Options

### Option 1: Sequential Pipeline
```
Input → CV (detect) → AE (navigate) → NLP (summarize) → Output
```

- **Pros:** Simple, easy to debug
- **Cons:** Slow, errors cascade

### Option 2: Concurrent System
```
          ├→ CV (detect)
Input ----├→ AE (navigate) → Output
          └→ NLP (process)
```

- **Pros:** Fast, parallel processing
- **Cons:** More complex coordination

### Option 3: Closed Loop
```
CV (detect) ↔ AE (navigate)
     ↓
  NLP (report)
```

- **Pros:** Most realistic, agent feedback improves decisions
- **Cons:** Most complex, requires careful state management

## Implementation Steps

### 1. Set Up Integration Environment
```bash
cd til-26-openlarp
pip install -r requirements-dev.txt
```

### 2. Run Individual Services
```bash
# Terminal 1: AE
cd ../ae/submit/src
uvicorn ae_server:app --port 5003

# Terminal 2: CV
cd ../cv/submit/src
uvicorn cv_server:app --port 5002

# Terminal 3: NLP
cd ../nlp/submit/src
uvicorn nlp_server:app --port 5004
```

### 3. Create Orchestrator
```python
# orchestrator.py
import requests

class Orchestrator:
    def __init__(self):
        self.ae = Client("http://localhost:5003")
        self.cv = Client("http://localhost:5002")
        self.nlp = Client("http://localhost:5004")
    
    def process(self, image):
        # Step 1: Detect objects
        detections = self.cv.detect(image)
        
        # Step 2: Navigate based on detections
        action = self.ae.act(detections)
        
        # Step 3: Generate summary
        summary = self.nlp.summarize(detections, action)
        
        return {"detections": detections, "action": action, "summary": summary}
```

### 4. Handle Inter-Service Communication
- **Protocol:** HTTP/REST or gRPC
- **Serialization:** JSON or Protocol Buffers
- **Retry logic:** Handle service failures gracefully
- **Caching:** Cache redundant calls
- **Rate limiting:** Respect service limits

### 5. Optimize System Performance
- **Parallel requests:** Use asyncio for concurrent calls
- **Model caching:** Keep models in memory
- **Batch processing:** Group similar requests
- **Pipeline optimization:** Reorder steps for speed

### 6. Test Integration
```python
# Test each pair
test_cv_ae()    # CV → AE
test_ae_nlp()   # AE → NLP
test_cv_nlp()   # CV → NLP
test_all()      # Full pipeline
```

## Performance Metrics

### Individual Challenge Scores
- AE score: Wins/resources collected
- CV score: mAP (object detection accuracy)
- NLP score: BLEU/ROUGE (answer quality)

### Integration Score
- **Latency:** Time for full pipeline
- **Accuracy:** End-to-end correctness
- **Robustness:** Handles failures gracefully
- **Efficiency:** Resource usage

## Integration Patterns

### Pattern 1: Agent-Centric
```
Agent observes → Gets vision input from CV → Uses NLP for context
```

### Pattern 2: Vision-Centric
```
Vision processes image → Agent acts on objects → NLP creates report
```

### Pattern 3: Knowledge-Centric
```
NLP loads knowledge → Agent uses knowledge → CV validates
```

## Advanced Techniques

### Multi-Task Learning
- Shared representations between components
- Transfer learning from individual models
- Joint optimization

### Active Learning
- Agent queries CV for uncertain detections
- Agent requests clarification from NLP
- Adaptive component selection

### Error Recovery
- Detect failed components
- Use fallback strategies
- Re-route to alternative path

## Challenges & Solutions

| Challenge | Solution |
|-----------|----------|
| Service failures | Implement retry + fallback |
| Slow pipelines | Parallelize, optimize, cache |
| Data misalignment | Normalize formats, add conversion |
| Performance bottleneck | Profile, optimize slowest step |
| Complex state | Use state machine pattern |

## Key Files

- `../til-26-openlarp/README.md` — Overview
- `../til-26-openlarp/AGENTS.md` — Integration guide
- `../ae/submit/src/ae_server.py` — AE service
- `../cv/submit/src/cv_server.py` — CV service
- `../nlp/submit/src/nlp_server.py` — NLP service

## Development Workflow

1. Get all three services running
2. Write orchestrator that calls them in sequence
3. Test each integration (CV→AE, AE→NLP, etc.)
4. Optimize for speed and accuracy
5. Add error handling and fallbacks
6. Deploy integrated system

## Testing Checklist

- [ ] All services run independently
- [ ] AE-CV integration works
- [ ] CV-NLP integration works
- [ ] AE-NLP integration works
- [ ] Full pipeline (AE→CV→NLP) works
- [ ] Handles single service failure
- [ ] Performance meets requirements
- [ ] State management is correct

## Submission

Finals are typically submitted as:
1. Individual challenge scores (AE, CV, NLP)
2. Integration score (system-level metric)
3. Leaderboard ranking (overall)

Check competition specs for exact format.

## Resources

- **Main integration guide:** `../til-26-openlarp/AGENTS.md`
- **AE service:** `../ae/submit/README.md`
- **CV service:** `../cv/submit/README.md`
- **NLP service:** `../nlp/submit/README.md`
- **Architecture:** `../ARCHITECTURE.md` (in readme/)

## Success Tips

1. **Master individual challenges first** — Get high scores in each
2. **Test components separately** — Verify each works alone
3. **Start simple** — Sequential pipeline before complex patterns
4. **Instrument code** — Log everything for debugging
5. **Profile performance** — Find bottlenecks early
6. **Test failures** — What if a service crashes?

## Timeline

**Recommended:**
- Week 1-2: Complete individual challenges
- Week 3: Build integration pipeline
- Week 4: Optimize and submit

**Advanced:**
- Start Finals integration early
- Iterate on both individual and system improvements
- Benchmark frequently

## Common Pitfalls

- ❌ Starting Finals too early (finish individual challenges first!)
- ❌ Not testing individual components
- ❌ Over-complicating architecture
- ❌ Not handling failures
- ❌ Ignoring performance

## Next Steps

1. **Complete** AE, CV, and/or NLP challenges first
2. **Read** `../til-26-openlarp/README.md`
3. **Read** `../til-26-openlarp/AGENTS.md`
4. **Get all services running** locally
5. **Write** orchestrator
6. **Iterate and optimize**

---

**Estimated time:** 4-6 hours (after completing individual challenges)
