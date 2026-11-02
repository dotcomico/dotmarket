# 🚀 Quick Start

## Fastest Way (Docker)

Runs the full app, including the AI chat assistant, from the published images.
You only need Docker and this repo. The first start downloads the chat model
(about 4.7 GB).

```bash
git clone https://github.com/dotcomico/dotmarket.git
cd dotmarket
cp .env.example .env
```

Set `JWT_SECRET` in `.env`, then:

```bash
docker compose pull
docker compose up -d --no-build
```

→ Open http://localhost

> The Docker images already contain seeded test data.

Test accounts (all passwords: `Test123!`):

| Role     | Email                |
|----------|----------------------|
| Admin    | admin@test.com       |
| Manager  | manager@test.com     |
| Customer | customer@test.com    |

Ollama runs CPU-only by default. Chat responses still work, just slower
(several seconds to tens of seconds per reply instead of ~1s) — this is
expected, not a bug.

**Have an NVIDIA GPU** (with the
[NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)
installed):

```bash
docker compose -f docker-compose.yml -f docker-compose.gpu.yml up -d --no-build
```

Same stack, but Ollama gets GPU passthrough for much faster chat responses.
See [Docker Publishing Guide](DOCKER_PUBLISHING.md#gpu-acceleration-for-the-ai-chat-assistant-optional)
for why this is a separate opt-in file rather than the default.

Stop everything with:

```bash
docker compose down
```

## Build the Docker Images from Source

With `backend/`, `frontend/`, `mcp-server/` and `ai-server/` cloned into this
folder, and `.env` set up as above:

```bash
docker compose up -d --build
```

## Local Development

### Prerequisites

- Node.js 18+ / npm
- Python 3.11+
- Git

### Step-by-step (fresh clone)

```bash
# 1. Clone repositories
git clone https://github.com/dotcomico/dotmarket-server.git backend
git clone https://github.com/dotcomico/dotmarket-client.git frontend
```

```bash
# 2. Backend
cd backend
python -m venv venv
source venv/bin/activate     # Windows: venv\Scripts\activate
pip install -r requirements.txt

# Optional: fresh database + test data
rm database.sqlite           # or supermarket.db – whatever your config uses
python seed_database.py

# Start backend
python -m src.main
# → http://localhost:3000
```

```bash
# 3. Frontend (in new terminal)
cd ../frontend
npm install
npm run dev
# → http://localhost:5173
```

Frontend expects backend at `http://localhost:3000`

This covers the storefront + admin app. To also run the optional AI chat
assistant, see [Optional: AI Chat Assistant](#optional-ai-chat-assistant-mcp-server--ai-server) below.

## Daily Dev (Already Set Up)

The steps above are for a **fresh clone/fork** (they create the venv, install deps from scratch). If `venv` (backend) and `node_modules` (frontend) already exist on your machine, skip straight to:

```bash
# Terminal 1 — Backend
cd backend
venv\Scripts\activate     # macOS/Linux: source venv/bin/activate
python -m src.main
# → http://localhost:3000
```

```bash
# Terminal 2 — Frontend
cd frontend
npm run dev
# → http://localhost:5173
```

Re-run `pip install -r requirements.txt` / `npm install` only if dependencies changed or the environment is missing/broken.

## Reset & Re-seed Database

```bash
cd backend
rm database.sqlite
python seed_database.py
```

Includes:
- 60 categories (12 parents + subcategories)
- 62 products (some low/out-of-stock for testing)
- 5 realistic orders in different states

## Optional: AI Chat Assistant (mcp-server + ai-server)

Two more services, on top of the backend already running above, power an
optional chat widget that answers product/category questions grounded in
real backend data: `mcp-server` exposes the backend as MCP tools, and
`ai-server` runs the LLM tool-calling loop and exposes an HTTP `/chat`
endpoint. Full details (architecture, tool list, env vars) are in each
service's own README — this is the fast path to get all four services up
together.

```bash
# 1. Clone (if not already present as siblings of backend/ and frontend/)
git clone https://github.com/dotcomico/dotmarket-mcp-server.git mcp-server
git clone https://github.com/dotcomico/dotmarket-ai-server.git ai-server
```

```bash
# 2. mcp-server (needs backend already running — see above)
cd mcp-server
python -m venv venv
venv\Scripts\activate        # macOS/Linux: source venv/bin/activate
pip install -r requirements.txt
copy .env.example .env       # defaults to http://localhost:3000 for the backend
python -m src.server
# → listens on http://127.0.0.1:8000/mcp
```

```bash
# 3. ai-server (needs mcp-server already running)
cd ../ai-server
python -m venv venv
venv\Scripts\activate        # macOS/Linux: source venv/bin/activate
pip install -r requirements.txt
copy .env.example .env       # set OPENAI_API_KEY, or point LLM_BASE_URL at a
                              # local Ollama server for free local dev
uvicorn src.api:app --port 8100 --reload
# → http://127.0.0.1:8100
```

```bash
# 4. Test it end-to-end
curl -X POST http://127.0.0.1:8100/chat \
  -H "Content-Type: application/json" \
  -d "{\"message\": \"What products do you have under $5?\"}"
# → {"reply": "..."} grounded in real seeded product data
```

Run order matters: backend → mcp-server → ai-server. Each step's README
(`mcp-server/README.md`, `ai-server/README.md`) also covers manual testing
with the MCP Inspector and Claude Desktop, which is worth doing before
wiring the chat widget into the frontend.

To record chats for debugging, set `CHAT_TRACE_ENABLED=true` in `ai-server/.env`,
or in the root `.env` when running Docker. See "Chat tracing" in
`ai-server/README.md`.

## Environment Variables (backend)

| Name         | Description                  | Required? | Default    |
|--------------|------------------------------|-----------|------------|
| `PORT`       | Server port                  | no        | 3000       |
| `JWT_SECRET` | Secret for signing tokens    | **yes**   | —          |
| `DB_STORAGE` | SQLite file path             | no        | `./database.sqlite` |
| `RATE_LIMIT` | Requests allowed per client  | no        | `1000 per 15 minutes` in Docker; otherwise set by `NODE_ENV` |

## Test Credentials

All environments use the same test accounts:

| Role     | Email                | Password   |
|----------|----------------------|------------|
| Admin    | admin@test.com       | Test123!   |
| Manager  | manager@test.com     | Test123!   |
| Customer | customer@test.com    | Test123!   |
