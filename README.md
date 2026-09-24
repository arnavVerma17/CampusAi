# 🎓 CampusAI — AI-Powered University Assistant

CampusAI is a full-stack AI-powered university assistant designed to give students a single place to access campus information such as **timetables, exams, scholarships, campus resources, and university-related information**.

The system uses **Retrieval-Augmented Generation (RAG)** to retrieve relevant, permission-scoped campus data before generating an answer with an LLM. This reduces hallucinations and prevents students from accessing information that belongs to other students or sections.

---

## 🚀 Features

* 🔐 **JWT Authentication**

  * Secure student login
  * Password hashing
  * Token-based authentication

* 🤖 **AI Campus Assistant**

  * Ask questions using natural language
  * Answers are generated using retrieved campus data
  * Prevents the AI from answering outside the available context

* 🧠 **RAG Pipeline**

  * TF-IDF based document retrieval
  * Cosine similarity search
  * Context-aware AI responses
  * Permission-scoped retrieval

* 📅 **Timetable Management**

  * Section-specific timetable information
  * Student-specific timetable access

* 📝 **Exam Information**

  * Exam schedules
  * Student-specific exam information
  * Permission-aware retrieval

* 🎓 **Scholarship Information**

* 🏫 **Campus Information**

  * Labs
  * Library
  * Campus notices
  * Other university resources

* 🗄️ **Database**

  * PostgreSQL for production-style deployment
  * SQLite support for local development
  * SQLAlchemy ORM

* 🌐 **Modern Frontend**

  * React
  * TypeScript
  * Vite
  * React Router
  * Axios

* 🐳 **Docker Support**

  * Backend container
  * Frontend container
  * PostgreSQL container
  * Docker Compose orchestration

---

## 🏗️ Architecture

```text
                   ┌──────────────────────┐
                   │      React + TS      │
                   │       Frontend       │
                   └──────────┬───────────┘
                              │
                         HTTP / JWT
                              │
                              ▼
                   ┌──────────────────────┐
                   │       FastAPI        │
                   │       Backend        │
                   └──────────┬───────────┘
                              │
              ┌───────────────┼────────────────┐
              │               │                │
              ▼               ▼                ▼
       ┌────────────┐  ┌──────────────┐  ┌──────────────┐
       │ PostgreSQL │  │ RAG Pipeline  │  │ Anthropic    │
       │ Database   │  │ TF-IDF +      │  │ API / LLM    │
       │            │  │ Cosine Search │  │              │
       └────────────┘  └──────────────┘  └──────────────┘
```

### Chat Request Flow

```text
Student
   │
   ▼
Login
   │
   ▼
JWT Authentication
   │
   ▼
Ask CampusAI a Question
   │
   ▼
Retrieve Relevant Knowledge
   │
   ▼
Apply Permission Scope
   │
   ├── Public Information
   ├── Section Information
   └── Student-Specific Information
   │
   ▼
Send Retrieved Context to LLM
   │
   ▼
Grounded AI Response
```

---

## 🧠 How the RAG System Works

CampusAI does not simply send every question directly to an AI model.

Instead, the application follows a retrieval pipeline:

### 1. Data Storage

Campus information is stored in the database, including:

* Students
* Timetables
* Exams
* Scholarships
* Library information
* Laboratories
* Notices
* Campus information

### 2. Knowledge Indexing

Database records are converted into natural-language knowledge chunks.

The chunks are indexed using **TF-IDF**.

### 3. Query Retrieval

When a student asks a question, the system calculates similarity between the question and available knowledge chunks using:

```text
TF-IDF Vectorization
        +
Cosine Similarity
```

### 4. Permission Filtering

Retrieved information is filtered according to the authenticated student's permissions.

Information can be scoped as:

```text
public
section:<section>
student:<student_id>
```

This means one student cannot retrieve another student's private information.

### 5. AI Generation

Only the retrieved and permission-cleared information is sent to the Anthropic API.

The model is instructed to answer only from the provided context.

If the required information is unavailable, CampusAI is instructed to say so rather than inventing an answer.

---

## 🛠️ Tech Stack

### Frontend

| Technology   | Purpose                        |
| ------------ | ------------------------------ |
| React        | UI development                 |
| TypeScript   | Type-safe frontend development |
| Vite         | Development/build tooling      |
| React Router | Client-side routing            |
| Axios        | API communication              |

### Backend

| Technology       | Purpose               |
| ---------------- | --------------------- |
| Python           | Backend programming   |
| FastAPI          | REST API framework    |
| SQLAlchemy       | ORM                   |
| PostgreSQL       | Database              |
| JWT              | Authentication        |
| Passlib / bcrypt | Password hashing      |
| Scikit-learn     | TF-IDF and similarity |
| Anthropic API    | LLM integration       |

### DevOps

| Technology     | Purpose                       |
| -------------- | ----------------------------- |
| Docker         | Containerization              |
| Docker Compose | Multi-container orchestration |

---

## 📁 Project Structure

```text
campusai/
│
├── backend/
│   ├── app/
│   │   ├── rag/
│   │   │   ├── indexer.py
│   │   │   ├── retriever.py
│   │   │   ├── llm.py
│   │   │   └── __init__.py
│   │   │
│   │   ├── routers/
│   │   │   ├── auth.py
│   │   │   ├── campus.py
│   │   │   ├── chat.py
│   │   │   ├── exams.py
│   │   │   ├── scholarships.py
│   │   │   └── timetable.py
│   │   │
│   │   ├── auth.py
│   │   ├── config.py
│   │   ├── database.py
│   │   ├── main.py
│   │   ├── models.py
│   │   ├── schemas.py
│   │   └── seed.py
│   │
│   ├── Dockerfile
│   ├── requirements.txt
│   └── .env.example
│
├── frontend/
│   ├── src/
│   │   ├── api/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── App.tsx
│   │   ├── main.tsx
│   │   └── styles.css
│   │
│   ├── Dockerfile
│   ├── package.json
│   └── .env.example
│
├── docker-compose.yml
├── .gitignore
└── README.md
```

---

# ⚙️ Installation & Setup

## Prerequisites

Make sure you have installed:

* Python 3.10+
* Node.js 18+
* npm
* Docker Desktop
* Git

An Anthropic API key is required for AI-powered chat.

---

# 🐳 Run Using Docker

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/campusai.git
cd campusai
```

### 2. Configure the backend

Copy the environment file:

```bash
cp backend/.env.example backend/.env
```

For Windows PowerShell:

```powershell
Copy-Item backend/.env.example backend/.env
```

Open:

```text
backend/.env
```

and configure:

```env
DATABASE_URL=postgresql://campusai:campusai@db:5432/campusai

ANTHROPIC_API_KEY=your_anthropic_api_key

ANTHROPIC_MODEL=claude-sonnet-5

JWT_SECRET=your_long_random_secret

CORS_ORIGINS=http://localhost:5173
```

### 3. Start the application

```bash
docker compose up --build
```

### 4. Seed the database

Open another terminal:

```bash
docker compose exec backend python -m app.seed
```

### 5. Open the application

Frontend:

```text
http://localhost:5173
```

Backend:

```text
http://localhost:8000
```

FastAPI Swagger documentation:

```text
http://localhost:8000/docs
```

---

# 💻 Run Without Docker

## Backend

Navigate to the backend:

```bash
cd backend
```

Create a virtual environment:

```bash
python -m venv venv
```

Activate it.

### Windows

```powershell
venv\Scripts\activate
```

### Linux/macOS

```bash
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Create your environment file:

```bash
cp .env.example .env
```

For Windows:

```powershell
Copy-Item .env.example .env
```

Seed the database:

```bash
python -m app.seed
```

Start FastAPI:

```bash
uvicorn app.main:app --reload
```

Backend will run at:

```text
http://localhost:8000
```

---

## Frontend

Open a new terminal:

```bash
cd frontend
```

Install dependencies:

```bash
npm install
```

Create the environment file:

```bash
cp .env.example .env
```

For Windows:

```powershell
Copy-Item .env.example .env
```

Start the frontend:

```bash
npm run dev
```

Frontend will run at:

```text
http://localhost:5173
```

---

# 🔑 Demo Accounts

The project includes seeded demo students for testing.

```text
Student 1
Email: arnav@sharda.ac.in
Password: password123

Student 2
Email: priya@sharda.ac.in
Password: password123
```

These accounts are intended for local development/testing only.

**Do not use these credentials in a production deployment.**

---

# 🔒 Security Model

CampusAI uses permission-scoped retrieval rather than relying only on the LLM prompt.

For example:

```text
Student A
    │
    ├── Public data
    ├── Student A's section data
    └── Student A's private data

Student B
    │
    ├── Public data
    ├── Student B's section data
    └── Student B's private data
```

A student's private records are not included in the retrieval context for another student.

This makes the permission boundary part of the application logic rather than simply asking the AI model to "not reveal" private information.

---

# 📊 Current Implementation

### Implemented

* [x] React frontend
* [x] TypeScript
* [x] FastAPI backend
* [x] PostgreSQL integration
* [x] SQLite local-development support
* [x] SQLAlchemy ORM
* [x] JWT authentication
* [x] Password hashing
* [x] Timetable API
* [x] Exam API
* [x] Scholarship API
* [x] Campus information API
* [x] AI chat API
* [x] TF-IDF retrieval
* [x] Cosine similarity
* [x] Permission-scoped RAG
* [x] Anthropic API integration
* [x] Docker support
* [x] Database seeding
* [x] FastAPI Swagger documentation

---

# 🚧 Future Improvements

The current project is a working prototype. The following improvements can make it production-ready:

### 🔹 Real University Data

Connect the application to actual:

* University ERP
* LMS
* Examination system
* Library system
* Scholarship database
* Timetable system

Currently, the project uses seeded demo data.

### 🔹 Semantic Search

Replace TF-IDF retrieval with an embedding-based approach such as:

```text
Sentence Transformers
        or
Embedding API
```

This would improve retrieval for queries using synonyms and natural language variations.

### 🔹 Real-Time Campus Information

Integrate:

* Lab occupancy sensors
* Library availability
* Campus events
* Live notices

### 🔹 Additional User Roles

Add role-based access for:

```text
Student
Faculty
Admin
```

### 🔹 Notifications

Future versions could support:

* Exam reminders
* Timetable changes
* Scholarship deadlines
* University announcements

### 🔹 Voice Assistant

Add speech-to-text and text-to-speech functionality for voice-based campus assistance.

---

# 🧪 Testing the Permission System

One of the important demonstrations of this project is permission-aware retrieval.

For example:

```text
Student A asks:
"What is my exam seat?"

→ Student A's exam information can be retrieved.

Student B asks:
"What is Student A's exam seat?"

→ Student A's private information is not included
   in Student B's retrieval context.
```

This demonstrates that the system's security boundary is implemented during retrieval rather than relying exclusively on the AI model.

---

# 📡 API Endpoints

The FastAPI backend exposes endpoints for:

```text
/auth
/timetable
/exams
/scholarships
/campus
/chat
/health
```

Interactive API documentation is available at:

```text
http://localhost:8000/docs
```

---

# 🎯 Project Objective

The objective of CampusAI is to demonstrate how **Generative AI + RAG + traditional university databases** can be combined into a practical campus information system.

Instead of forcing students to search across multiple fragmented systems, CampusAI provides a conversational interface that retrieves relevant information and generates grounded responses.

---

# 👨‍💻 Development

Built as a full-stack AI project using:

```text
Frontend → React + TypeScript
Backend  → FastAPI + Python
Database → PostgreSQL
AI       → RAG + Anthropic API
DevOps   → Docker
```

---

# ⚠️ Disclaimer

CampusAI is currently a prototype and uses demo/seeded university data.

It is **not connected to an actual university ERP, LMS, examination system, or student information system**.

For production deployment, authentication, database security, university integrations, monitoring, rate limiting, and data privacy controls would need additional implementation.

---

## ⭐ If You Find This Project Useful

Consider giving the repository a ⭐ on GitHub.

---

## 📄 License

This project is available for educational and development purposes.
