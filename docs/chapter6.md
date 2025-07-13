---
hide:
    - toc
---

## Part 1: Foundations

# 6. Understanding the Stack

> “Structure isn’t built in a vacuum. It reflects your stack’s behavior.”

---

### Why Stack Awareness Matters

Your folder structure should *mirror* how your app works—not fight it.

You can’t structure a React + FastAPI app the same way you would structure a monolithic Rails app or a serverless function chain. The **communication style**, **state boundaries**, and **build process** between frontend and backend shape how your code should be organized.

This chapter gives you a bird’s-eye view of the **React + FastAPI full-stack architecture**—so you know *what* you’re structuring and *why* certain patterns emerge.

Let’s break it down.

---

### React + Vite: The Frontend

React is a **component-driven UI library**. Vite is its **next-gen build tool**, optimized for speed and modularity.

#### Key Characteristics:

* **File-based routing** (via React Router, not enforced by framework)
* **Component reusability** is encouraged (hooks, context, UI libraries)
* **Client-side rendering by default**
* **Modular JavaScript/TypeScript imports**
* **Single-page application model**, often talking to an API backend
* Optional integrations: state management (Zustand, Redux, Jotai), form libs, TailwindCSS, etc.

#### Frontend Folder Design Considerations:

* You’ll likely need folders for:

  * `components/`: shared visual components
  * `features/`: domain-based modules (e.g., chatbot, invoice)
  * `hooks/`: reusable logic
  * `api/`: backend interaction (e.g., axios or fetch wrappers)
  * `assets/`: static images, videos, icons
  * `routes/`: layout-level control (React Router v6)
  * `context/` or `store/`: global or scoped state

#### Common Patterns:

* **Feature-first foldering** is often better early on
* Shared logic moves into `shared/` or `lib/` as complexity grows
* Components are often colocated with CSS or styled using utility-first approaches (e.g., Tailwind)

---

### FastAPI: The Backend

FastAPI is a **modern Python web framework** built on Pydantic and Starlette. It’s API-first, type-hinted, and async-ready.

#### Key Characteristics:

* **Route-first design**: APIs are defined via decorators (`@router.get`, `@router.post`)
* **Request validation and serialization** via Pydantic schemas
* **ASGI-native**: supports WebSockets, streaming, background tasks
* **Auto-docs via OpenAPI**, great for teams and testing
* **Flexible architecture**: microservice-ready, but can be modular or monolithic

#### Backend Folder Design Considerations:

* Key layers include:

  * `api/`: all route logic
  * `schemas/`: request/response Pydantic models
  * `services/`: business logic
  * `core/`: config, constants, middleware
  * `infra/`: database, vector DBs, storage clients
  * `utils/` or `shared/`: generic reusable helpers
  * `tests/`: unit + integration test structure

#### Common Patterns:

* **Layer-first** is preferred for complex or team-sized projects
* **Dependency injection** (via `Depends`) supports testability
* **Router composition** (e.g., `include_router()`) allows per-module routing logic
* Can be **modularized** per feature or **layered**—or both (hybrid)

---

### How React and FastAPI Communicate

The two parts of your stack are **decoupled**—but tightly integrated.

| Communication Layer          | Tech                                      | Structure Tip                                                 |
| ---------------------------- | ----------------------------------------- | ------------------------------------------------------------- |
| **REST API**                 | JSON over HTTP                            | Use an `api/` folder on both frontend and backend             |
| **WebSockets**               | Real-time, duplex                         | Isolate WS logic in a shared `sockets/` or `events/` layer    |
| **Streaming**                | `yield` from backend → chunks in frontend | Group stream handlers and frontend consumers as a unit        |
| **File Uploads**             | `multipart/form-data`                     | Create a unified `upload/` or `documents/` module             |
| **Authentication**           | JWT, OAuth, sessions                      | Mirror `auth/` logic in both frontend and backend features    |
| **Embeddings/Vector Search** | Supabase, Pinecone, Weaviate              | Treat as an infrastructure concern in `infra/` or `services/` |

---

### Shared Challenges in a Full-Stack AI App

With React + FastAPI, especially in AI-centric use cases (e.g., OCR, GPT, chatbots), you face shared architectural challenges that should influence folder design:

#### 1. **State Synchronization**

* React stores client-side UI state, auth state, and sometimes cached data
* Backend stores ground truth (e.g., user sessions, file uploads, GPT responses)
* Solution: use `context/` or `store/` in React + strict separation of `services/` and `api/` in FastAPI

#### 2. **API Versioning**

* As your API grows, changes must be non-breaking
* Solution: versioned routers (e.g., `api/v1/chat.py`, `api/v2/chat.py`) and API clients on frontend

#### 3. **Authentication & Authorization**

* Frontend handles token storage (often via HTTP-only cookies or `localStorage`)
* Backend must validate tokens on each request
* Solution: `auth/` folders in both front and back, shared schema if monorepo

#### 4. **Model Wrapping and Latency**

* GPT calls, vector DB lookups, or OCR take time
* Use background jobs (`BackgroundTasks`, Celery) and streaming where appropriate

#### 5. **Error Handling and Logging**

* Frontend: user-friendly messages, fallback states
* Backend: structured logs, `try/except` middleware, Sentry integration

---

### Summary: Structure Reflects Stack Realities

| Layer           | React (Vite)                          | FastAPI                           |
| --------------- | ------------------------------------- | --------------------------------- |
| Presentation    | `components/`, `routes/`              | N/A (or static templates)         |
| State           | `context/`, `store/`, `hooks/`        | `Depends()`, middleware, DB       |
| API Integration | `api/`, `services/`                   | `api/`, `schemas/`, `services/`   |
| Business Logic  | `features/`, `services/`              | `services/`, `core/`, `infra/`    |
| Shared Logic    | `shared/`, `lib/`                     | `shared/`, `utils/`               |
| Tests           | `__tests__/`, colocated with features | `tests/`, per module or per layer |

A well-structured project doesn’t just separate concerns—it reflects the *shape of your stack*.

---

