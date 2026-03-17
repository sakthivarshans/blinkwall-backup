<div align="center">

<br/>

# 🧱 BlinkWall

### *A private, real-time notice board built exclusively for Karunya University.*

<br/>

[![Live Demo](https://img.shields.io/badge/🚀_Live_Demo-blinkwall.vercel.app-6C63FF?style=for-the-badge)](https://blinkwall.vercel.app)
[![React](https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react)](https://reactjs.org/)
[![Node.js](https://img.shields.io/badge/Node.js-Express-339933?style=for-the-badge&logo=node.js)](https://nodejs.org/)
[![MongoDB](https://img.shields.io/badge/MongoDB-Mongoose-47A248?style=for-the-badge&logo=mongodb)](https://mongodb.com/)
[![Google OAuth](https://img.shields.io/badge/Auth-Google_OAuth_2.0-4285F4?style=for-the-badge&logo=google)](https://developers.google.com/identity)

<br/>

> **BlinkWall** is a campus-exclusive anonymous notice board where **Karunya University** students and faculty can post, browse, and manage short notes — organized into curated feeds like *For You*, *Featured*, and *Events*. Only verified university accounts (`@karunya.edu.in` / `@karunya.edu`) can access the wall.

<br/>

</div>

---

## 📖 Table of Contents

- [✨ How It Works](#-how-it-works)
- [🚀 Features](#-features)
- [🏗️ Architecture](#️-architecture)
- [🛠️ Tech Stack](#️-tech-stack)
- [⚙️ Prerequisites](#️-prerequisites)
- [📦 Setup & Installation](#-setup--installation)
  - [1 · Clone & Navigate](#1--clone--navigate)
  - [2 · Google OAuth Setup](#2--google-oauth-setup)
  - [3 · MongoDB Setup](#3--mongodb-setup)
  - [4 · Backend Configuration](#4--backend-configuration)
  - [5 · Frontend Configuration](#5--frontend-configuration)
- [▶️ Running Locally](#️-running-locally)
- [🔐 Environment Variables Reference](#-environment-variables-reference)
- [📡 API Reference](#-api-reference)
- [📁 Project Structure](#-project-structure)
- [☁️ Deployment](#️-deployment)
- [🤝 Contributing](#-contributing)

---

## ✨ How It Works

BlinkWall follows a clean **3-step user journey**:

```
1. LOGIN         2. COMPLETE PROFILE         3. POST & BROWSE
─────────        ──────────────────          ────────────────
Google Sign-In   Enter name, nickname,       Browse notes by category.
(restricted to   year (1–4), department      Post your own in ≤15 words.
university       → Stored in MongoDB         Delete your own notes anytime.
emails only)     → Unlocks the wall
```

**Under the hood**, the flow looks like this:

```
Browser → React SPA → Axios (with cookies)
                           ↓
                    Express API (port 5000)
                           ↓
                    Passport.js session check
                           ↓
              ┌────────────┴────────────┐
           MongoDB                Google OAuth 2.0
        (Users, Notes)          (Identity provider)
```

On every page load, React calls `GET /api/auth/me`. Based on the response, it routes the user to one of three states:
- 🔒 **Not logged in** → `/login`
- ⚠️ **Logged in, profile incomplete** → `/profile` (forced)
- ✅ **Logged in, profile complete** → `/` (the wall)

---

## 🚀 Features

| Feature | Details |
|---|---|
| 🔐 **University-only Access** | Login is restricted to `@karunya.edu.in` (students) and `@karunya.edu` (faculty) Google accounts |
| 👤 **Profile Onboarding** | First-time users must complete a profile: display name, nickname, year (1–4), and department |
| 🗒️ **Note Wall** | Browse and post short anonymous-style notes tagged under three feeds |
| 📂 **Category Feeds** | **For You** (all notes), **Featured** (curated highlights), **Events** (campus happenings) |
| ✂️ **15-Word Limit** | All notes are enforced to be ≤ 15 words, keeping the wall clean and scannable |
| 🗑️ **Own-Note Deletion** | Users can delete only their own notes — author ownership is enforced on the backend |
| 🍪 **7-Day Session** | Secure, HTTP-only session cookies last 7 days so users stay logged in between visits |
| 💀 **Skeleton Loading** | Notes render skeleton placeholders while the feed loads for a smooth experience |
| 🎞️ **Animated UI** | Page transitions and component animations powered by Framer Motion |
| 📱 **Responsive Design** | Fully responsive layout built with Bootstrap 5 |

---

## 🏗️ Architecture

```
blinkwall-backup/
│
├── backend/               ← Node.js / Express REST API
│   ├── models/
│   │   ├── User.js        ← Mongoose schema: googleId, email, name, nickname, year, dept
│   │   └── Note.js        ← Mongoose schema: text, category, authorId, authorNickname
│   ├── db.js              ← MongoDB connection via Mongoose
│   ├── server.js          ← Express app: routes, Passport, session, CORS
│   └── package.json
│
└── frontend/              ← React 19 SPA (Create React App)
    └── src/
        ├── api.js          ← Centralized Axios API calls (withCredentials: true)
        ├── App.js          ← Router + global auth state + route guards
        ├── pages/
        │   ├── LoginPage.js       ← Google Sign-In button
        │   ├── ProfilePage.js     ← Onboarding form (first-time login)
        │   ├── HomePage.js        ← Main note wall with category tabs
        │   └── LoginFailurePage.js ← Shown when non-university email tries to sign in
        └── components/
            ├── Navbar.js          ← Top nav with user avatar and logout
            ├── NoteForm.js        ← Post-a-note form with word counter
            ├── NoteList.js        ← Renders list of <Note /> cards
            ├── Note.js            ← Individual note card with delete button
            ├── DeleteModal.js     ← Confirmation dialog before deletion
            └── SkeletonNote.js    ← Loading placeholder for notes
```

---

## 🛠️ Tech Stack

### Backend

| Technology | Version | Purpose |
|---|---|---|
| **Node.js** | ≥ 18 | JavaScript runtime |
| **Express** | 4.x | HTTP server framework |
| **Mongoose** | 8.x | MongoDB ODM |
| **Passport.js** | 0.6 | Authentication middleware |
| **passport-google-oauth20** | 2.x | Google OAuth 2.0 strategy |
| **express-session** | 1.x | Session management + cookie storage |
| **CORS** | 2.x | Cross-origin resource sharing |
| **dotenv** | 16.x | Environment variable loading |
| **Nodemon** | 3.x | Dev auto-restart |

### Frontend

| Technology | Version | Purpose |
|---|---|---|
| **React** | 19.x | UI library |
| **React Router DOM** | 7.x | Client-side routing |
| **Axios** | 1.x | HTTP requests (with credentials) |
| **Bootstrap** | 5.3 | CSS component framework |
| **Framer Motion** | 12.x | Animations and transitions |

### Infrastructure

| Service | Purpose |
|---|---|
| **MongoDB Atlas** | Cloud-hosted MongoDB database |
| **Google Cloud (OAuth)** | Identity & authentication provider |
| **Vercel** | Frontend deployment |
| **Render / Railway** | Backend deployment |

---

## ⚙️ Prerequisites

Before you begin, ensure you have:

- **Node.js** ≥ 18.x ([download](https://nodejs.org/))
- **npm** ≥ 9.x (bundled with Node.js)
- A **MongoDB** instance — [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) (free tier works perfectly)
- A **Google Cloud** project with OAuth 2.0 credentials set up

---

## 📦 Setup & Installation

### 1 · Clone & Navigate

```bash
git clone https://github.com/sakthivarshans/blinkwall-backup.git
cd blinkwall-backup
```

---

### 2 · Google OAuth Setup

1. Go to [Google Cloud Console](https://console.cloud.google.com/).
2. Create a new project (or select an existing one).
3. Navigate to **APIs & Services → Credentials → Create Credentials → OAuth 2.0 Client IDs**.
4. Set **Application type** to **Web application**.
5. Under **Authorized JavaScript origins**, add:
   ```
   http://localhost:5000
   https://your-backend.onrender.com
   ```
6. Under **Authorized redirect URIs**, add:
   ```
   http://localhost:5000/api/auth/google/callback
   https://your-backend.onrender.com/api/auth/google/callback
   ```
7. Save and copy your **Client ID** and **Client Secret**.

---

### 3 · MongoDB Setup

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) and create a free cluster.
2. Create a database user with read/write access.
3. Whitelist your IP (or use `0.0.0.0/0` for all access during development).
4. Click **Connect → Drivers** and copy the connection string. It will look like:
   ```
   mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/?retryWrites=true&w=majority
   ```
5. Replace `<username>` and `<password>` with your database user credentials.

---

### 4 · Backend Configuration

```bash
cd backend
npm install
```

Create a `.env` file inside `backend/`:

```env
# MongoDB Connection String (from Atlas)
DATABASE_URL=mongodb+srv://youruser:yourpassword@cluster0.xxxxx.mongodb.net/blinkwall?retryWrites=true&w=majority

# Google OAuth Credentials
GOOGLE_CLIENT_ID=your_google_client_id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=your_google_client_secret

# Session Secret (generate a long, random string)
SESSION_SECRET=some_very_long_and_random_secret_string_here

# URLs
BACKEND_URL=http://localhost:5000
FRONTEND_URL=http://localhost:3000

# Environment
NODE_ENV=development

# Port
PORT=5000
```

> 💡 **Tip for SESSION_SECRET**: Generate a secure random value using:
> ```bash
> node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"
> ```

---

### 5 · Frontend Configuration

```bash
cd ../frontend
npm install
```

Create a `.env` file inside `frontend/`:

```env
# Backend API URL
REACT_APP_API_URL=http://localhost:5000
```

---

## ▶️ Running Locally

You need **two terminals** running simultaneously:

**Terminal 1 — Backend:**
```bash
cd backend
npm run dev     # Starts with Nodemon on http://localhost:5000
# or
npm start       # Production start
```

**Terminal 2 — Frontend:**
```bash
cd frontend
npm start       # Starts React dev server on http://localhost:3000
```

Open [http://localhost:3000](http://localhost:3000) and sign in with a `@karunya.edu.in` or `@karunya.edu` Google account.

---

## 🔐 Environment Variables Reference

### Backend (`/backend/.env`)

| Variable | Required | Description |
|---|:---:|---|
| `DATABASE_URL` | ✅ | MongoDB connection URI |
| `GOOGLE_CLIENT_ID` | ✅ | Google OAuth 2.0 client ID |
| `GOOGLE_CLIENT_SECRET` | ✅ | Google OAuth 2.0 client secret |
| `SESSION_SECRET` | ✅ | Random string for signing session cookies |
| `BACKEND_URL` | ✅ | Full URL where this backend is hosted |
| `FRONTEND_URL` | ✅ | Full URL of the React frontend (controls CORS + redirects) |
| `NODE_ENV` | ✅ | `development` or `production` (affects cookie security) |
| `PORT` | ➖ | Server port, defaults to `5000` |

### Frontend (`/frontend/.env`)

| Variable | Required | Description |
|---|:---:|---|
| `REACT_APP_API_URL` | ✅ | Base URL of the backend API |

---

## 📡 API Reference

All routes are prefixed with `/api`. Session cookies are required for protected routes.

### Auth

| Method | Endpoint | Auth | Description |
|---|---|:---:|---|
| `GET` | `/api/auth/google` | ❌ | Initiates Google OAuth flow |
| `GET` | `/api/auth/google/callback` | ❌ | OAuth callback — sets session cookie |
| `GET` | `/api/auth/me` | ✅ | Returns the current authenticated user object |
| `POST` | `/api/auth/logout` | ✅ | Destroys session and clears cookie |

### Profile

| Method | Endpoint | Auth | Body | Description |
|---|---|:---:|---|---|
| `POST` | `/api/profile` | ✅ | `{ name, nickname, year, department }` | Saves profile; sets `profileCompleted: true` |

### Notes

| Method | Endpoint | Auth | Query / Body | Description |
|---|---|:---:|---|---|
| `GET` | `/api/notes` | ✅ | `?category=For You\|Featured\|Events` | Returns up to 50 notes, newest first |
| `POST` | `/api/notes` | ✅ *(profile required)* | `{ text, category }` | Creates a note (max 15 words) |
| `DELETE` | `/api/notes/:id` | ✅ | — | Deletes a note (author only) |

---

## 📁 Project Structure

```
blinkwall-backup/
│
├── backend/
│   ├── models/
│   │   ├── User.js         # { googleId, email, name, nickname, year, department, profileCompleted }
│   │   └── Note.js         # { text, category, authorId, authorNickname, timestamps }
│   ├── db.js               # Mongoose.connect() wrapper
│   ├── server.js           # Express app entry point
│   └── package.json
│
└── frontend/
    ├── public/             # Static assets
    └── src/
        ├── api.js          # All Axios API call wrappers (centralized)
        ├── App.js          # BrowserRouter + auth state + 3-tier route guard
        ├── index.js        # React DOM root render
        ├── index.css       # Global styles
        ├── App.css         # App-level styles
        │
        ├── pages/
        │   ├── LoginPage.js         # "Sign in with Google" screen
        │   ├── ProfilePage.js       # Onboarding form (name, nickname, year, dept)
        │   ├── HomePage.js          # Note wall: tabs, form, list
        │   └── LoginFailurePage.js  # Access denied screen
        │
        └── components/
            ├── Navbar.js            # Header with user info + logout
            ├── NoteForm.js          # Post form with live word counter
            ├── NoteList.js          # Maps notes → <Note /> cards
            ├── Note.js              # Single note card with trash icon
            ├── DeleteModal.js       # "Are you sure?" confirmation modal
            └── SkeletonNote.js      # Animated loading placeholder card
```

---

## ☁️ Deployment

The live deployment is available at 👉 **[blinkwall.vercel.app](https://blinkwall.vercel.app)**

### Frontend → Vercel

1. Push the `frontend/` folder content (or the whole repo) to GitHub.
2. Import the repo in [Vercel](https://vercel.com/).
3. Set **Root Directory** to `frontend`.
4. Add environment variable `REACT_APP_API_URL` pointing to your deployed backend URL.
5. Deploy!

### Backend → Render (or Railway)

1. Create a new **Web Service** on [Render](https://render.com/).
2. Set **Root Directory** to `backend`.
3. Set **Build Command**: `npm install`
4. Set **Start Command**: `npm start`
5. Add all environment variables from the backend `.env` section above.
6. Set `NODE_ENV=production` and update `FRONTEND_URL` to your Vercel URL.
7. Deploy!

> ⚠️ **Important for Production**: The Google OAuth callback URL in Google Cloud Console **must exactly match** `https://your-backend.onrender.com/api/auth/google/callback`. Also ensure `FRONTEND_URL` and `BACKEND_URL` use `https://`.

---

## 🤝 Contributing

All contributions are welcome! To get started:

1. **Fork** this repository
2. **Create** a feature branch: `git checkout -b feature/your-feature-name`
3. **Commit** your changes: `git commit -m 'feat: add your feature'`
4. **Push** to the branch: `git push origin feature/your-feature-name`
5. **Open** a Pull Request

Please follow [Conventional Commits](https://www.conventionalcommits.org/) for commit messages.

---

<div align="center">

*Built for Karunya. Blink it. Post it. Move on.* ⚡

**[🌐 Visit Live App →](https://blinkwall.vercel.app)**

</div>
