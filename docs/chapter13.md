---
hide:
    - toc
---

## Part 3: Scalability

# 13. Scalable Folder Structure for FastAPI

> “When APIs grow, folder structure becomes your defense against entropy.”

---

### Why Scalability Hits the Backend First

React can get away with a little mess. FastAPI cannot.

As your backend evolves to support:

* Auth and permission layers
* Vector DBs, async GPT pipelines, OCR parsing
* Multiple API versions
* Streaming, WebSockets, background jobs

…your old `api/`, `services/`, `schemas/` layout starts to strain.

You need a **layered backend architecture** that:

* Supports independent deployments
* Reduces coupling between services
* Enables async, task-driven workflows
* Maintains testability and refactor-safety

---

### Recommended Scalable FastAPI Folder Layout

```bash
app/
├── api/                # Route definitions (per version, grouped by domain)
│   ├── v1/
│   │   ├── chatbot.py
│   │   ├── invoice.py
│   │   └── upload.py
├── services/           # Business logic (GPT, processing, workflows)
│   ├── gpt_service.py
│   ├── invoice_parser.py
│   └── embedding_service.py
├── schemas/            # Pydantic models for API input/output
│   ├── chatbot.py
│   ├── invoice.py
│   └── shared.py
├── core/               # App settings, middleware, auth guards
│   ├── config.py
│   ├── security.py
│   └── exceptions.py
├── infra/              # External systems (DB, VectorDB, S3, Celery)
│   ├── database.py
│   ├── supabase.py
│   ├── weaviate.py
│   └── celery_worker.py
├── tasks/              # Background task definitions
│   └── process_invoice.py
├── shared/             # Utilities used across layers
│   ├── ocr.py
│   ├── image_utils.py
│   └── logging.py
├── main.py             # FastAPI app entry point
└── routers.py          # Route inclusion logic
```

This structure mirrors **clean architecture** and **DDD layering**, while staying idiomatic to FastAPI.

---

### Key Layers and Responsibilities

| Layer       | Purpose                                                 |
| ----------- | ------------------------------------------------------- |
| `api/`      | Route definitions, versioning, and decorators           |
| `services/` | Business logic, orchestration, pipeline steps           |
| `schemas/`  | Data contracts between client and backend               |
| `core/`     | Security, config, middleware, exception handling        |
| `infra/`    | External resources like DBs, VectorStores, file storage |
| `tasks/`    | Async background jobs (Celery, BackgroundTasks)         |
| `shared/`   | Utility functions reused across modules                 |
| `tests/`    | May live locally or in root folder, depending on scale  |

---

### Clean Separation of Router, Service, and Schema

FastAPI makes it easy to blur lines—resist the temptation.

**Bad Pattern**:

```python
@app.post("/chat")
def chat_handler(input: str = Body(...)):
    response = openai.ChatCompletion.create(...)
    return response
```

**Scalable Pattern**:

```python
# api/v1/chatbot.py
@router.post("/chat", response_model=ChatResponse)
def handle_chat(input: ChatInput, svc: GPTService = Depends()):
    return svc.generate_response(input)

# services/gpt_service.py
class GPTService:
    def generate_response(self, input: ChatInput) -> ChatResponse:
        ...
```

Why?

* Routes should be *thin*
* Services should be *testable*
* Schemas should be *reusable*

---

### Versioned Routing and Scalable Endpoints

Structure your `api/` like this:

```bash
api/
├── v1/
│   ├── chatbot.py
│   ├── invoice.py
│   └── upload.py
├── v2/
│   └── chatbot.py
```

In `main.py` or `routers.py`:

```python
app.include_router(v1.chatbot.router, prefix="/v1/chatbot")
app.include_router(v2.chatbot.router, prefix="/v2/chatbot")
```

Benefits:

* Non-breaking upgrades
* Parallel development of new features
* Easier deprecation and API sunset cycles

---

### Supporting Background Jobs

Background processing becomes essential at scale:

* GPT calls that exceed request timeouts
* OCR-heavy image parsing
* Long-running embedding workflows

Structure:

```bash
tasks/
├── embed_document.py
├── process_invoice.py
└── reindex_vector_db.py
```

Use:

* `BackgroundTasks` (FastAPI native, simple cases)
* `Celery` + Redis (robust task queue)
* `RQ`, `dramatiq`, or `Huey` (lightweight alternatives)

Keep `tasks/` **decoupled** from `api/`—triggered by `services/`.

---

### Auth, Permissions, and Role-Based Modules

Place guards and JWT logic in:

```bash
core/
├── security.py       # Auth check functions, token decode/validate
├── permissions.py    # Role-based access checks
└── dependencies.py   # Dependency injection helpers
```

In route:

```python
@router.post("/secure-action", dependencies=[Depends(require_admin)])
def admin_only_action():
    ...
```

* Clean
* Declarative
* Testable

---

### Repository Pattern for Data Access

To decouple DB from services:

```
infra/
├── database.py           # Engine + session
├── repositories/
│   ├── user_repo.py
│   └── invoice_repo.py
```

Services call repositories, not `Session` or raw SQL:

```python
class InvoiceService:
    def __init__(self, repo: InvoiceRepository):
        self.repo = repo

    def create_invoice(...):
        return self.repo.insert_invoice(...)
```

This enables:

* Swappable DBs (e.g., PostgreSQL → Supabase)
* Easier mocking in tests
* Separation of query logic from business flow

---

### Summary: FastAPI at Scale

| Concern                | Where It Lives                       |
| ---------------------- | ------------------------------------ |
| Routes                 | `api/v1/`                            |
| Business logic         | `services/`                          |
| Schema validation      | `schemas/`                           |
| Config/auth/middleware | `core/`                              |
| VectorDB, Celery, S3   | `infra/`                             |
| Async tasks            | `tasks/`                             |
| Shared utils           | `shared/`                            |
| API versioning         | `api/v1/`, `api/v2/`                 |
| Role-based access      | `core/security.py`, `permissions.py` |

This structure prepares your backend to:

* Handle growing teams and responsibilities
* Expand into multi-feature orchestration
* Scale horizontally (via services) and vertically (via workers/tasks)

In the next chapter, we’ll zoom out to deployment concerns—and how structure **directly impacts your CI/CD, Dockerization, and scaling workflows.**

---