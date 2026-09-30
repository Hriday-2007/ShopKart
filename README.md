# ShopKart

<p align="center">
  <strong>A thoughtful little corner of the internet for everyday finds.</strong>
  <br />
  A full-stack shopping experience built with React, Express, and MongoDB.
</p>

<p align="center">
  <a href="https://react.dev/"><img alt="React" src="https://img.shields.io/badge/React-UI-149ECA?style=flat-square&logo=react&logoColor=white"></a>
  <a href="https://vite.dev/"><img alt="Vite" src="https://img.shields.io/badge/Vite-tooling-646CFF?style=flat-square&logo=vite&logoColor=white"></a>
  <a href="https://expressjs.com/"><img alt="Express" src="https://img.shields.io/badge/Express-API-333333?style=flat-square&logo=express&logoColor=white"></a>
  <a href="https://www.mongodb.com/"><img alt="MongoDB" src="https://img.shields.io/badge/MongoDB-data-47A248?style=flat-square&logo=mongodb&logoColor=white"></a>
</p>

ShopKart is a curated storefront for browsing a small collection of useful, characterful products. Customers can create an account, sign in, explore the catalog, and view product details. The interface is built with React; an Express API handles customer sessions and product data backed by MongoDB.

## Contents

- [Highlights](#highlights)
- [Built With](#built-with)
- [Getting Started](#getting-started)
- [Environment](#environment)
- [API](#api)
- [Project Layout](#project-layout)

## Highlights

- Customer registration, login, session lookup, and logout
- HTTP-only cookie authentication with JWTs
- Product catalog with text search, category filtering, and price sorting
- Individual product detail pages
- Responsive React interface powered by Vite
- MongoDB-backed products and customers, with sample catalog data

## Built With

| Area | Technology |
| --- | --- |
| Frontend | React, React Router, Axios, Vite |
| Backend | Node.js, Express 5 |
| Database | MongoDB, Mongoose |
| Authentication | JWT, HTTP-only cookies, bcrypt |

## Getting Started

### Prerequisites

- Node.js 20 or newer and npm
- A MongoDB connection string (local MongoDB or MongoDB Atlas)

### 1. Configure the backend

Create `backend/.env`:

```dotenv
PORT=3000
MONGO_URI=mongodb://127.0.0.1:27017/shopkart
JWT_SECRET=replace-with-a-long-unique-random-secret
CLIENT_URL=http://localhost:5173
```

Use a strong, private value for `JWT_SECRET`. Do not commit `.env` files or share production credentials.

Install backend dependencies and start the API:

```bash
cd backend
npm install
npm run dev
```

The API is available at `http://localhost:3000`. Its root endpoint, `GET /`, returns a small status response.

### 2. Start the frontend

In a second terminal:

```bash
cd frontend
npm install
npm run dev
```

Open the local URL printed by Vite, usually `http://localhost:5173`. The frontend uses `http://localhost:3000` by default for API requests. To use another API URL, create `frontend/.env` and set:

```dotenv
VITE_API_URL=http://localhost:3000
```

### 3. Load sample products (optional)

With `MONGO_URI` configured in `backend/.env`, run:

```bash
cd backend
npm run seed
```

**The seed script deletes all existing products in the connected database before inserting the sample catalog.** Use a development database, and only run it when that replacement is intended.

## Environment

| Variable | Required | Description |
| --- | --- | --- |
| `MONGO_URI` | Yes | MongoDB connection string used by the API and seed script |
| `JWT_SECRET` | Yes | Private secret used to sign customer session tokens |
| `PORT` | No | API port; defaults to `3000` |
| `CLIENT_URL` | No | Additional allowed frontend origin for cross-origin requests |
| `VITE_API_URL` | No | Frontend API base URL; defaults to `http://localhost:3000` |
| `NODE_ENV` | No | Set to `production` to enable secure, cross-site auth cookies |

## API

Customer routes are mounted at `/customers`; product routes are mounted at `/products`.

| Method | Path | Description |
| --- | --- | --- |
| `POST` | `/customers/register` | Register a customer |
| `POST` | `/customers/login` | Sign in and set the session cookie |
| `GET` | `/customers/me` | Return the signed-in customer (requires authentication) |
| `POST` | `/customers/logout` | Clear the session cookie |
| `GET` | `/products` | List products; accepts `search`, `category`, and `sort` query parameters |
| `GET` | `/products/:id` | Get a product by its MongoDB ID |
| `POST` | `/products` | Create a product |

Supported `sort` values are `price_asc` and `price_desc`; without a sort value, newest products appear first. Product categories used by the storefront are Electronics, Fashion, Books, and Home.

## Project Layout

```text
ShopKart/
├── backend/
│   ├── controllers/    # Customer and product request handlers
│   ├── middlewares/    # Authentication middleware
│   ├── models/         # Mongoose schemas
│   ├── routes/         # Express route definitions
│   ├── index.js        # API setup and server startup
│   └── seed.js         # Sample product catalog
└── frontend/
    └── src/
        ├── components/ # Shared navigation, product, and route components
        ├── pages/      # Authentication, home, catalog, and product views
        └── services/   # API client
```