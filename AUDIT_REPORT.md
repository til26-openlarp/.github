# TIL-26 Repository Audit Report
**Date:** 2026-05-18

## Executive Summary
This repository contains 4 main challenge directories (ae, cv, nlp, til-26-openlarp), each with git submodules for `submit` and/or `train` components. Most documentation is in place, but there are gaps in setup instructions for some submodules.

## Directory Structure

```
TIL-26/
├── ae/               ✓ Git repo with 2 submodules (submit, til-26-ae)
├── cv/               ✓ Git repo with 2 submodules (submit, train)
├── nlp/              ✓ Git repo with 2 submodules (submit, train)
└── til-26-openlarp/  ✓ Git repo with 2 submodules (til-26-ae, til-26-finals)
```

## Documentation Status by Directory

### AE (Agent Environment - Bomberman)
- ✓ README.md - Clear quickstart and interface docs
- ✓ AGENTS.md - Comprehensive agent dev guide
- ✓ submit/README.md - Submission interface documented
- ✓ submit/requirements.txt - Dependencies clear
- ✓ til-26-ae/README.md - Environment package well documented
- ✓ til-26-ae/requirements.txt - Package deps clear
- **Status: COMPLETE**

### CV (Computer Vision)
- ✓ README.md - Overview and interface docs
- ✓ AGENTS.md - Agent instructions
- ✓ YAML config files in config/ directory
- ✓ submit/requirements.txt - Dependencies listed
- ✓ template/ - Starter template provided
- ✗ submit/README.md - **MISSING** - No submission setup guide
- ✗ train/README.md - **MISSING** - No training documentation
- **Status: NEEDS ATTENTION**

### NLP (Natural Language Processing)
- ✓ README.md - Interface format documented
- ✓ AGENTS.md - Agent development guide
- ✓ submit/README.md - Submission interface documented
- ✓ submit/requirements.txt - Dependencies clear
- ✓ train/README.md - Training guide present
- ✓ train/requirements.txt - Package deps clear
- **Status: COMPLETE**

### TIL-26-OpenLarp (Finals/Integration)
- ✓ README.md - Project overview and setup
- ✓ AGENTS.md - Agent guide
- ✓ requirements-dev.txt - Development dependencies
- ✓ Multiple subdirectories (ae, cv, nlp, asr, noise) with READMEs
- **Status: COMPLETE**

## Git Submodule Status

### AE
| Submodule | Commit | Status |
|-----------|--------|--------|
| submit | 0d3e936 | Clean |
| til-26-ae | 39ae862 | Clean |

### CV
| Submodule | Commit | Status |
|-----------|--------|--------|
| submit | 49409a0 | Clean |
| train | 9633a06 | ⚠️ MODIFIED (has uncommitted changes) |

### NLP
| Submodule | Commit | Status |
|-----------|--------|--------|
| submit | 5d91695 | Clean |
| train | n/a (checked as part of repo) | Clean |

## Critical Issues Found

### 1. CV/Train Submodule Status
**Issue:** cv/train shows "+" indicator, indicating uncommitted changes
**Impact:** Submodule may not be in a consistent state
**Action Required:** Investigate and resolve uncommitted changes

### 2. Missing README Files
**Location:** cv/submit/ and cv/train/
**Impact:** Users cannot quickly understand how to set up or use these components
**Files Needed:**
- cv/submit/README.md - Document the submission server setup and FastAPI interface
- cv/train/README.md - Document training workflow, dataset setup, model selection

### 3. Missing Top-Level Documentation
**Issue:** No CLAUDE.md or comprehensive setup guide at repository root
**Impact:** New users cannot quickly understand project organization
**Files Needed:** Top-level documentation explaining:
- Project overview (4 challenges)
- Quick start for each challenge
- Common setup steps (venv, dependencies, submodules)
- Directory map with cross-references

## Recommendations

### Priority 1: Fix cv/train Submodule
1. Check what files are modified in cv/train
2. Either commit changes or revert to clean state
3. Update parent repo to reference correct commit

### Priority 2: Create Missing READMEs
1. **cv/submit/README.md** - Document:
   - FastAPI server setup (port 5002)
   - Dockerfile build and test
   - How to mount models
   - Testing the /cv endpoint locally

2. **cv/train/README.md** - Document:
   - Dataset organization
   - Model options (YOLOv8, RT-DETR, RF-DETR)
   - Training workflow with scripts
   - Export and conversion process
   - Evaluation metrics

### Priority 3: Create Top-Level Documentation
1. Create CLAUDE.md with:
   - Project architecture overview
   - Environment setup (Python 3.10+, venv, submodules)
   - Per-challenge quick start
   - Common commands (build, test, submit)
   - Link to Wiki for detailed specs

### Priority 4: Documentation Consistency
1. Ensure all challenge `submit` directories have similar README structure
2. Ensure all challenge `train` directories have similar README structure
3. Add troubleshooting sections to each README

## Quick Validation Checklist

- [ ] Run `git submodule status` and verify clean state
- [ ] Verify all `README.md` files exist in submission directories
- [ ] Verify all dependencies in `requirements.txt` are correct
- [ ] Test at least one challenge's quickstart (ae quickstart is best)
- [ ] Verify setup-env.sh scripts are executable and functional
- [ ] Ensure all Dockerfiles reference correct ports and services

