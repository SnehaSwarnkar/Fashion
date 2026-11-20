# WearWise — AI Personal Fashion Stylist

A full-stack AI fashion platform with hand-drawn illustration design, virtual try-on, avatar builder, AI stylist chat, and a complete shop.

---

## 🏗️ Project Structure

```
fashion/
├── backend/                    ← FastAPI Python backend
│   ├── app/
│   │   ├── main.py             ← FastAPI app entry point, CORS, router registration
│   │   ├── database.py         ← Lightweight JSON file DB (no external DB needed)
│   │   ├── routers/            ← API route handlers
│   │   │   ├── auth.py         ← /api/auth/*  (signup, login, me, profile)
│   │   │   ├── products.py     ← /api/products/*
│   │   │   ├── catalogue.py    ← /api/catalogue/{gender}
│   │   │   ├── cart.py         ← /api/cart/*
│   │   │   ├── orders.py       ← /api/orders/*
│   │   │   ├── avatar.py       ← /api/avatar/*
│   │   │   ├── wardrobe.py     ← /api/wardrobe/*
│   │   │   ├── saved.py        ← /api/saved/*
│   │   │   ├── assistant.py    ← /api/assistant/chat
│   │   │   └── tryon.py        ← /api/tryon/*
│   │   └── schemas/            ← Pydantic request/response models
│   ├── data/                   ← JSON collection files (auto-created)
│   ├── uploads/                ← User-uploaded files (auto-created)
│   │   ├── tryon/
│   │   ├── wardrobe/
│   │   └── avatars/
│   ├── venv/                   ← Python virtual environment (not in Git)
│   ├── requirements.txt
│   ├── run.sh                  ← Convenience start script
│   └── .env.example
│
├── src/                        ← React + Vite frontend
│   ├── api/                    ← Frontend API service layer
│   │   ├── client.js           ← Base fetch wrapper + session ID
│   │   ├── auth.js             ← Authentication calls
│   │   ├── cart.js             ← Cart operations
│   │   ├── catalogue.js        ← Gender-specific shop catalogue
│   │   ├── products.js         ← General product listing
│   │   ├── orders.js           ← Order placement and history
│   │   ├── avatar.js           ← Avatar profile save/analyze
│   │   ├── wardrobe.js         ← Wardrobe upload/list/delete
│   │   ├── saved.js            ← Saved looks + wishlist
│   │   ├── assistant.js        ← AI stylist chat
│   │   └── tryon.js            ← Virtual try-on upload + process
│   ├── context/
│   │   ├── AuthContext.jsx     ← User auth state
│   │   └── ThemeContext.jsx    ← Light theme stub
│   ├── pages/                  ← All page components
│   └── styles/                 ← Global design system
│
├── .env.example                ← Frontend env template
├── .env                        ← Your local env (not in Git)
├── package.json
└── vite.config.js
```

---

## 🚀 Quick Start

### 1. Backend

```bash
cd fashion/backend

# Create and activate virtual environment
python3 -m venv venv
source venv/bin/activate       # macOS / Linux
# venv\Scripts\activate        # Windows

# Install dependencies
pip install -r requirements.txt

# Start the server (auto-reload on changes)
./run.sh
# OR: uvicorn app.main:app --reload --port 8000
```

Backend runs at: **http://localhost:8000**  
Swagger docs: **http://localhost:8000/docs**

### 2. Frontend

```bash
cd fashion

# Copy and configure environment
cp .env.example .env
# Edit .env if your backend runs on a different port

# Install dependencies (first time only)
npm install

# Start Vite dev server
npm run dev
```

Frontend runs at: **http://localhost:5173**

---

## 🔌 API Route Map

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/health` | Health check |
| POST | `/api/auth/signup` | Register new user |
| POST | `/api/auth/login` | Login, returns token |
| GET | `/api/auth/me` | Get current user |
| PATCH | `/api/auth/profile` | Update gender/body profile |
| GET | `/api/products/` | List all products |
| GET | `/api/products/{id}` | Single product |
| GET | `/api/catalogue/{gender}` | Shop by gender + filters |
| GET | `/api/cart/{session_id}` | Get cart |
| POST | `/api/cart/add` | Add to cart |
| PUT | `/api/cart/update` | Update quantity |
| DELETE | `/api/cart/{session_id}/{item_id}` | Remove item |
| DELETE | `/api/cart/{session_id}` | Clear cart |
| POST | `/api/orders/place` | Place order |
| GET | `/api/orders/my` | My orders (auth required) |
| GET | `/api/orders/detail/{id}` | Order detail |
| POST | `/api/avatar/save` | Save avatar profile |
| GET | `/api/avatar/{session_id}` | Get avatar profile |
| POST | `/api/avatar/analyze-photo` | AI photo analysis |
| POST | `/api/wardrobe/upload` | Upload clothing images |
| GET | `/api/wardrobe/{session_id}` | List wardrobe items |
| PATCH | `/api/wardrobe/{item_id}` | Update wardrobe item |
| DELETE | `/api/wardrobe/{item_id}` | Delete wardrobe item |
| POST | `/api/saved/looks` | Save a look |
| GET | `/api/saved/looks/{session_id}` | List saved looks |
| DELETE | `/api/saved/looks/{look_id}` | Remove saved look |
| POST | `/api/saved/wishlist` | Add to wishlist |
| GET | `/api/saved/wishlist/{session_id}` | List wishlist |
| DELETE | `/api/saved/wishlist/{item_id}` | Remove from wishlist |
| POST | `/api/assistant/chat` | AI stylist chat |
| POST | `/api/tryon/upload-photo` | Upload user photo |
| POST | `/api/tryon/process` | Apply look to photo |
| GET | `/api/tryon/{session_id}` | Get try-on result |

---

## 🔐 Environment Variables

### Frontend (`fashion/.env`)
```
VITE_API_URL=http://localhost:8000
```

### Backend (`fashion/backend/.env.example`)
```
CORS_ORIGINS=http://localhost:5173,http://127.0.0.1:5173
SECRET_KEY=change-me-to-a-long-random-secret-in-production
```

---

## 🗄️ Database

The backend uses a **JSON file database** — no external database required.

Each collection is stored as a `.json` file in `backend/data/`:

| File | Contents |
|------|----------|
| `users.json` | Registered user accounts |
| `carts.json` | Active shopping carts |
| `orders.json` | Placed orders |
| `avatar_profiles.json` | Style profiles |
| `wardrobe.json` | Uploaded wardrobe items |
| `saved_looks.json` | Saved look collections |
| `wishlist.json` | Product wishlists |
| `tryon_sessions.json` | Virtual try-on sessions |

To reset any collection, delete the corresponding `.json` file.

---

## 📦 Build for Production

### Frontend
```bash
cd fashion
npm run build
# Output: fashion/dist/
```

### Backend
For production, replace the token system with JWT and configure a proper WSGI server:
```bash
pip install gunicorn
gunicorn app.main:app -w 4 -k uvicorn.workers.UvicornWorker
```

---

## 🔑 Authentication

The backend uses a simple **deterministic SHA-256 token** (`sha256("wearwise-{user_id}")`).

For production, replace with **JWT** using `python-jose` (already installed in requirements.txt).

Tokens are stored in `localStorage` as `ww-auth-token` by the frontend.

---

## ⚠️ Known Limitations

| Feature | Status | Notes |
|---------|--------|-------|
| Virtual Try-On AI | Simulated | Returns original photo. Integrate a real try-on model (e.g. DressCode, IDM-VTON) in `routers/tryon.py` |
| Avatar Photo Analysis | Simulated | Returns rule-based profile. Integrate a CV model in `routers/avatar.py` |
| AI Stylist | Rule-based | Keyword intent detection. Replace `_generate_reply()` with an LLM call (OpenAI, Gemini, etc.) |
| Auth tokens | Simple SHA-256 | Upgrade to JWT for production |
| Database | JSON files | Upgrade to PostgreSQL/MongoDB for production scale |
