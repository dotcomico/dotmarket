# 🐳 Docker — Maintainer Guide

Notes for building, running, and publishing the project's Docker images. (For just *running* the app via Docker, see [QUICK_START.md](QUICK_START.md).)

Published images:
- **Backend** → `dotcoms/supermarket-backend`
- **Frontend** → `dotcoms/supermarket-frontend`

## Local Compose Workflow

```bash
# Build all images defined in docker-compose.yml
docker-compose build

# Start the services
docker-compose up
# ...or in the background
docker-compose up -d

# Rebuild after code/Dockerfile changes
docker-compose up --build -d

# Stop running services
docker-compose down

# Enter a running container
docker-compose exec <service_name> sh    # or bash

# Check running containers
docker ps
```

## GPU Acceleration for the AI Chat Assistant (optional)

By default the `ollama` service runs CPU-only — this is deliberate, so
`docker-compose up` works out of the box for anyone who pulls the images,
regardless of their hardware. CPU inference is noticeably slower (expect
several seconds to tens of seconds per chat response instead of ~1s), which
is normal and not a bug.

If you have an NVIDIA GPU with the
[NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)
installed, layer the optional override file on top to pass the GPU through
to Ollama:

```bash
docker-compose -f docker-compose.yml -f docker-compose.gpu.yml up -d
```

Don't add the GPU block to the base `docker-compose.yml` — on a machine
without the NVIDIA Container Toolkit, that reservation makes the `ollama`
container fail to start at all, breaking the default CPU path for everyone
else. Keep it in `docker-compose.gpu.yml` and opt in per-machine.

## Publishing Images — Important Notes

- You **must** be logged in → run `docker login` before any `docker push`
- If you see `denied: requested access to the resource is denied` → you are either not logged in, or you do not have push permission to the `dotcoms` namespace/org
- `:latest` is great for development & quick testing → **never use `:latest` in production deployments**, always prefer explicit version tags (`:v1.2.3`, `:2026-02-03`, etc.)
- Version tags give you control, rollback capability, and reproducibility — `:latest` can silently break things downstream

## Quick First-Time Push (create repo + initial images)

```bash
# 1. Build both images
docker-compose build

# 2. Tag & push backend
docker tag supermarket-backend dotcoms/supermarket-backend:latest
docker push    dotcoms/supermarket-backend:latest

# 3. Tag & push frontend
docker tag supermarket-frontend dotcoms/supermarket-frontend:latest
docker push    dotcoms/supermarket-frontend:latest
```

## Normal Update Workflow (after code changes)

```bash
# Rebuild (only rebuilds what changed — usually fast)
docker-compose build

# Push updated :latest tags
docker push dotcoms/supermarket-backend:latest
docker push dotcoms/supermarket-frontend:latest
```

## Best Practice: Versioned Releases

```bash
# After build + testing
docker tag supermarket-backend   dotcoms/supermarket-backend:v1.2.0
docker tag supermarket-frontend  dotcoms/supermarket-frontend:v1.2.0

# Push the version tags
docker push dotcoms/supermarket-backend:v1.2.0
docker push dotcoms/supermarket-frontend:v1.2.0

# Optional: also refresh :latest
docker push dotcoms/supermarket-backend:latest
docker push dotcoms/supermarket-frontend:latest
```

## Pull Example

```bash
docker pull dotcoms/supermarket-backend:latest
docker pull dotcoms/supermarket-frontend:latest

# Then start everything (recommended):
docker-compose up -d
```

## Convenience Push Script (optional but handy)

Save as `push.sh`:

```bash
#!/usr/bin/env bash
set -euo pipefail

VERSION="${1:-latest}"

echo "→ Building images..."
docker-compose build

echo "→ Tagging as ${VERSION} ..."
docker tag supermarket-backend   dotcoms/supermarket-backend:${VERSION}
docker tag supermarket-frontend  dotcoms/supermarket-frontend:${VERSION}

echo "→ Pushing ${VERSION} ..."
docker push dotcoms/supermarket-backend:${VERSION}
docker push dotcoms/supermarket-frontend:${VERSION}

echo "Done → pushed version: ${VERSION}"
```
