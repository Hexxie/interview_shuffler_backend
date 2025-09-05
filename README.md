# Interview Shuffler — Backend

Backend service for generating **randomized interviews** from a pool of questions.  
The UI (React) allows users to upload a list of questions → backend parses them one by one and stores them in **SQLite**.  
When the interviewer clicks "Generate Interview" and chooses a time slot, the backend selects a **random set of questions** (accordion format).  
Clicking a question triggers a call to the **LLM**, which returns a short **reference answer**.

---

## Vision

The goal of this project is to make **practice interviews accessible to anyone**, even when a subject-matter expert is not available.  

Imagine:  
- You want to prepare for a **Python developer** interview.  
- Your friend is a **C++ developer**, and doesn’t know Python.  
- They can still interview you — focusing on *how clearly you explain*, *whether your reasoning makes sense*, and *whether the answer feels complete*.  

The **LLM-generated reference answers** provide just enough context to guide the interviewer, regardless of their expertise.  
This makes it possible to:  
- Run **peer-to-peer mock interviews** across different domains.  
- Practice **communication and clarity**, not just correctness.  
- Lower the barrier for interview prep by not requiring an expert in every technology.  

Ultimately, Interview Shuffler helps candidates build **confidence, adaptability, and interview readiness** — even outside their immediate professional circle.

---

## Roadmap

**MVP (v0.1)**  
- [ ] `Question` model in SQLite  
- [ ] Bulk import (parsed line by line)  
- [ ] Generate interview session: random questions based on `duration_min`  
- [ ] Endpoint for LLM-generated short answer  

**v0.2**  
- [ ] Filters (tags, difficulty, "not recently used")  
- [ ] Session log: asked vs skipped questions  
- [ ] Accordion rendering in React  

**v0.3**  
- [ ] Auth / roles (token-based)  
- [ ] Import / export (CSV / JSON)  
- [ ] Analytics: most frequent questions  

---

## Tech Stack

- **FastAPI** — REST API (ASGI)  
- **Uvicorn[standard]** — ASGI server  
- **SQLite** — embedded DB (MVP)  
- **SQLAlchemy 2.x** — ORM / SQL Core  
- **Alembic** — DB migrations  
- **Pydantic** — validation and schemas  
- **pytest + httpx** — testing  
- **Poetry 2.x** — dependency management & env handling  

> LLM provider is configurable via `LLM_PROVIDER` and `LLM_API_KEY` (OpenAI, Anthropic, or others through a thin adapter).

---

## Getting Started

### Requirements
- Python **3.11+**  
- Poetry **2.x**

### 1) Clone & Init
```bash
git clone <repo-url>
cd interview_shuffler_backend
```

### 2) Setup Environment & Install Deps
```bash
poetry env use python3.11
poetry install
```

### 3) Environment Variables
Create a .env file:

```bash
DATABASE_URL=sqlite:///./data.db
LLM_PROVIDER=openai
LLM_API_KEY=YOUR_KEY
LLM_MODEL=gpt-4o-mini
DEFAULT_DURATION_MIN=30
```

### 4) Run Migrations (Alembic)
```bash
poetry run alembic init migrations
poetry run alembic revision --autogenerate -m "init"
poetry run alembic upgrade head
```

### 5) Start Dev Server
```bash
poetry run uvicorn app.main:app --reload --port 8000
# Swagger UI → http://127.0.0.1:8000/docs
```

### 6) Run Tests
```bash
poetry run pytest -q
```

## License
MIT