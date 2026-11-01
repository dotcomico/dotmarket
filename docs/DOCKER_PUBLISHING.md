# 🐳 Docker — Maintainer Guide

Notes for building, running, and publishing the project's Docker images. (For just *running* the app via Docker, see [QUICK_START.md](QUICK_START.md).)

Published images:
- **Backend** → `dotcoms/supermarket-backend`
- **Frontend** → `dotcoms/supermarket-frontend`
- **MCP server** → `dotcoms/supermarket-mcp-server`
- **AI server** → `dotcoms/supermarket-ai-server`

These names are set by the `image:` lines in `docker-compose.yml`. Every compose command needs `JWT_SECRET` set in a root `.env` file (see `.env.example`).

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

## Push All Images (first time or after code changes)

```bash
# Rebuild (only rebuilds what changed — usually fast)
docker compose build

# List the services: a plain `push` would also try to push the ollama image
docker compose push backend frontend mcp-server ai-server
```

## Best Practice: Versioned Releases

```bash
# After build + testing
VERSION=v1.2.0
for name in backend frontend mcp-server ai-server; do
  docker tag  dotcoms/supermarket-$name:latest dotcoms/supermarket-$name:$VERSION
  docker push dotcoms/supermarket-$name:$VERSION
done
```

## Pull Example

```bash
docker compose pull
docker compose up -d --no-build
```
