# TIL-26 Team Competition Journey

<style>
@keyframes slideIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}
@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.7; }
}
.header { animation: slideIn 0.6s ease-out; }
.phase { animation: slideIn 0.6s ease-out; }
.milestone { animation: pulse 2s infinite; }
</style>

<div class="header">

## Your Path to Victory

This documentation guides your team through the DSTA BrainHack TIL-AI 2026 competition. From kickoff to final submission, everything you need to succeed is here.

**Status:** Ready for Competition | **Files:** 23 | **Size:** 261 KB

</div>

---

## The Stages of Your Journey

### Stage 1: Team Mobilization (Week 1)

Before you can compete, your team needs to be ready. This is where you align on strategy and get your environment set up.

**Key Milestones:**
- Team understands the competition structure
- Environment is properly configured
- All tools are installed and verified
- Team has selected its focus area(s)

**Documentation:**
- [QUICK_START.md](../QUICK_START.md) — Get your team up and running in 5 minutes
- [SETUP.md](../SETUP.md) — Ensure everyone has the same environment
- [VERIFY.md](../VERIFY.md) — Confirm everything works before you begin

**Time Investment:** 30-45 minutes per team member

---

### Stage 2: Challenge Selection & Deep Dive (Week 1-2)

Your team needs to understand what you're building and why. This is the research and planning phase.

**Key Milestones:**
- Team selects 1-2 challenges to focus on
- Deep understanding of challenge requirements
- Clear success metrics identified
- Development strategy documented

**Documentation:**
- [CHALLENGES.md](../CHALLENGES.md) — Overview of all four challenges
- [CHALLENGES/ae.md](../CHALLENGES/ae.md) — Agent Environment details
- [CHALLENGES/cv.md](../CHALLENGES/cv.md) — Computer Vision details
- [CHALLENGES/nlp.md](../CHALLENGES/nlp.md) — NLP details
- [CHALLENGES/finals.md](../CHALLENGES/finals.md) — Finals integration strategy

**Time Investment:** 45-60 minutes for team alignment

---

### Stage 3: Development & Iteration (Week 2-3)

This is where the real work happens. Your team builds, tests, and improves solutions.

**Key Milestones:**
- Baseline solution working locally
- First submission completed
- Performance metrics established
- Iteration cycle in place

**Documentation:**
- [DEVELOPMENT.md](../DEVELOPMENT.md) — Development workflow and best practices
- [ARCHITECTURE.md](../ARCHITECTURE.md) — Understanding the repository structure
- [API.md](../API.md) — API contracts and specifications
- [GIT_GUIDE.md](../GIT_GUIDE.md) — Coordinating work with Git

**Time Investment:** Variable (most of competition time)

---

### Stage 4: Optimization & Polish (Week 3-4)

Time to squeeze every last point out of your solutions. This is the optimization phase.

**Key Milestones:**
- Performance improvements implemented
- Multiple submission iterations completed
- Team comparing against leaderboard
- Final tuning in progress

**Documentation:**
- [PERFORMANCE.md](../PERFORMANCE.md) — Optimization techniques and strategies
- [SUBMISSION.md](../SUBMISSION.md) — How to submit your work
- [DOCKER.md](../DOCKER.md) — Containerizing your solutions

**Time Investment:** 30-40 minutes for strategy, then ongoing

---

### Stage 5: Final Push & Submission (Week 4)

The final stretch. Your team makes the last optimizations and gets everything submitted before the deadline.

**Key Milestones:**
- Final submissions in place
- All solutions tested and validated
- Team tracking leaderboard positions
- Lesson documentation for next time

**Documentation:**
- [SUBMISSION.md](../SUBMISSION.md) — Final submission process
- [TROUBLESHOOTING.md](../TROUBLESHOOTING.md) — Quick problem resolution
- [RESOURCES.md](../RESOURCES.md) — External references for last-minute help

**Time Investment:** Varies

---

## Team Workflow

### Typical Week Structure

```
Monday     → Challenge analysis & planning
Tuesday    → Development & experimentation  
Wednesday  → Testing & local validation
Thursday   → Integration & submission prep
Friday     → Optimization & performance tuning
Weekend    → Reflection & iteration planning
```

### Communication & Coordination

**Use Git effectively:**
- Feature branches for each improvement (`feature/ae-agent`, `feature/cv-model`)
- Clear commit messages explaining changes
- Regular status checks with `git log --oneline`
- See [GIT_GUIDE.md](../GIT_GUIDE.md) for workflow

**Track progress:**
- Update team on submission status
- Share performance metrics regularly
- Celebrate improvements and milestones
- Document lessons learned

---

## Challenge Selection Strategy

Choose based on your team's strengths:

### Agent Environment (AE)
**Best for:** Teams with RL expertise, learning-focused approach  
**Difficulty:** Beginner-friendly  
**Time to first submission:** 4-6 hours  
**Skills needed:** Python, basic deep learning  

### Computer Vision (CV)
**Best for:** ML practitioners, production experience  
**Difficulty:** Intermediate  
**Time to first submission:** 6-8 hours  
**Skills needed:** Python, PyTorch/TensorFlow, computer vision  

### Natural Language Processing (NLP)
**Best for:** LLM experts, cutting-edge interests  
**Difficulty:** Advanced  
**Time to first submission:** 6-8 hours  
**Skills needed:** Python, transformers, retrieval systems  

### Finals Integration
**Best for:** Ambitious teams, full-stack thinking  
**Difficulty:** Very Advanced  
**Prerequisites:** Completion of at least 1-2 other challenges first  
**Skills needed:** System design, API integration, DevOps  

---

## Essential Documentation by Phase

### Getting Started (First 30 Minutes)

Your team's first hour should include:

1. Read: [QUICK_START.md](../QUICK_START.md)
2. Follow: [SETUP.md](../SETUP.md)
3. Validate: [VERIFY.md](../VERIFY.md)
4. Discuss: [CHALLENGES.md](../CHALLENGES.md)

### Development (Primary Phase)

Your team's main resources:

- [DEVELOPMENT.md](../DEVELOPMENT.md) — Day-to-day workflow
- [ARCHITECTURE.md](../ARCHITECTURE.md) — Understanding the codebase
- Challenge-specific guides in CHALLENGES/
- [API.md](../API.md) — API contracts

### Optimization (Ongoing)

To improve your score:

- [PERFORMANCE.md](../PERFORMANCE.md) — Optimization strategies
- Challenge-specific optimization sections
- [RESOURCES.md](../RESOURCES.md) — External techniques and papers

### Submission & Support

When you're ready to submit or need help:

- [SUBMISSION.md](../SUBMISSION.md) — Submission checklist
- [DOCKER.md](../DOCKER.md) — Container deployment
- [TROUBLESHOOTING.md](../TROUBLESHOOTING.md) — Problem solving

---

## Team Success Metrics

Track your team's progress with these indicators:

### Early Stage (Week 1-2)
- Environment properly configured (100% of team)
- At least one submission completed
- Baseline score established
- Team velocity in commits/PRs

### Mid Stage (Week 2-3)
- Multiple iterations submitted
- Performance improving each week
- Clear optimization strategy in place
- Team communication smooth

### Late Stage (Week 3-4)
- Final optimizations yielding gains
- Leaderboard position improving
- Team focused and executing
- Preparation for final push complete

---

## Coordination Checklist

Use this to keep your team aligned:

- [ ] All team members have environment set up
- [ ] Team has selected challenge(s)
- [ ] Development roles assigned
- [ ] Submission schedule planned
- [ ] Git workflow established
- [ ] Communication channels clear
- [ ] Regular sync meetings scheduled
- [ ] Progress tracking system in place
- [ ] Optimization strategy defined
- [ ] Final submission plan documented

---

## The Files Your Team Will Use Most

| File | When | Why |
|------|------|-----|
| [QUICK_START.md](../QUICK_START.md) | Week 1 | Initial setup |
| [CHALLENGES.md](../CHALLENGES.md) | Week 1 | Choose focus |
| [DEVELOPMENT.md](../DEVELOPMENT.md) | Week 2-4 | Daily reference |
| [API.md](../API.md) | Week 2-4 | Implementation details |
| [PERFORMANCE.md](../PERFORMANCE.md) | Week 3-4 | Score improvement |
| [SUBMISSION.md](../SUBMISSION.md) | Week 4 | Final checklist |
| [TROUBLESHOOTING.md](../TROUBLESHOOTING.md) | Ongoing | Problem resolution |

---

## Competition Timeline

### Week 1: Kickoff
- [ ] Team assembled
- [ ] Environments configured
- [ ] Challenge selected
- [ ] Strategy discussed

### Week 2: Development
- [ ] Initial solution working
- [ ] First submission made
- [ ] Performance baseline established
- [ ] Iteration cycle begun

### Week 3: Optimization
- [ ] Multiple iterations submitted
- [ ] Performance improving
- [ ] Team in rhythm
- [ ] Leaderboard tracking

### Week 4: Final Push
- [ ] Final optimizations in place
- [ ] Last submissions queued
- [ ] Team focused
- [ ] Prepared for deadline

---

## Getting Help

When your team gets stuck:

**Technical Issues**
- First check: [TROUBLESHOOTING.md](../TROUBLESHOOTING.md)
- Then check: [GIT_GUIDE.md](../GIT_GUIDE.md) for version control issues
- Finally: [DOCKER.md](../DOCKER.md) for deployment issues

**Challenge Questions**
- Review: Challenge-specific guide (CHALLENGES/*)
- Check: Main challenge README in root directory
- See: [API.md](../API.md) for interface specifications

**External Help**
- Discord: #hackoverflow channel
- Wiki: https://github.com/til-ai/til-26/wiki
- Leaderboard: Strategist's Handbook (Notion)

**When You're Blocked**
- Document what you've tried
- Share error messages with team
- Check Discord for others facing same issue
- Reference [TROUBLESHOOTING.md](../TROUBLESHOOTING.md)

---

## Next Steps for Your Team

1. **Today:** Read this document, run QUICK_START.md
2. **This Week:** Complete SETUP.md, choose challenge, review strategy
3. **Ongoing:** Use DEVELOPMENT.md as daily reference
4. **Week 3:** Start PERFORMANCE.md optimizations
5. **Week 4:** Follow SUBMISSION.md checklist

---

## Quick Navigation

- **Getting Started:** [QUICK_START.md](../QUICK_START.md)
- **Setup:** [SETUP.md](../SETUP.md)
- **Challenge Info:** [CHALLENGES.md](../CHALLENGES.md)
- **Development:** [DEVELOPMENT.md](../DEVELOPMENT.md)
- **Need Help:** [TROUBLESHOOTING.md](../TROUBLESHOOTING.md)
- **All Files:** [_INDEX.md](./_INDEX.md)

---

**Your team has everything needed to succeed. Focus on execution.**

Competition dates | Team coordination | Clear strategy = Victory

*Generated: 2026-05-19*
