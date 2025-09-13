# Mern Auth

MERN authentication system. A production-ready authentication starter built with **MongoDB, Express, React (Vite), and Node.js**.  
Features secure **HTTP-only cookie** auth with JWT, protected API routes, and a polished React Bootstrap UI.



---

## ✨ Features

- 🔐 JWT auth stored in **HTTP-only cookies** (XSS-safe)
- 👤 Register / Login / Logout / Update Profile
- 🔒 Protected routes (server + client guards)
- 🧰 Redux Toolkit + RTK Query for API calls & state
- 🎨 React Bootstrap UI + React Toastify notifications
- 🗄️ MongoDB (Mongoose) models & validation
- 🧪 Error handling & async middleware
- 🚀 Single command dev workflow with `concurrently`
- 📦 Production build serves the React app via Express

---

## 🗂️ Project Structure

```
root/
├── backend/
│   ├── config/
│   │   └── db.js
│   ├── controllers/
│   │   └── userController.js
│   ├── middleware/
│   │   ├── authMiddleware.js
│   │   └── errorMiddleware.js
│   ├── models/
│   │   └── userModel.js
│   ├── routes/
│   │   └── userRoutes.js
│   ├── utils/
│   │   └── generateToken.js
│   └── server.js
├── frontend/ (Vite + React)
│   └── src/
│       ├── components/ (Header, Loader, PrivateRoute, etc.)
│       ├── screens/ (Home, Login, Register, Profile)
│       ├── slices/ (apiSlice, usersApiSlice, authSlice)
│       ├── store.js
│       └── main.jsx / App.jsx
├── package.json (root scripts: server/client/dev)
└── README.md
```

---

## 🔧 Tech Stack

**Backend**
- Node.js, Express
- Mongoose (MongoDB)
- JSON Web Tokens (JWT)
- cookie-parser
- express-async-handler
- dotenv

**Frontend**
- React 18 + Vite
- React Router v6
- React Bootstrap
- Redux Toolkit + RTK Query
- React Toastify

---

## 🔐 API Endpoints

Base path: `/api/users`

- `POST /` — Register new user  
  **Body:** `{ name, email, password }` → **Sets** HTTP-only `jwt` cookie

- `POST /auth` — Login  
  **Body:** `{ email, password }` → **Sets** HTTP-only `jwt` cookie

- `POST /logout` — Logout  
  **Clears** the `jwt` cookie

- `GET /profile` — Get current user (🔒 protected)  
  **Headers:** `Cookie: jwt=<token>`

- `PUT /profile` — Update name/email/password (🔒 protected)

> Server protection is applied using `authMiddleware.js` (verifies cookie JWT and attaches `req.user`).

---

## 🧩 Environment Variables

Create a file named `.env` in the project root (same level as `backend/` and `frontend/`) **and DO NOT commit it**.

```env
NODE_ENV=development
PORT=5000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=super_secret_jwt_key
```

> If deploying to production, set `NODE_ENV=production` and ensure HTTPS so cookies can be `secure`.

---

## ▶️ Getting Started (Development)

1) **Install dependencies** (root installs server; frontend has its own):
```bash
npm install
cd frontend && npm install
cd ..
```

2) **Run both client and server together** (concurrently):
```bash
npm run dev
```
- Server: http://localhost:5000
- Client: http://localhost:5173 (Vite)

Alternatively, run them separately:
```bash
# Terminal 1
npm run server

# Terminal 2
npm run client
```

---

## 🏗️ Build for Production

Create an optimized React build and serve it via Express:

```bash
cd frontend
npm run build
cd ..
NODE_ENV=production npm start
```

- Express statically serves `frontend/dist`
- All unknown routes (`*`) return `index.html` (SPA)

---

## 🧱 Database Model (User)

```js
{
  name: String (required),
  email: String (required, unique),
  password: String (hashed, required),
  timestamps: true
}
```

- Passwords are **salted & hashed** using `bcryptjs`.  
- `matchPassword` instance method compares raw vs hashed.  
- Pre-save hook re-hashes when password is modified.

---

## 🔒 Auth Flow

1. **Register/Login** → server **sets** signed JWT in an **HTTP-only** cookie
2. **Protected routes** check and decode cookie → attach `req.user`
3. **Client** guards (e.g., `PrivateRoute.jsx`) redirect unauthenticated users
4. **Logout** clears cookie

Cookie flags:
- `httpOnly: true`
- `secure: NODE_ENV !== 'development'`
- `sameSite: 'strict'`
- `maxAge: 30d`

---

## 🧪 Helpful Scripts

From the repo root:
```bash
npm start        # Run server in production mode
npm run server   # Nodemon: backend only (dev)
npm run client   # Vite: frontend only (dev)
npm run dev      # Run both (concurrently)
```

From `/frontend`:
```bash
npm run dev
npm run build
npm run preview
```

---

## 🚀 Deployment Notes

- Set environment variables on your host (Render, Railway, Heroku, etc.).
- Use a hosted MongoDB (MongoDB Atlas).  
- Enforce HTTPS in production so cookies use `secure: true`.
- Configure CORS only if you split client/server across domains during development.

---

## 🧭 Roadmap Ideas

- Email verification & password reset
- Role-based authorization (admin/users)
- 2FA / OTP (TOTP or email code)
- Account lockouts & rate limiting
- Audit logs, sessions management
- Dockerfile & CI/CD

---

