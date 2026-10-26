# Misinformation AI

**Paste a post, a message or a news snippet, and two AI agents research it on the web and return a structured verdict: what each claim says, whether the evidence supports it, and the sources behind that.**

Built by team AI Gyani.

## How it works

```
Next.js app ──POST /api/analyze──▶ FastAPI ──▶ CrewAI crew (Gemini 2.5 Flash)
                                                  1. Researcher: searches the web (Serper) for each claim
                                                  2. Analyst: weighs the evidence and writes the verdict
                                     ◀── structured JSON, validated by Pydantic
```

Every response follows one schema (`backend/app/models/schemas.py`):

| Field | Meaning |
| --- | --- |
| `overall_verdict` | One of `Accurate`, `Misleading`, `False`, `Unsubstantiated`, `Contested` |
| `confidence_score` | 0.0 to 1.0 |
| `claims[]` | Each extracted claim with its own verdict, an evidence summary and source links |
| `analysis_summary`, `reasoning` | A short summary, and the step-by-step reasoning behind the verdict |

Because the output is a typed object rather than free text, the frontend can render a verdict card per claim, and a wrong shape fails loudly instead of showing up as a garbled answer.

## Run it

**Backend** (Python 3.11+):

```bash
cd backend
pip install -r requirements.txt
# .env needs GEMINI_API_KEY (for Gemini through CrewAI) and SERPER_API_KEY (web search)
uvicorn app.main:app --reload            # http://127.0.0.1:8000, docs at /docs
```

The pinned `requirements.txt` was exported on Windows and includes `pywin32`; on macOS or Linux, remove that line first.

**Frontend** (Node 18+):

```bash
cd frontend
npm install
npm run dev                              # http://localhost:3000
```

The frontend calls `http://127.0.0.1:8000` by default; set `NEXT_PUBLIC_API_URL` to point it elsewhere.

## Stack

FastAPI, Pydantic, CrewAI, Gemini 2.5 Flash, Serper, Next.js 14, TypeScript, Tailwind CSS, React Query.
