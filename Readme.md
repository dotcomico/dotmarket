# 🛒 Dotmarket

**A full-stack supermarket e-commerce app** — customer shopping experience + a complete admin/manager back office, built to demonstrate production-style architecture across a decoupled frontend and backend.

[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
![React](https://img.shields.io/badge/React-19-149ECA?logo=react&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-blue?logo=typescript&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-Python%203.11-000000?logo=flask&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-database-07405E?logo=sqlite&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-ready-2496ED?logo=docker&logoColor=white)

---

## 📸 Preview

> _Screenshots/GIFs coming soon — placeholders below, swap in real captures as the app is polished._

| Customer Storefront | Admin Dashboard |
|---|---|
| ![Storefront placeholder](https://placehold.co/600x360?text=Storefront+Screenshot) | ![Admin dashboard placeholder](https://placehold.co/600x360?text=Admin+Dashboard+Screenshot) |

| Cart & Checkout | Product Management |
|---|---|
| ![Checkout placeholder](https://placehold.co/600x360?text=Checkout+Flow+GIF) | ![Product management placeholder](https://placehold.co/600x360?text=Product+CRUD+GIF) |

---

## 🧭 What This Is

Dotmarket simulates a real supermarket's online storefront alongside the internal tooling that runs it — split into two independently deployable services, the way a small e-commerce product would actually be built and shipped:

- **Customers** browse categorized products, search, manage a cart, check out, and view order history.
- **Admins & managers** get a dashboard with live stats and low-stock alerts, full CRUD over products/categories, order status control, and user role management.

## ✨ Key Features

**Customer**
- Browse & search products across a full category tree
- Cart & checkout flow
- Order history & profile management

**Admin / Manager**
- Dashboard with stats and low-stock alerts
- Product & category CRUD
- Order status management
- User role management

## 🏗️ Architecture

Frontend and backend are **separate repositories and separately deployable services**, communicating over a REST API — not a single monolith:

```mermaid
flowchart LR
    U["Customer / Admin Browser"] -->|HTTPS| FE["dotmarket-frontend\nReact 19 + TypeScript + Vite"]
    FE -->|REST API + JWT| BE["dotmarket-backend\nFlask + SQLAlchemy"]
    BE --> DB[("SQLite")]
```

| Repo | Role |
|---|---|
| [`dotmarket-frontend`](https://github.com/dotcomico/dotmarket-frontend) | Customer + admin UI |
| [`dotmarket-backend`](https://github.com/dotcomico/dotmarket-backend) | REST API, auth, business logic, database |

## 🛠️ Tech Stack

| Layer      | Technologies                              |
|------------|-------------------------------------------|
| Frontend   | React 19, TypeScript, Vite, React Router 7, Zustand, Axios |
| Backend    | Python 3.11, Flask, SQLAlchemy, JWT, Flask-CORS |
| Database   | SQLite (file-based)                       |
| Deployment | Docker (separate image per layer)         |

## 📚 Documentation

- [Quick Start](docs/QUICK_START.md) — run it locally or via Docker, test credentials, environment variables
- [API Reference](docs/API_REFERENCE.md) — REST endpoints
- [Docker Publishing Guide](docs/DOCKER_PUBLISHING.md) — build/tag/push the images (maintainers)

## 🔗 Links

- Frontend repo: https://github.com/dotcomico/dotmarket-frontend
- Backend repo: https://github.com/dotcomico/dotmarket-backend
- Live demo: _coming soon_

## 📄 License

[MIT](LICENSE)
