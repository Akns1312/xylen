# DBMS - Inventory/Warehouse Management System

This repository contains a full-stack mini DBMS project for warehouse/inventory workflows.

- `frontend`: React + Vite web app for admins and staff
- `backend`: Node.js + Express API connected to PostgreSQL

## What the project does

The system supports:

- User authentication (`signin` / `signup`) with JWT
- Role-based behavior (`admin` and `staff`)
- CRUD operations for:
  - users (admin only for management actions)
  - products
  - suppliers
  - customers
  - inventory transactions
- Transaction history with joined details (product/supplier/customer/user names)

## Tech stack

### Frontend

- React 19
- Vite 7
- React Router
- Axios
- Tailwind CSS 4

### Backend

- Node.js + Express 5
- PostgreSQL (`pg`)
- JWT (`jsonwebtoken`)
- Password hashing with `bcrypt`
- `dotenv`, `cors`

## Repository structure

```text
dbms/
  backend/
    index.js
    db.js
    package.json
  frontend/
    src/
      pages/
        landing.jsx
        signin.jsx
        signup.jsx
        admintables.jsx
        stafftables.jsx
    package.json
```

## Prerequisites

- Node.js (LTS recommended)
- npm
- PostgreSQL

## Backend setup

1. Go to backend folder:

   ```bash
   cd backend
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Create a `.env` file in `backend/`:

   ```env
   PG_USER=your_pg_user
   PG_HOST=localhost
   PG_DATABASE=your_database_name
   PG_PASSWORD=your_pg_password
   PG_PORT=5432
   ```

4. Start backend server:

   ```bash
   node index.js
   ```

Backend runs on `http://localhost:3000`.

> Note: `backend/package.json` scripts currently point to `src/index.js`, but the actual file is `backend/index.js`.

## Frontend setup

1. Go to frontend folder:

   ```bash
   cd frontend
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Start dev server:

   ```bash
   npm run dev
   ```

Frontend runs on `http://localhost:5173`.

## Authentication flow

- On sign in, frontend calls `POST /signin`
- Backend returns JWT + role
- Frontend stores:
  - `token` in localStorage
  - `userRole` in localStorage
- Protected requests send `Authorization: Bearer <token>`
- Admin-only routes are guarded by middleware on the backend

## API overview

### Public endpoints

- `POST /signup`
- `POST /signin`
- `GET /users`
- `GET /products`
- `GET /suppliers`
- `GET /customers`
- `GET /transactions`

### Auth-protected endpoints

- Products: `POST /products`, `PUT /products/:id`, `DELETE /products/:id`
- Suppliers: `POST /suppliers`, `PUT /suppliers/:id`, `DELETE /suppliers/:id`
- Customers: `POST /customers`, `PUT /customers/:id`, `DELETE /customers/:id`
- Transactions: `POST /transactions`, `PUT /transactions/:id`, `DELETE /transactions/:id`

### Admin-only endpoints

- `POST /users`
- `PUT /users/:id`
- `DELETE /users/:id`

## Expected database tables

Based on backend queries, the schema should include:

- `users`
- `products`
- `suppliers`
- `customers`
- `inventory_transactions`

`inventory_transactions` is joined with the other tables using foreign keys in `GET /transactions`.

## Current UI pages

- `/` - Landing page
- `/signin` - Sign in
- `/signup` - Sign up (currently gated in UI by an admin password prompt)
- `/admintables` - Admin dashboard for all tables
- `/stafftables` - Staff dashboard for products + transactions

## Notes

- Backend currently defines JWT secret directly in code (`index.js`). Moving this to an environment variable is recommended.
- CORS is configured for frontend origin `http://localhost:5173`.
