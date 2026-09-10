# 📌 API Reference

Base URL (local): `http://localhost:3000/api`

## Auth
- `POST /api/auth/register`
- `POST /api/auth/login`
- `GET  /api/auth/me`

## Products
- `GET  /api/products`
- `GET  /api/products/:id`
- `GET  /api/products/stats` (admin/manager) — dataset-wide inventory aggregates:
  `totalProducts`, `lowStockCount`, `outOfStockCount`, `inventoryValue`,
  `lowStockThreshold`, and `lowStockProducts` (up to 5, lowest stock first; each
  is `{ id, name, stock, price }` — no `image`, the panel renders no thumbnail).
  Computed in SQL — clients must not re-derive these from a paginated page.
- `POST /api/products` (admin/manager)
- `PUT  /api/products/:id` (admin/manager)
- `DELETE /api/products/:id` (admin)

## Categories
- `GET  /api/categories` — flat list
- `GET  /api/categories/tree` — nested tree
- `GET  /api/categories/:slug` — one category + breadcrumbs
- `GET  /api/categories/:slug/products`
- `POST /api/categories` (admin/manager)
- `GET  /api/categories/:id` (admin/manager) — by numeric id; this rule wins over
  `/:slug`, so the slug route only resolves for non-numeric slugs
- `PUT  /api/categories/:id` (admin/manager)
- `DELETE /api/categories/:id` (admin)

## Orders
- `GET  /api/orders` — **all** orders for admin/manager; a customer gets `[]`.
  This is the admin order list, not "my orders".
- `GET  /api/orders/privet` — the logged-in user's own orders. The route name is
  a typo for "private" that shipped as-is; the frontend calls it, so it is
  documented rather than renamed.
- `GET  /api/orders/:id` — the order's owner, or any admin/manager
- `POST /api/orders`
- `PUT  /api/orders/:id` (admin/manager) — update status. There is no
  `/:id/status` sub-route.
- `DELETE /api/orders/:id` (admin)

## Users
- `GET  /api/users/profile` — the logged-in user's own profile
- `GET  /api/users` (admin) — each user also carries `ordersCount` (all orders,
  cancelled included) and `totalSpent` (sum of `totalAmount` for paid/shipped
  orders only, matching the dashboard's Total Revenue definition).
- `PUT  /api/users/:id/role` (admin)

There is no `DELETE /api/users/:id` — user deletion is not implemented.

(Full list available in the backend repo's own README)
