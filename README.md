# 🛍️ Momentra AI Commerce

> A production-ready, full-stack AI-powered e-commerce platform built with React, Node.js, MongoDB, and OpenAI.

![Momentra Banner](https://images.unsplash.com/photo-1556742049-0cfed4f6a45d?w=1200&h=400&fit=crop)

---

## ✨ Features

### 🧠 AI-Powered
- **Smart Recommendations** — personalized based on order history & browsing
- **Momo AI Chatbot** — floating assistant powered by GPT-3.5 (falls back to smart mock)
- **"You May Also Like"** — category-aware related products
- **Behavior Tracking** — recently viewed, preference learning

### 🔐 Auth & Security
- JWT authentication with refresh support
- bcrypt password hashing (12 rounds)
- Role-based access control (User / Admin)
- Rate limiting, CORS, Helmet security headers

### 👤 User Features
- Browse, search & filter products
- Add to cart / wishlist
- Multi-step checkout (Shipping → Payment → Review)
- Order history & detailed tracking
- Profile management

### 🛒 Admin Dashboard
- Revenue charts (Recharts area + pie)
- Product CRUD with image upload (Cloudinary)
- Order management with status updates
- User management & role control

### 💳 Payments
- Stripe (cards) integration
- Razorpay integration
- Cash on Delivery fallback
- Mock payment mode for development

### 🎨 UI/UX
- Clash Display + DM Sans typography
- Brand orange (#f97316) accent system
- Full dark mode toggle
- Framer Motion page transitions
- Mobile-responsive (320px to 4K)
- Skeleton loading states

---

## 📁 Project Structure

```
momentra/
├── client/                     # React + Vite frontend
│   ├── src/
│   │   ├── components/
│   │   │   ├── ai/             # AIChatWidget (floating Momo)
│   │   │   ├── common/         # Spinner, Skeleton, Pagination, EmptyState
│   │   │   ├── layout/         # Navbar, Footer
│   │   │   └── product/        # ProductCard
│   │   ├── pages/
│   │   │   ├── admin/          # Dashboard, Products, Orders, Users, AddProduct
│   │   │   ├── HomePage.jsx
│   │   │   ├── ProductsPage.jsx
│   │   │   ├── ProductDetailPage.jsx
│   │   │   ├── CartPage.jsx
│   │   │   ├── CheckoutPage.jsx
│   │   │   ├── OrdersPage.jsx
│   │   │   ├── OrderDetailPage.jsx
│   │   │   ├── WishlistPage.jsx
│   │   │   ├── ProfilePage.jsx
│   │   │   ├── LoginPage.jsx
│   │   │   └── RegisterPage.jsx
│   │   ├── services/
│   │   │   └── api.js          # Axios with all API calls
│   │   ├── store/
│   │   │   ├── authStore.js    # Zustand auth state
│   │   │   ├── cartStore.js    # Zustand cart state
│   │   │   └── themeStore.js   # Dark mode state
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   └── index.css
│   ├── tailwind.config.js
│   ├── vite.config.js
│   └── .env.example
│
├── server/                     # Node.js + Express backend
│   ├── controllers/
│   │   ├── authController.js
│   │   ├── productController.js
│   │   ├── orderController.js
│   │   ├── cartController.js
│   │   ├── userController.js
│   │   ├── aiController.js
│   │   └── paymentController.js
│   ├── models/
│   │   ├── User.js
│   │   ├── Product.js
│   │   ├── Order.js
│   │   └── Cart.js
│   ├── routes/
│   │   ├── authRoutes.js
│   │   ├── productRoutes.js
│   │   ├── orderRoutes.js
│   │   ├── cartRoutes.js
│   │   ├── userRoutes.js
│   │   ├── aiRoutes.js
│   │   ├── paymentRoutes.js
│   │   ├── reviewRoutes.js
│   │   ├── uploadRoutes.js
│   │   └── wishlistRoutes.js
│   ├── middleware/
│   │   └── authMiddleware.js   # protect, adminOnly, optionalAuth
│   ├── server.js
│   ├── package.json
│   └── .env.example
│
└── package.json                # Root with concurrently dev script
```

---

## 🚀 Quick Start

### Prerequisites
- Node.js v18+
- MongoDB (local or Atlas)
- npm or yarn

### 1. Clone & Install

```bash
git clone https://github.com/yourname/momentra-ai-commerce.git
cd momentra-ai-commerce
npm run install:all
```

### 2. Configure Environment

**Server** — copy `server/.env.example` to `server/.env`:
```bash
cp server/.env.example server/.env
```

Edit `server/.env`:
```env
NODE_ENV=development
PORT=5000
MONGO_URI=mongodb://localhost:27017/momentra
JWT_SECRET=your_super_secret_key_minimum_32_chars
JWT_EXPIRE=30d

# Optional — AI features degrade gracefully without these
OPENAI_API_KEY=sk-...
CLOUDINARY_CLOUD_NAME=...
CLOUDINARY_API_KEY=...
CLOUDINARY_API_SECRET=...
STRIPE_SECRET_KEY=sk_test_...
RAZORPAY_KEY_ID=rzp_test_...
RAZORPAY_KEY_SECRET=...

CLIENT_URL=http://localhost:5173
```

**Client** — copy `client/.env.example` to `client/.env`:
```bash
cp client/.env.example client/.env
```

Edit `client/.env`:
```env
VITE_API_URL=http://localhost:5000/api
VITE_STRIPE_PUBLISHABLE_KEY=pk_test_...
```

### 3. Seed Sample Data (Optional)

```bash
cd server
node utils/seeder.js
```

### 4. Run Development

```bash
# From root — starts both client and server
npm run dev

# Or run separately:
npm run server   # http://localhost:5000
npm run client   # http://localhost:5173
```

### 5. Create Admin Account

Register normally, then in MongoDB:
```javascript
db.users.updateOne({ email: "your@email.com" }, { $set: { role: "admin" } })
```

Or during development, pass `"role": "admin"` in the register request body.

---

## 📡 API Documentation

### Base URL
```
http://localhost:5000/api
```

All protected routes require: `Authorization: Bearer <token>`

---

### 🔐 Auth

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/auth/register` | — | Register new user |
| POST | `/auth/login` | — | Login, returns token |
| GET | `/auth/me` | ✅ | Get current user |
| PUT | `/auth/profile` | ✅ | Update profile |
| PUT | `/auth/change-password` | ✅ | Change password |

**Register:**
```json
POST /auth/register
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "password123"
}
```

**Login Response:**
```json
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR...",
  "user": { "_id": "...", "name": "Jane", "role": "user", ... }
}
```

---

### 📦 Products

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/products` | — | List products (paginated, filterable) |
| GET | `/products/featured` | — | Featured products |
| GET | `/products/categories` | — | Category list with counts |
| GET | `/products/:id` | — | Single product with reviews |
| GET | `/products/:id/related` | — | Related products |
| POST | `/products` | Admin | Create product |
| PUT | `/products/:id` | Admin | Update product |
| DELETE | `/products/:id` | Admin | Soft-delete product |
| POST | `/products/:id/reviews` | ✅ | Add review |

**Query Parameters for GET /products:**
```
?page=1&limit=12&category=Electronics&search=headphones
&minPrice=500&maxPrice=5000&sort=-rating&featured=true&brand=Sony
```

**Sort options:** `-createdAt` (default), `price`, `-price`, `-rating`, `-sold`

---

### 🛒 Cart

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/cart` | ✅ | Get user's cart |
| POST | `/cart` | ✅ | Add item `{ productId, quantity }` |
| PUT | `/cart/:itemId` | ✅ | Update quantity `{ quantity }` |
| DELETE | `/cart/:itemId` | ✅ | Remove item |
| DELETE | `/cart` | ✅ | Clear cart |

---

### 📋 Orders

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/orders` | ✅ | Create order |
| GET | `/orders/my` | ✅ | User's orders |
| GET | `/orders/:id` | ✅ | Single order |
| GET | `/orders` | Admin | All orders |
| PUT | `/orders/:id/status` | Admin | Update status |

**Create Order:**
```json
POST /orders
{
  "items": [{ "product": "productId", "quantity": 2 }],
  "shippingAddress": {
    "street": "123 Main St",
    "city": "Mumbai",
    "state": "Maharashtra",
    "country": "India",
    "zipCode": "400001"
  },
  "paymentMethod": "cod"
}
```

**Order Statuses:** `pending` → `processing` → `shipped` → `delivered` | `cancelled` | `refunded`

---

### 👥 Users (Admin)

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/users` | Admin | List all users |
| GET | `/users/:id` | Admin | Single user |
| PUT | `/users/:id/role` | Admin | Change role |
| PUT | `/users/:id/status` | Admin | Activate/deactivate |
| GET | `/users/wishlist` | ✅ | Get wishlist |
| POST | `/users/wishlist/:productId` | ✅ | Toggle wishlist |

---

### 🧠 AI

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/ai/chat` | Optional | Chat with Momo `{ message, history[] }` |
| GET | `/ai/recommendations` | Optional | Personalized product recs |
| POST | `/ai/track` | Optional | Track behavior `{ productId, action }` |
| GET | `/ai/recently-viewed` | ✅ | Recently viewed products |
| GET | `/ai/dashboard-stats` | Admin | Dashboard analytics |

---

### 💳 Payments

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| POST | `/payment/stripe/intent` | ✅ | Create Stripe PaymentIntent |
| POST | `/payment/confirm` | ✅ | Confirm payment on order |
| POST | `/payment/razorpay/order` | ✅ | Create Razorpay order |

---

## 🌍 Deployment

### Frontend → Vercel

```bash
cd client
npm run build

# Or connect GitHub repo to Vercel
# Set environment variables in Vercel dashboard:
# VITE_API_URL=https://your-backend.onrender.com/api
# VITE_STRIPE_PUBLISHABLE_KEY=pk_live_...
```

`vercel.json` (create in `/client`):
```json
{
  "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }]
}
```

### Backend → Render

1. Create a new **Web Service** on [Render](https://render.com)
2. Connect your GitHub repo
3. Set **Root Directory:** `server`
4. **Build Command:** `npm install`
5. **Start Command:** `node server.js`
6. Add all environment variables from `.env.example`

### Database → MongoDB Atlas

1. Create cluster at [mongodb.com/atlas](https://mongodb.com/atlas)
2. Create database user
3. Whitelist IP `0.0.0.0/0` (for Render)
4. Copy connection string → `MONGO_URI` in Render env vars

---

## 🔧 Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18, Vite, Tailwind CSS |
| State | Zustand |
| Routing | React Router v6 |
| HTTP | Axios |
| Animation | Framer Motion |
| Charts | Recharts |
| Backend | Node.js, Express.js |
| Database | MongoDB, Mongoose |
| Auth | JWT, bcryptjs |
| AI | OpenAI GPT-3.5 (+ mock fallback) |
| Payments | Stripe, Razorpay |
| Images | Cloudinary |
| Security | Helmet, CORS, Rate Limiting |

---

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/amazing-feature`
3. Commit: `git commit -m 'Add amazing feature'`
4. Push: `git push origin feature/amazing-feature`
5. Open a Pull Request

---

## 📄 License

MIT License — free to use for personal and commercial projects.

---

<div align="center">
  <strong>Built with ❤️ by the Momentra team</strong><br>
  <sub>AI-Powered Commerce for the Modern Web</sub>
</div>
