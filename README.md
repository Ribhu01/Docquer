<div align="center">

# Docquer

RAG‑powered AI chat with streaming answers, citations, and web/file sources.

[![Made with FastAPI](https://img.shields.io/badge/Made%20with-FastAPI-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=black)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![ChromaDB](https://img.shields.io/badge/Vector%20DB-ChromaDB-8A2BE2)](https://www.trychroma.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

</div>

---

> Ask questions about your PDFs, DOCX, TXT, and web links. Answers stream in like ChatGPT and include expandable citations with jump‑to‑source.
> When no documents are active, Docquer acts as a normal general‑purpose chatbot.


## Table of Contents
- [Highlights](#highlights)
- [Live Demo](#live-demo)
- [Screenshots](#screenshots)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
- [How It Works](#how-it-works)
- [Deploy](#deploy)
- [Roadmap](#roadmap)
- [Tech Stack](#tech-stack)
- [License](#license)


## Highlights
- File + Web ingestion (PDF/DOCX/TXT and readable HTML)
- Retrieval‑Augmented Generation (ChromaDB + Sentence Transformers)
- Streaming responses (SSE) with natural, chat‑like pacing
- Citations under answers (expand for larger snippet, open source, file/web badges)
- Source filters (Files/Web) and one‑click “Clear context”
- Clean, responsive UI
- Works as a normal chatbot for general queries when no documents/links are selected


## Live Demo
Add your link here once deployed.

- Frontend: `https://your-frontend.example.com`
- Backend: `https://api.your-backend.example.com`


## Screenshots
> Replace with real images or a short GIF for maximum impact.

- Chat with streaming + citations
- Link ingestion UI and filters
- Clear context in Active Documents section


## Quick Start
Prerequisites: Node 18+, pnpm, Python 3.10+, virtualenv

### 1) Backend
```bash
cd backend
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt

# .env (minimal)
GROQ_API_KEY=your_groq_api_key
FRONTEND_ORIGIN=http://localhost:5173
PORT=8000

uvicorn main:app --host 0.0.0.0 --port 8000
```
Check: http://localhost:8000/health

### 2) Frontend
```bash
cd Frontend
pnpm install

# Frontend/.env
VITE_API_BASE_URL=http://localhost:8000

pnpm dev
```
Open: http://localhost:5173

Try it:
- Click paperclip to upload a file; link icon to ingest a URL
- Ask your question → streamed answer + citations
- Use Files/Web filters; click “Clear” to reset context
- Ask any general question (with no active docs) → standard chatbot answer without citations


## Configuration
Backend `.env`:
```bash
GROQ_API_KEY=your_groq_api_key
FRONTEND_ORIGIN=http://localhost:5173   # or your deployed frontend origin (no trailing slash)
PORT=8000
```
Frontend `Frontend/.env`:
```bash
VITE_API_BASE_URL=http://localhost:8000  # or your deployed backend URL
```


## How It Works
```
User → Frontend (React) → FastAPI /api/chat
                        ↘ RAG: embed + search (ChromaDB)
                           ↘ Sentence Transformers (embeddings)
→ Groq LLM (chat completion) → Streaming SSE back to browser
                              → Citations (top chunks) under answer
```
Behavior:
- If one or more documents/links are active, Docquer retrieves relevant chunks and grounds the answer (with citations)
- If no documents/links are active (or no strong matches are found), Docquer responds as a general chatbot without citations


## Deploy
1) Deploy Backend (Render/Railway/Fly)
- Start: `uvicorn main:app --host 0.0.0.0 --port $PORT`
- Env: `GROQ_API_KEY`, `FRONTEND_ORIGIN`, `PORT`
- Persist `backend/vector_db/` if you want docs to survive restarts

2) Deploy Frontend (Vercel/Netlify)
- Build: `pnpm install && pnpm build`
- Output dir: `Frontend/client/dist`
- Env: `VITE_API_BASE_URL=https://your-backend`

3) Tighten CORS
- Set backend `FRONTEND_ORIGIN=https://your-frontend` and restart




## Tech Stack
- Frontend: React 18, TypeScript, Vite, Tailwind
- Backend: FastAPI, Pydantic, Uvicorn
- RAG: Sentence Transformers, ChromaDB
- Model: Groq chat completions


---
If you found this helpful, please ⭐ the repo and share feedback. Want help deploying? Open an issue!
