# Next — AI-Powered Movie Discovery Platform

Next is a full-stack movie discovery and recommendation platform.  
It combines modern web technologies, recommendation systems, and large language models to deliver personalized movie recommendations, conversational search, and intelligent movie discovery experiences.

<img width="1918" height="909" alt="Image" src="https://github.com/user-attachments/assets/3b8a96b3-f222-45ac-90d6-a586183ccc30" />

The project is organized as a monorepo containing two submodules:

- `frontend/` — React + Vite client application
- `backend/` — Flask API, recommendation engine, and AI services

---

## Features

### Personalized Recommendations
- Behavior Sequence Transformer (BST-style) recommender trained on user rating history
- SVD collaborative filtering recommender
- Background recommendation computation with APScheduler
- Personalized recommendation feeds stored per user

<img width="1532" height="636" alt="image" src="https://github.com/user-attachments/assets/432e515a-239d-4349-bc52-423d8d4e0449" />

### AI Movie Finder
Describe a movie in natural language:

> “space movie with a black hole”

The backend uses an LLM to extract candidate movies and validates them against TMDB metadata.

<img width="568" height="415" alt="Image" src="https://github.com/user-attachments/assets/49e720e5-bc94-4fb0-98d5-01da226767bc" />

### Conversational Chatbot
An AI-powered movie chatbot capable of:
- Random movie recommendations
- Genre-based recommendations
- Director-based recommendations
- Actor-based recommendations
- Similar-movie recommendations
- Context-aware multi-turn conversations

<img width="237" height="412" alt="Image" src="https://github.com/user-attachments/assets/1dad73ba-fc8b-4bae-a973-9313d2222630" />

### Movie Discovery
- Trending, popular, and top-rated movies
- TV show browsing
- Movie details pages
- Cast and trailers
- Similar and recommended titles
- Full-text search

### User Features
- Firebase authentication
- Email verification and password reset
- Watchlists
- Star ratings
- Activity tracking

---

## Project Architecture

| Layer | Technology |
|---|---|
| Frontend | React 18, Vite, Redux Toolkit, MUI |
| Backend | Flask 3.x, Python 3.11 |
| Database/Auth | Firebase Auth + Firestore |
| Recommendation Models | TensorFlow/Keras BST + SVD |
| AI Services | Groq LLM API |
| Movie Data | TMDB API |

---

## Repository Structure

```text
./
│
├── frontend/     # React + Vite frontend
├── backend/      # Flask backend + AI services
└── README.md
```

---

# Frontend

The frontend is a React-based movie discovery interface that integrates both TMDB APIs and the Flask backend.

## Frontend Tech Stack

- React 18
- Vite 4
- Redux Toolkit
- React Router v6
- MUI v5 + Emotion
- SCSS
- Axios

## Frontend Features

- Trending / popular / top-rated browsing
- Movie and TV search
- Detailed movie pages
- Watchlists and ratings
- Personalized recommendations
- AI movie finder
- Floating AI chatbot

## Frontend Setup

### Prerequisites

- Node.js 18+
- Backend server running locally

### Installation

```bash
cd frontend

npm install
```

### Environment Variables

Create a `.env` file:

```env
VITE_APP_TMDB_TOKEN=<your-tmdb-v4-read-access-token>
VITE_FLASK_API_URL=http://127.0.0.1:5000
```

### Run Development Server

```bash
npm run dev
```

Frontend runs on:

```text
http://127.0.0.1:5173
```

---

# Backend

The backend powers authentication, recommendation models, AI integrations, and movie-related APIs.

## Backend Architecture

| Module | Responsibility |
|---|---|
| `user/` | Authentication, ratings, watchlists, history |
| `movie_finder/` | Natural-language movie search |
| `movie_chatbot/` | Conversational movie recommendations |
| `movie_recommender/` | BST + collaborative filtering |
| `core/` | Shared services and utilities |
| `config/` | Typed configuration system |

## Backend Tech Stack

- Python 3.11
- Flask 3.x
- TensorFlow 2.15 / Keras 2.15
- Firebase Admin SDK
- Groq SDK
- TMDB API
- APScheduler

---

## Backend Setup

### Prerequisites

- Python 3.11
- Firebase project
- TMDB API key
- Groq API key
- SMTP email account

### Installation

```bash
cd backend

python -m venv venv
```

Activate virtual environment:

```bash
# Windows
venv\Scripts\activate

# Linux / macOS
source venv/bin/activate
```

Install dependencies:

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

---

## Backend Configuration

Create the following files inside `backend/config/`.

### `FlaskAppConfig.json`

```json
{
  "SECRET_KEY": "<secret>",
  "SECURITY_PASSWORD_SALT": "<salt>",
  "JWT_BLACKLIST_TOKEN_CHECKS": ["access", "refresh"],
  "JWT_BLACKLIST_ENABLED": true,
  "MAIL_SERVER": "smtp.gmail.com",
  "MAIL_PORT": 465,
  "MAIL_USE_SSL": true,
  "MAIL_USE_TLS": false,
  "MAIL_USERNAME": "<email>",
  "MAIL_PASSWORD": "<password>"
}
```

### `FirebaseConfig.json`

```json
{
  "apiKey": "...",
  "authDomain": "<project>.firebaseapp.com",
  "databaseURL": "https://<project>.firebaseio.com",
  "storageBucket": "<project>.appspot.com"
}
```

### `GroqConfig.json`

```json
{
  "api_key": "<groq-api-key>",
  "model": "llama-3.3-70b-versatile"
}
```

### `TMDBConfig.json`

```json
{
  "api_key": "<tmdb-api-key>",
  "language": "en"
}
```

### `key.json`

Firebase service-account private key downloaded from Firebase Console.

---

## Run Backend

```bash
python app.py
```

Backend runs on:

```text
http://127.0.0.1:5000
```

---

# Recommendation System

The recommendation engine combines two approaches:

## 1. Behavior Sequence Transformer (BST)

A Transformer-based recommendation model trained on user interaction sequences.

Features:
- Sequential recommendation
- Learns user behavior patterns
- Personalized predictions

## 2. Collaborative Filtering (SVD)

Matrix-factorization collaborative filtering using user ratings.

Features:
- Learns latent user/movie embeddings
- Captures collaborative preferences
- Complements sequence modeling

Recommendations are periodically recomputed using APScheduler and stored in Firestore.

---

# AI Components

## Movie Finder

Uses a Groq-hosted LLM to:
1. Parse natural-language descriptions
2. Generate candidate movie titles
3. Validate matches against TMDB

## Conversational Chatbot

The chatbot:
- Detects recommendation intent
- Extracts filters
- Maintains conversational memory
- Returns contextual recommendations

Supported intents:
- Random recommendations
- By genre
- By director
- By actor
- Similar to another movie

---

# API Overview

## Authentication

```text
POST /auth/signup
POST /auth/login
POST /auth/password_reset
```

## Recommendations

```text
GET /movie_recommender/
GET /movie_recommender/content-based
GET /movie_recommender/cf
```

## AI Features

```text
POST /movie_finder/
POST /chatbot/
```

## User Actions

```text
POST /ratings
POST /watchlist
GET /watchlist
POST /activity
```

---

# License

This project is licensed under the MIT License.  
