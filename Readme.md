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

<p align="center">
  <img src="media/screenshots/hero-storefront.png" width="850" alt="Dotmarket storefront home page, logged in — search bar, cart badge showing 4 items, a category tile grid, and a product grid of real named products with photography, descriptions, and prices, with inline quantity steppers on items already in the cart">
  <br><sub><b>Storefront</b> — home page, logged in</sub>
</p>

**Storefront & admin, side by side:**

<table>
<tr>
<td width="50%">
<img src="media/screenshots/products-browse.png" width="100%" alt="Fruits & Vegetables category page with breadcrumb, category icon and title, a Shop by Category row of subcategories proving the category tree, and a full grid of products">
<sub><b>Category browsing</b> — breadcrumb + subcategory tree</sub>
</td>
<td width="50%">
<img src="media/screenshots/admin-dashboard.png" width="100%" alt="Dotmarket admin dashboard — Total Revenue, Total Orders, a triggered Low Stock Alert of 7 items, Total Products at 84, a Recent Orders table spanning five different customers, and a Low Stock Alert panel listing the specific out-of-stock and low-stock products">
<sub><b>Admin dashboard</b> — live stats + low-stock alerts</sub>
</td>
</tr>
</table>

**See it in action:**

<p align="center">
  <img src="media/screenshots/cart-checkout.gif" width="700" alt="Animated walkthrough: adding Coffee Beans to the cart from its product page, the cart view with a free-shipping progress bar, filling in checkout shipping details, and reaching the order confirmation screen">
  <br><sub><b>Cart → checkout</b> — add to cart, free-shipping progress, order confirmation</sub>
</p>

<p align="center">
  <img src="media/screenshots/admin-product-crud.gif" width="700" alt="Animated walkthrough: opening the Edit Product modal for Apple in the admin Product Management table, changing its price, saving, and seeing the updated price reflected in the table">
  <br><sub><b>Admin product editing</b> — inline CRUD from the Product Management table</sub>
</p>

**🔄 Every one of the 84 products has a real 360° spin, not just a photo:**

<p align="center">
  <img src="media/screenshots/product-360-spin.gif" width="700" alt="Animated 360° spin of the Horizon Smart Hub & Speaker after toggling from Photo to 360° View on the product page">
  <br><sub><b>360° product viewer</b> — Photo / 360° toggle on the product page</sub>
</p>

<details>
<summary><b>📱 Responsive — view on mobile</b></summary>
<br>
<p align="center">
  <img src="media/screenshots/responsive-strip.png" width="850" alt="Dotmarket on a 390px-wide phone: the storefront home screen, a product detail page with the Photo/360° toggle, and the cart with an order summary and free-shipping progress bar">
</p>
</details>

---

## 🧭 What This Is

Dotmarket simulates a real supermarket's online storefront alongside the internal tooling that runs it — split into two independently deployable services, the way a small e-commerce product would actually be built and shipped:

- **Customers** browse categorized products, search, manage a cart, check out, and view order history.
- **Admins & managers** get a dashboard with live stats and low-stock alerts, full CRUD over products/categories, order status control, and user role management.

## ✨ Key Features

**Customer**
- Browse & search products across a full category tree
- 🔄 **360° product viewer** — every one of the 84 seeded products ships a real spin GIF, toggled from a Photo/360° switch on the product page
- Cart & checkout flow
- Order history & profile management

**Admin / Manager**
- Dashboard with stats and low-stock alerts
- Product & category CRUD
- Order status management
- User role management

## 🔐 Engineering Highlights

`RBAC` `JWT Auth` `bcrypt` `Rate Limiting` `Layered Architecture` `Centralized Error Handling` `Zustand`

<details>
<summary>What's behind these tags</summary>

- **Role-based access control** — a custom `@checkRole` decorator guards admin/manager-only routes (products, categories, orders, users) on top of JWT identity, instead of checking roles ad-hoc in each handler
- **Password security** — bcrypt with salted hashing (never plaintext or reversible encryption) for stored credentials
- **Rate limiting** — Flask-Limiter on the API to blunt brute-force and abuse, not just a demo without production guardrails
- **Layered backend architecture** — routes → services → models, with a shared validators module, so business logic isn't stuck in route handlers and is easy to unit test
- **Centralized error handling** — a single Flask error handler normalizes exceptions into consistent JSON error responses instead of leaking stack traces or inconsistent shapes per route
- **Client state via Zustand** — separate, focused stores (auth, cart, orders, products, categories) instead of one global blob or prop-drilling

</details>

## 🏗️ Architecture

Frontend and backend are **separate repositories and separately deployable services**, communicating over a REST API — not a single monolith:

```mermaid
flowchart LR
    U["Customer / Admin Browser"] -->|HTTPS| FE["dotmarket-client\nReact 19 + TypeScript + Vite"]
    FE -->|REST API + JWT| BE["dotmarket-server\nFlask + SQLAlchemy"]
    BE --> DB[("SQLite")]
```

| Repo | Role |
|---|---|
| [`dotmarket-client`](https://github.com/dotcomico/dotmarket-client) | Customer + admin UI |
| [`dotmarket-server`](https://github.com/dotcomico/dotmarket-server) | REST API, auth, business logic, database |

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

- Frontend repo: https://github.com/dotcomico/dotmarket-client
- Backend repo: https://github.com/dotcomico/dotmarket-server
- Live demo: _coming soon_

## 📄 License

[MIT](LICENSE)
