# Git & Submodule Guide

Working with git and git submodules in TIL-26.

## Quick Git Reference

### Clone Repository

```bash
git clone <your-repo-url>
cd TIL-26
git submodule update --init --recursive
```

### Check Status

```bash
# Your branch status
git status

# All submodules
git submodule status
```

### Commit Changes

```bash
# Stage files
git add <file>

# Commit
git commit -m "Meaningful message"

# Push to remote
git push origin <branch-name>
```

---

## Git Submodules Explained

TIL-26 uses **git submodules** to manage separate repositories:

```
TIL-26 (parent repo)
├── ae/submit         (submodule) → separate git repo
├── ae/til-26-ae      (submodule) → separate git repo
├── cv/submit         (submodule) → separate git repo
├── cv/train          (submodule) → separate git repo
└── ...
```

### What This Means

- Each submodule is a **separate git repository**
- Submodules have **independent git history**
- Parent repo tracks **which commit** each submodule points to
- Submodule updates are **separate commits** in parent repo

### Typical Submodule Workflow

```bash
# Update submodule to latest
cd ae/submit
git fetch origin
git checkout main  # or master
cd ../..

# Or from parent repo
git submodule update --remote ae/submit

# Commit the change
git add ae/submit
git commit -m "Update ae/submit to latest"
git push
```

---

## Common Submodule Tasks

### Initialize Submodules

First time only:
```bash
git submodule update --init --recursive
```

### Update All Submodules

```bash
git submodule update --remote --recursive
```

### Check Submodule Status

```bash
git submodule status
# Shows commit hash for each submodule
```

### Work Inside a Submodule

```bash
cd ae/submit

# See submodule's git status
git status

# Make changes, commit, push
git add <file>
git commit -m "Fix in ae/submit"
git push origin main

# Return to parent
cd ../..

# Update parent to point to new commit
git add ae/submit
git commit -m "Update ae/submit to new commit"
git push
```

### Pull Latest Submodule Changes

```bash
git pull
git submodule update --init --recursive
```

---

## SSH vs HTTPS

### SSH (Recommended)

Configure once:
```bash
git config --global url."git@github.com:".insteadOf "https://github.com/"
```

Then all git commands use SSH.

### HTTPS

Use with Personal Access Token:
```bash
git clone https://<token>@github.com/<user>/<repo>.git
```

---

## Troubleshooting Submodules

### Issue: `git submodule update` hangs or fails

**Solution:**
```bash
# Check SSH is configured
ssh -T git@github.com
# Should show: "Hi <username>!"

# If not, set up SSH key or use HTTPS
git config --global url."git@github.com:".insteadOf "https://github.com/"
```

### Issue: Submodule shows as "detached HEAD"

```bash
cd <submodule>
git checkout main  # or master
cd ..
git add <submodule>
git commit -m "Fix detached HEAD in submodule"
```

### Issue: Submodule changes not committed

```bash
# Changes in submodule must be committed twice:
cd <submodule>
git add <file>
git commit -m "Change in submodule"
git push
cd ..

# Then commit in parent
git add <submodule>
git commit -m "Update submodule pointer"
git push
```

### Issue: "Permission denied" when cloning

**Solution:**
```bash
# Set up SSH key
ssh-keygen -t ed25519 -C "your_email"
# Add public key to GitHub

# Or use HTTPS with token
git config --global credential.helper store
```

---

## Development Workflow

### Single Challenge Development

```bash
# Create feature branch
git checkout -b feature/improve-model

# Make changes (in submit/ or train/)
cd <challenge>/submit/src
# Edit <challenge>_manager.py
git add .
git commit -m "Improve model accuracy"

# Test locally before pushing
python test_locally.py

# Push
git push origin feature/improve-model

# Create pull request (if team project)
# Or just push to main
git checkout main
git merge feature/improve-model
git push
```

### Multi-Challenge Development

```bash
# Separate branches for each challenge
git checkout -b feature/ae-agent
# Work on AE...
git commit -m "Implement new AE strategy"
git push

# Switch to CV work
git checkout -b feature/cv-model
# Work on CV...
git commit -m "Train CV model"
git push

# Merge when ready
git checkout main
git merge feature/ae-agent
git merge feature/cv-model
git push
```

---

## Commit Message Best Practices

### Good Commits

```
Short title (50 chars)

Optional longer explanation (80 chars per line)
explaining the WHY and HOW of the change.

- Bullet points for multiple changes
- Reference issues/PRs (#123)
```

### Examples

```
Implement RAG for NLP challenge

- Use sentence-transformers for embeddings
- FAISS vector store for retrieval
- LLaMA 2 for generation

Fixes #45, related to #32
```

```
Optimize CV inference speed

Batch processing reduces latency by 50%.
Uses FP16 quantization for smaller model.

Tested on 1000 images, no accuracy loss.
```

---

## Branching Strategy

### Recommended Setup

```
main (default)
├── feature/ae-* (AE features)
├── feature/cv-* (CV features)
├── feature/nlp-* (NLP features)
├── bugfix/ae-* (AE fixes)
└── experiment/* (experiments, deleted after)
```

### Creating Features

```bash
# Create from main
git checkout main
git pull
git checkout -b feature/my-improvement

# Make changes...
git commit -m "Description"

# Push
git push origin feature/my-improvement

# Merge to main when ready
git checkout main
git merge feature/my-improvement
git push
```

---

## Git History Inspection

### View Commit History

```bash
# Recent commits
git log --oneline -10

# With details
git log -p -n 5

# Graph view
git log --graph --oneline --all --decorate
```

### See What Changed

```bash
# Diff uncommitted changes
git diff

# Diff staged changes
git diff --cached

# Diff between branches
git diff main feature/my-feature

# See file history
git log -p <file>

# Who changed each line
git blame <file>
```

### Revert Changes

```bash
# Undo uncommitted changes
git checkout <file>

# Undo staged changes
git reset <file>

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Revert a published commit
git revert <commit-hash>
```

---

## Remote Branches

### View Remote Branches

```bash
git branch -r
# Shows all remote branches
```

### Track Remote Branch

```bash
# Automatically track
git checkout <branch-name>
# If <branch-name> doesn't exist locally, git creates it tracking remote

# Manually
git branch -u origin/<branch-name>
```

### Delete Remote Branch

```bash
git push origin :<branch-name>
# Or
git push origin --delete <branch-name>
```

---

## Tags & Releases

### Create Tag

```bash
# Lightweight tag
git tag v1.0.0

# Annotated tag with message
git tag -a v1.0.0 -m "First release"

# Push tag
git push origin v1.0.0
```

### List Tags

```bash
git tag
git tag -l "v1.*"
```

---

## Tips & Tricks

### Useful Aliases

```bash
# Add to ~/.gitconfig
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    unstage = reset HEAD --
    last = log -1 HEAD
    visual = log --graph --oneline --all
```

### Fast Commit

```bash
git add .
git commit -m "message"
git push
```

### Stashing Changes

```bash
# Save work in progress
git stash

# Work on something else
git checkout main

# Return to saved work
git stash pop
```

### Check Before Push

```bash
# See what will be pushed
git log origin/main..HEAD

# Then push only when ready
git push
```

---

## Common Mistakes & Recovery

| Mistake | Recovery |
|---------|----------|
| Committed to wrong branch | Create new branch from commit, reset original |
| Pushed wrong commit | `git revert <commit>` to undo publicly |
| Deleted file accidentally | `git checkout <file>` to restore |
| Messed up merge | `git merge --abort` or `git reset --hard HEAD` |

---

**For more help:**
```bash
git help <command>  # Built-in help
man git            # Manual pages
```

---

**Remember:** Commit early, commit often, and commit with meaningful messages!
