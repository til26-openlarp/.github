# Docker Guide

Build and deploy TIL-26 services using Docker.

## Docker Basics

**What is Docker?** Container technology that packages your app with all dependencies.

**Why use Docker?** Ensures reproducibility — same image runs the same everywhere.

## Quick Start

```bash
# Build image
cd <challenge>/submit
docker build -t til-<challenge> .

# Run container
docker run --rm -p <PORT>:<PORT> --gpus all til-<challenge>

# Test
curl http://localhost:<PORT>/health
```

## Building Docker Images

### Build Command

```bash
cd <challenge>/submit
docker build -t til-<challenge>:latest .
# -t = tag (name:version)
# . = Dockerfile in current directory
```

### Build with Arguments

```bash
docker build \
  --build-arg PYTHON_VERSION=3.11 \
  -t til-<challenge>:v1.0 \
  .
```

### Multi-Stage Build (for CV)

```dockerfile
# Stage 1: Builder
FROM pytorch/pytorch:latest as builder
WORKDIR /build
RUN pip install torch torchvision ultralytics

# Stage 2: Runtime (smaller)
FROM python:3.11-slim
COPY --from=builder /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
```

## Running Containers

### Basic Run

```bash
docker run --rm til-<challenge>
# --rm = remove container after exit
```

### With GPU Support

```bash
docker run --rm --gpus all til-<challenge>
# --gpus all = use all available GPUs
```

### With Port Mapping

```bash
docker run --rm -p 5003:5003 til-ae
# -p host_port:container_port
# Access at http://localhost:5003
```

### With Environment Variables

```bash
docker run --rm \
  -e MODEL_PATH=/models/best.pt \
  -e BATCH_SIZE=16 \
  til-<challenge>
```

### With Volume Mounting

```bash
docker run --rm \
  -v /path/to/models:/app/models \
  -v /path/to/logs:/app/logs \
  til-<challenge>
```

### Interactive Mode

```bash
docker run -it til-<challenge> /bin/bash
# -i = interactive
# -t = terminal
```

## Dockerfile Structure

### Typical Dockerfile

```dockerfile
# Base image
FROM pytorch/pytorch:latest

# Set working directory
WORKDIR /app

# Copy requirements
COPY requirements.txt .

# Install dependencies
RUN pip install -r requirements.txt

# Copy application code
COPY . .

# Expose port
EXPOSE 5002

# Health check
HEALTHCHECK --interval=10s --timeout=5s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:5002/health || exit 1

# Start command
CMD ["uvicorn", "src.cv_server:app", "--host", "0.0.0.0", "--port", "5002"]
```

### Key Directives

| Directive | Purpose |
|-----------|---------|
| `FROM` | Base image to build from |
| `WORKDIR` | Working directory inside container |
| `COPY` | Copy files from host to container |
| `RUN` | Execute command during build |
| `ENV` | Set environment variables |
| `EXPOSE` | Document ports used |
| `HEALTHCHECK` | Define health check |
| `CMD` | Default startup command |
| `ENTRYPOINT` | Override CMD with fixed command |

## Docker Image Management

### List Images

```bash
docker image ls
docker image ls | grep til-

# Show image details
docker image inspect til-ae:latest
```

### Remove Images

```bash
docker image rm til-cv
docker image prune  # Remove unused images
```

### Tag Image

```bash
# Create alias
docker tag til-ae:latest til-ae:v1.0

# For registry
docker tag til-ae myregistry.com/til-ae:latest
```

### Push to Registry

```bash
docker push myregistry.com/til-ae:latest
```

## Container Management

### List Running Containers

```bash
docker ps
docker ps -a  # All containers (including stopped)
```

### Stop Container

```bash
docker stop <container-id>
docker kill <container-id>  # Force kill
```

### View Container Logs

```bash
docker logs <container-id>
docker logs -f <container-id>  # Follow logs (live)
docker logs --tail 100 <container-id>  # Last 100 lines
```

### Execute Command in Container

```bash
docker exec <container-id> python --version
docker exec -it <container-id> /bin/bash  # Interactive shell
```

### Copy Files

```bash
docker cp <container-id>:/app/output.txt ./local/
docker cp ./local/file.txt <container-id>:/app/
```

## Optimization Tips

### Smaller Images

```dockerfile
# Bad: Large base image
FROM ubuntu:latest
RUN apt-get update && apt-get install python3

# Good: Use pre-built image
FROM python:3.11-slim
```

### Faster Builds

```dockerfile
# Bad: Large layer
COPY . .
RUN pip install -r requirements.txt && do_other_stuff

# Good: Separate layers
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
RUN do_other_stuff
```

### Multi-Stage Builds

```dockerfile
# Builder stage (large)
FROM pytorch/pytorch:latest as builder
RUN pip install all-dev-tools

# Runtime stage (small)
FROM python:3.11-slim
COPY --from=builder /usr/local/lib .
```

## Docker Compose (for Multiple Services)

Create `docker-compose.yml`:

```yaml
version: '3.8'

services:
  ae:
    build: ./ae/submit
    ports:
      - "5003:5003"
    environment:
      - MODEL_PATH=/models/ae.pt
    volumes:
      - ./models:/models
    gpus: all
  
  cv:
    build: ./cv/submit
    ports:
      - "5002:5002"
    environment:
      - MODEL_PATH=/models/cv.pt
    volumes:
      - ./models:/models
    gpus: all
  
  nlp:
    build: ./nlp/submit
    ports:
      - "5004:5004"
    environment:
      - MODEL_PATH=/models/nlp.pt
    volumes:
      - ./models:/models
```

Run all services:
```bash
docker-compose up -d
docker-compose down
```

## Troubleshooting

### Issue: Build Fails - Module Not Found

```
ERROR: ModuleNotFoundError: No module named 'torch'
```

**Fix:**
```dockerfile
# Ensure requirements.txt is copied BEFORE pip install
COPY requirements.txt .
RUN pip install -r requirements.txt
```

### Issue: Runtime Error - Module Not Found

```
ERROR: ModuleNotFoundError: No module named 'my_package'
```

**Fix:**
```dockerfile
# Verify all dependencies are listed in requirements.txt
# Rebuild from scratch
docker build --no-cache -t til-<challenge> .
```

### Issue: Port Already in Use

```
docker: Error response from daemon: driver failed programming external connectivity
```

**Fix:**
```bash
# Use different port
docker run -p 5099:5003 til-ae

# Or find and stop conflicting container
docker ps | grep 5003
docker stop <container-id>
```

### Issue: Out of Memory

```
FATAL ERROR: CALL_AND_RETRY_LAST Allocation failed
```

**Fix:**
```bash
# Increase memory limit
docker run -m 8g til-<challenge>

# Or reduce batch size
docker run -e BATCH_SIZE=8 til-<challenge>
```

### Issue: GPU Not Available

```
RuntimeError: CUDA out of memory
```

**Fix:**
```bash
# Ensure --gpus flag
docker run --gpus all til-<challenge>

# Check NVIDIA runtime installed
docker run --rm --gpus all nvidia/cuda:11.8.0 nvidia-smi
```

## Best Practices

1. **Use `.dockerignore`** — Exclude unnecessary files
   ```
   __pycache__
   *.pyc
   .git
   .env
   models/  (too large)
   ```

2. **Non-root user** — For security
   ```dockerfile
   RUN useradd -m appuser
   USER appuser
   ```

3. **Health checks** — So orchestrators can restart
   ```dockerfile
   HEALTHCHECK --interval=30s CMD curl -f http://localhost/health
   ```

4. **Small layers** — For faster builds
   ```dockerfile
   RUN apt-get update && apt-get install -y pkg && rm -rf /var/lib/apt
   ```

5. **Pin versions** — For reproducibility
   ```dockerfile
   FROM python:3.11.0-slim  # Not :latest
   RUN pip install torch==2.0.0  # Not latest
   ```

## Pre-Submission Docker Checklist

- [ ] Image builds without errors
- [ ] Container starts without errors
- [ ] Health endpoint responds (curl http://localhost:<PORT>/health)
- [ ] Test payload produces correct output
- [ ] Performance is acceptable
- [ ] Image size reasonable (<5GB)
- [ ] GPU support verified (if needed)
- [ ] Environment variables documented

---

**For Docker docs:** https://docs.docker.com/
