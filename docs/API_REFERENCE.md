# 📌 API Reference

Base URL (local): `http://localhost:3000/api`

## Auth
- `POST /api/auth/register`
- `POST /api/auth/login`
- `GET  /api/auth/me`

## Products
- `GET  /api/products`
- `GET  /api/products/:id`
- `POST /api/products` (admin)
- `PUT  /api/products/:id` (admin)
- `DELETE /api/products/:id` (admin)

## Orders
- `GET  /api/orders` (own orders)
- `POST /api/orders`
- `PUT  /api/orders/:id/status` (admin/manager)

## Admin only
- `GET  /api/users`
- `PUT  /api/users/:id/role`
- `DELETE /api/users/:id`

(Full list available in the backend repo's own README)
