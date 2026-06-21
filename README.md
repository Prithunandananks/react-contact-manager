# ConnectFlow — Smart Contact CRM

ConnectFlow is a production-ready, portfolio-grade Smart Contact CRM built for modern teams. It features a sleek SaaS design system inspired by Vercel, Linear, and Stripe, and supports real-time filtering, detailed chronological activity audits, CSV contact exports, file uploads, and secure JWT session controls.

---

## 📞 System Architecture

```
                    +------------------------------------+
                    |        React Single Page App       |
                    |           (Vite + Tailwind)        |
                    +-----------------+------------------+
                                      |
                                      | HTTP Requests (JWT Bearer)
                                      v
                    +-----------------+------------------+
                    |             FastAPI                |
                    |         (Uvicorn Runner)           |
                    +--------+------------------+--------+
                             |                  |
             SQLAlchemy ORM  |                  | SQLAlchemy ORM
                             v                  v
                    +--------+-------+  +-------+--------+
                    | SQLite Database|  |   PostgreSQL   |
                    | (Local Dev Env)|  | (Prod Docker)  |
                    +----------------+  +----------------+
```

---

## ⚡ Key Features

1. **Secure JWT Authentication:** User registration, login, and token validation with automatic session sign-out upon expiration.
2. **Server-Side Paginated Contacts:** Paginated listings supporting full-text search and category filter parameters to keep listing queries lightweight.
3. **Contact Details & Profile Layout:** Beautiful profiles displaying categories, phone numbers, addresses, custom notes, and large profile pictures.
4. **Chronological Activity Timeline:** Automatically audits and logs event timelines for contact creations, details edits, category modifications, note updates, image uploads, and favorite status toggles.
5. **Interactive Dashboard Visualizations:** High-quality stats cards showing metrics, a donut chart illustrating category splits, and a growth chart showing monthly contact additions.
6. **Smart CSV Exports:** Allows download of filtered contact lists matching active search terms or categories.
7. **Multipart File Uploads:** Supports drag-and-drop file inputs, file size validation (max 2MB), format filtering, and disk cleanup upon image replacement/removal.
8. **Dual-Mode Database Fallback:** Connects to PostgreSQL in production and automatically falls back to SQLite for local development out of the box.

---

## 🛠️ Technology Stack

* **Frontend:** React, Vite, Tailwind CSS, React Router, Axios, Recharts, Lucide Icons, React Hot Toast.
* **Backend:** FastAPI, SQLAlchemy, Alembic, Pydantic V2, Uvicorn, Python-Jose (JWT), Bcrypt (Password Hashing).
* **Orchestration:** Docker, Docker Compose, Nginx.

---

## ⚙️ Environment Variables

### Backend (`/backend/.env`)
Create a `.env` file in the `backend/` directory:
```env
# Database connection string (Leave blank to use SQLite database contacts.db automatically)
DATABASE_URL=postgresql://postgres:securepassword123@db:5432/contact_manager

# Secret key used to encrypt JWT signatures
JWT_SECRET=8f5b4d7c81d89d6e873bcf4402a5c4e951234567890abcdef1234567890abcde
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=1440

PORT=8000
HOST=0.0.0.0
UPLOAD_DIR=uploads
```

### Frontend (`/frontend/.env`)
Create a `.env` file in the `frontend/` directory:
```env
VITE_API_URL=http://127.0.0.1:5000
```

---

## 🐳 Docker Deployment Setup (Recommended)

To launch the complete application stack (Frontend, Backend, PostgreSQL database) in isolated containers:

1. **Build and Run Containers:**
   ```bash
   docker-compose up --build
   ```
2. **Access the Application:**
   * Frontend Web UI: [http://localhost](http://localhost) (Port 80)
   * Backend OpenAPI Docs: [http://localhost:8000/docs](http://localhost:8000/docs) (Port 8000)

---

## 💻 Local Setup (Without Docker)

### 1. Backend Setup
1. **Navigate to backend and create environment:**
   ```bash
   cd backend
   python -m venv venv
   ```
2. **Activate Virtual Environment:**
   * *Windows:* `.\venv\Scripts\activate`
   * *Mac/Linux:* `source venv/bin/activate`
3. **Install Requirements:**
   ```bash
   pip install -r requirements.txt
   ```
4. **Run Migrations (Initializes SQLite database automatically):**
   ```bash
   alembic upgrade head
   ```
5. **Start FastAPI Application Server:**
   ```bash
   uvicorn app.main:app --reload --port 8000
   ```

### 2. Frontend Setup
1. **Navigate to frontend and install modules:**
   ```bash
   cd frontend
   npm install
   ```
2. **Start Vite Development Server:**
   ```bash
   npm run dev
   ```
3. **Open Browser:**
   * Go to [http://localhost:5173](http://localhost:5173)

---

## 📌 REST API Endpoints

### 🔐 Authentication Router
* `POST /api/auth/register` - Create a new user account.
* `POST /api/auth/login` - Verify password and return a JWT access token.

### 📞 Contacts Router
* `GET /api/contacts` - Retrieve paginated and filtered contact lists.
* `GET /api/contacts/{id}` - Get details for a specific contact.
* `GET /api/contacts/{id}/activities` - Get chronological activity logs.
* `GET /api/contacts/activities` - Get the last 10 activities across all user contacts.
* `GET /api/contacts/stats` - Retrieve totals and chart aggregation data.
* `GET /api/contacts/export` - Export filtered contacts as a CSV download.
* `POST /api/contacts` - Create a contact (supports multipart form-data image uploads).
* `PUT /api/contacts/{id}` - Update a contact (supports multipart updates).
* `PATCH /api/contacts/{id}/favorite` - Toggle favorite boolean status.
* `DELETE /api/contacts/{id}` - Delete a contact and clear associated files.

---

## 🎨 Screenshots & Design System

The layout features:
* **Glassmorphic panels** with smooth backdrop-blurs.
* **Modern dark/light theme switching** that is fully reactive.
* **Micro-interactions** and Vercel-like hover frames.
