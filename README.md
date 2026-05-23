# 🏆 Case — AI Legal Research Assistant
### ConHacks 2026 · Snowflake Track Winner

Case is an AI-powered legal research assistant for Canadian refugee law. It delivers citation-grounded answers to complex legal questions by retrieving from a curated database of 3,000+ tribunal decisions and 70+ statutes — reducing research time from hours to seconds.

---

## Demo

[![Case Demo](https://img.youtube.com/vi/OXfa7wEoPg0/maxresdefault.jpg)](https://youtu.be/OXfa7wEoPg0)

---

## How It Works

Case uses a routed RAG (Retrieval-Augmented Generation) pipeline:

1. **Query rewriting** — user questions are rewritten and classified for intent using Gemini
2. **Semantic retrieval** — top-k results pulled from Snowflake Cortex Search across tribunal decisions and statutes
3. **Grounded generation** — Gemini generates answers with citation-only outputs to minimize hallucination
4. **Citation cards** — every response surfaces the source cases and statutes it drew from

---

## Tech Stack

![TypeScript](https://img.shields.io/badge/TypeScript-%233178C6.svg?style=for-the-badge&logo=typescript&logoColor=white)
![Python](https://img.shields.io/badge/Python-%2314354C.svg?style=for-the-badge&logo=python&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-%23000000.svg?style=for-the-badge&logo=next.js&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-%23009688.svg?style=for-the-badge&logo=fastapi&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-%231C3C3C.svg?style=for-the-badge&logo=langchain&logoColor=white)
![Snowflake](https://img.shields.io/badge/Snowflake-%2329B5E8.svg?style=for-the-badge&logo=snowflake&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind-%2306B6D4.svg?style=for-the-badge&logo=tailwindcss&logoColor=white)

---

## Project Structure

```
backend/
  main.py          # FastAPI app and /api/chat endpoint
  services/rag.py  # LangChain + Snowflake retrieval service
  schemas/chat.py  # Request/response models
frontend/          # Next.js chat UI with citation cards
```

---

## Setup

### Environment Variables

Copy `.env.example` to `.env` and fill in:

```
GEMINI_API_KEY
SNOWFLAKE_ACCOUNT
SNOWFLAKE_USER
SNOWFLAKE_PASSWORD
SNOWFLAKE_CORTEX_SEARCH_SERVICE
```

Optional but recommended:
```
SNOWFLAKE_WAREHOUSE
SNOWFLAKE_DATABASE
SNOWFLAKE_SCHEMA
SNOWFLAKE_ROLE
SNOWFLAKE_CORTEX_CONTENT_FIELD
SNOWFLAKE_CORTEX_CASE_NAME_FIELD
SNOWFLAKE_CORTEX_SOURCE_URL_FIELD
RAG_TOP_K
NEXT_PUBLIC_API_BASE_URL
```

### Backend

```bash
python -m venv .venv
# Activate venv, then:
pip install -r requirements.txt
uvicorn backend.main:app --reload --host 127.0.0.1 --port 8000
```

### Frontend

```bash
cd frontend
npm install
npm run dev
```

Open `http://localhost:3000`.

---

## API

`POST /api/chat`

**Request:**
```json
{
  "query": "What is the legal test for X?"
}
```

**Response:**
```json
{
  "answer": "string",
  "citations": [
    {
      "case_name": "string",
      "url": "string",
      "relevance_score": 0.92
    }
  ]
}
```