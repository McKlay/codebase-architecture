---
hide:
    - toc
---

## Part 2: Modularity

# 9. Modular Folder Structure for FastAPI

> “FastAPI is flexible enough to let you do anything.
> But structure tells you what you *should* do.”

---

### Why Modularity Matters More in Backend

In a React frontend, the biggest risks of poor structure are visual bugs, state bleed, or duplicated logic.

In a FastAPI backend, bad structure means:

* Route spaghetti
* Logic leakage
* Circular imports
* Untestable services
* Monolithic pain

Modular folder design in FastAPI creates **cleanly separated APIs**, **testable service layers**, and **an architecture that evolves with your app.**

This chapter walks you through how to build modular backends that scale—with clear folders, sharp imports, and minimal surprise.

---

### What “Feature-Centric” Means in FastAPI

Just like the frontend, modular FastAPI apps group logic **by feature**, not just by file type.

Instead of:

```
app/
├── api/
├── services/
├── schemas/
```

You build:

```
app/
├── chatbot/
│   ├── api/
│   ├── services/
│   ├── schemas/
├── invoice/
│   ├── api/
│   ├── gpt_parser.py
│   └── schemas/
├── shared/
│   ├── ocr/
│   ├── gpt/
│   └── db/
```

Each **feature module** owns its API, logic, and schemas.

You gain:

* Isolation for debugging
* Parallel development
* Feature-based ownership

---

### Example Modular FastAPI Folder Layout

```
app/
├── chatbot/
│   ├── api/               # Routes (chatbot endpoints)
│   ├── services/          # Business logic (streaming, tokenization)
│   ├── schemas/           # Request/response models
│   ├── utils/             # Optional: local GPT wrappers, guards
│   └── __tests__/         # Feature-scoped unit tests
├── invoice/
│   ├── api/
│   ├── services/
│   ├── schemas/
│   └── __tests__/
├── shared/
│   ├── gpt/               # GPT/OpenAI clients
│   ├── ocr/               # pytesseract, EasyOCR, image preprocessing
│   ├── db/                # DB engine, models, repositories
│   ├── auth/              # JWT validation, role guard utils
│   └── test_helpers/      # Mock clients, test fixtures
├── core/                  # Global settings, constants, DI container
├── main.py                # FastAPI app entry point
└── routers.py             # Route inclusion and router setup
```

---

### How to Organize Routes, Schemas, and Services

#### Routes → `/api/`

Every feature should define its own routes in a dedicated file (or files) under `api/`.

```python
# chatbot/api/chat_routes.py
from fastapi import APIRouter
from ..services.chat_service import stream_response

router = APIRouter()

@router.post("/chat")
async def chat_handler(payload: ChatRequest):
    return await stream_response(payload)
```

Then in `routers.py` (or `main.py`):

```python
from chatbot.api import chat_routes
from invoice.api import invoice_routes

app.include_router(chat_routes.router, prefix="/chatbot")
app.include_router(invoice_routes.router, prefix="/invoice")
```

This modularizes routing logic **without polluting `main.py`**.

---

#### Schemas → `/schemas/`

Pydantic models for request/response validation should be:

* Inside each feature
* Named clearly (e.g., `ChatRequest`, `InvoiceUploadRequest`)
* Not shared unless 100% generic

Keep your schemas *tight and feature-specific*—don’t overgeneralize prematurely.

---

#### Services → `/services/`

This is your **business logic** layer.
It should:

* Be importable in tests
* Never access `Request`/`Response` objects directly
* Contain all GPT, OCR, or parsing logic per feature

Example:

```python
# invoice/services/gpt_parser.py
def extract_invoice_data(text: str) -> dict:
    prompt = f"Extract fields from: {text}"
    return call_gpt(prompt)
```

Avoid mixing API and service logic. Your services should feel *pure*.

---

### Keeping Shared Logic Clean

Not everything belongs to a feature. Common patterns include:

| Shared Concern                | Location               |
| ----------------------------- | ---------------------- |
| GPT clients & config          | `shared/gpt/`          |
| OCR utilities                 | `shared/ocr/`          |
| Database session, base models | `shared/db/`           |
| Auth utilities                | `shared/auth/`         |
| Test fixtures                 | `shared/test_helpers/` |

If **two or more modules use it**, pull it out of the feature folder.
Use `shared/` to avoid duplication and prevent coupling.

---

### Handling Circular Imports

Circular imports are a common pain in FastAPI modular design.

#### Problem:

`chatbot/services/gpt.py` imports `shared/gpt/client.py`
but `client.py` also imports schema from `chatbot/schemas/`

#### Solution:

* **Never import feature-local files inside `shared/`**
* Move common schemas to `shared/schemas/` *only if truly generic*
* Use **Dependency Injection** to decouple layers

---

### Structuring Testable Services

Your `services/` should be:

* Importable without side effects
* Independent from actual external APIs (thanks to wrappers/mocks)
* Tested inside `__tests__/` per feature

```python
# chatbot/__tests__/test_streaming.py

from chatbot.services.chat_service import stream_response

def test_stream_yields_valid_chunks():
    payload = ChatRequest(message="Hi")
    chunks = list(stream_response(payload))
    assert len(chunks) > 0
```

Use `shared/test_helpers/` to store:

* Mock GPT/OpenAI clients
* Mock DB responses
* Test images for OCR

---

### Dependency Injection and Inversion

FastAPI’s `Depends` is your secret weapon for modular testable code.

Example:

```python
def get_vector_db():
    return SupabaseVectorStore()

@router.post("/embed")
def embed_doc(input: DocInput, db = Depends(get_vector_db)):
    return db.add(input)
```

In tests:

```python
def test_embed(monkeypatch):
    monkeypatch.setattr("shared.vectorstore.get_vector_db", lambda: MockVectorDB())
```

This keeps services decoupled and **mockable**, a key modularity goal.

---

### Summary: Modular FastAPI Guidelines

| Principle                       | Practice                                          |
| ------------------------------- | ------------------------------------------------- |
| Feature-based foldering         | Each feature gets `api/`, `services/`, `schemas/` |
| Keep routes thin                | All logic moves to services                       |
| Local schemas                   | Use per-feature Pydantic models                   |
| Shared logic lives in `shared/` | Only extract when multiple features need it       |
| Avoid circular imports          | Never import upwards; use DI patterns             |
| Local tests per module          | Use `__tests__/` to scope and scale testing       |

---

