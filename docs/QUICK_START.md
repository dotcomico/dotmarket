# 🚀 Quick Start

## Fastest Way (Docker)

```bash
# 1. Start backend (port 3000)
docker pull dotcoms/supermarket-backend:latest
docker run -d -p 3000:3000 dotcoms/supermarket-backend:latest

# 2. Start frontend (port 80)
docker pull dotcoms/supermarket-frontend:latest
docker run -d -p 80:80 dotcoms/supermarket-frontend:latest
```

→ Open http://localhost

> The Docker images already contain seeded test data.

Test accounts (all passwords: `Test123!`):

| Role     | Email                |
|----------|----------------------|
| Admin    | admin@test.com       |
| Manager  | manager@test.com     |
| Customer | customer@test.com    |

## Local Development

### Prerequisites

- Node.js 18+ / npm
- Python 3.11+
- Git

### Step-by-step (fresh clone)

```bash
# 1. Clone repositories
git clone https://github.com/dotcomico/dotmarket-backend.git
git clone https://github.com/dotcomico/dotmarket-frontend.git
```

```bash
# 2. Backend
cd dotmarket-backend
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
cd ../dotmarket-frontend
npm install
npm run dev
# → http://localhost:5173
```

Frontend expects backend at `http://localhost:3000`

## Daily Dev (Already Set Up)

The steps above are for a **fresh clone/fork** (they create the venv, install deps from scratch). If `venv` (backend) and `node_modules` (frontend) already exist on your machine, skip straight to:

```bash
# Terminal 1 — Backend
cd backend-py
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
cd backend-py
rm database.sqlite
python seed_database.py
```

Includes:
- 60 categories (12 parents + subcategories)
- 62 products (some low/out-of-stock for testing)
- 5 realistic orders in different states

## Environment Variables (backend)

| Name         | Description                  | Required? | Default    |
|--------------|------------------------------|-----------|------------|
| `PORT`       | Server port                  | no        | 3000       |
| `JWT_SECRET` | Secret for signing tokens    | **yes**   | —          |
| `DB_STORAGE` | SQLite file path             | no        | `./database.sqlite` |

## Test Credentials

All environments use the same test accounts:

| Role     | Email                | Password   |
|----------|----------------------|------------|
| Admin    | admin@test.com       | Test123!   |
| Manager  | manager@test.com     | Test123!   |
| Customer | customer@test.com    | Test123!   |
