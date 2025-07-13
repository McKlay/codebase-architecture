---
hide:
    - toc
---

## Part 4: Hybrid and Advanced Techniques

# 15. Hybrid Folder Structures

> “You don’t have to choose between modular and scalable.  
> You can evolve *through* them.”

---

### What is a Hybrid Folder Structure?

A **hybrid architecture** blends **feature-first modularity** with **layered scalability**.

You keep the independence of modules (e.g., `chatbot/`, `invoice/`)  
But you extract global logic (e.g., GPT, OCR, auth, logging) into reusable layers like `services/`, `core/`, and `shared/`.

> It’s not either-or.  
> It’s *both*—with boundaries.

Hybrid structure is often a **transition state**, but it’s also a **long-term strategy** for many growing apps.

---

### When Should You Go Hybrid?

You’ve outgrown pure modularity *but* aren’t ready to go full scalable.

Use hybrid when:

* You want to preserve **feature ownership**
* You need to centralize **GPT, OCR, or vector DB logic**
* Your React app has reusable UI systems + scoped features
* Your FastAPI backend has shared schemas, background tasks, or infra

In short: **you're building an AI-first product** that must scale over time—without losing its feature velocity.

---

### React: Hybrid Folder Example

```bash
src/
├── features/
│   ├── chatbot/
│   │   ├── ChatPage.tsx
│   │   ├── hooks/
│   │   ├── api/
│   │   └── __tests__/
│   └── invoice/
│       ├── UploadPage.tsx
│       ├── components/
│       └── __tests__/
├── api/              # Global API clients
├── services/         # Shared GPT, auth, upload logic
├── components/       # Shared UI components
├── hooks/            # Global utilities
├── context/          # Auth, theme, toast providers
└── store/            # Global state
```

You preserve feature folders for **UI flow**, but global workflows are abstracted to `/services/`.

Example:

* `useChatSession()` → uses `gptService.generateResponse()`
* `uploadInvoice()` → uses `uploadService.sendFile()`

---

### FastAPI: Hybrid Folder Example

```bash
app/
├── chatbot/
│   ├── api/
│   ├── schemas/
│   ├── __tests__/
├── invoice/
│   ├── api/
│   └── __tests__/
├── api/              # Versioned routing entry points
├── services/         # GPT, embeddings, invoice parsing
├── schemas/          # Shared schemas (e.g., pagination, base models)
├── infra/            # DB, vector store, Celery, Redis
├── tasks/            # Background jobs
├── core/             # Config, security, middlewares
└── main.py
```

Each feature still has local routes, but GPT logic is unified under `services/gpt_service.py`.

This allows:

* Parallel feature work
* Centralized optimization of shared layers
* Modular tests + scalable deployment

---

### Evolving from Modular → Hybrid → Scalable

Here’s a safe evolution path:

| Stage           | Folder Traits                                                                                   |
| --------------- | ----------------------------------------------------------------------------------------------- |
| **1. Modular**  | Feature-first: `chatbot/`, `invoice/`, `upload/`                                                |
| **2. Hybrid**   | Extract shared logic into `/services/`, `/shared/`, `/infra/`                                   |
| **3. Scalable** | Add strict layering: `/api`, `/core`, versioned APIs, DI, repository pattern, CI testing scopes |

Use **refactor points** as checkpoints:

* GPT logic duplicated? Extract to `services/`
* OCR utils copy-pasted? Move to `shared/ocr.py`
* Tests cross module boundaries? Split into `integration/` vs `unit/`
* CI/CD too slow? Use per-layer workflows

---

### Hybrid Folder Design Goals

| Goal                          | Design Strategy                                                   |
| ----------------------------- | ----------------------------------------------------------------- |
| Preserve modular feature flow | Keep `features/` or scoped `chatbot/` folders                     |
| Enable shared logic reuse     | Extract to `services/`, `shared/`, `infra/`                       |
| Avoid tight coupling          | Use dependency injection or shared interfaces                     |
| Keep tests isolated           | Per-feature `__tests__/`, global mocks in `test_helpers/`         |
| Support CI pipelines          | Build/test/deploy by folder scope (e.g., backend only if changed) |

---

### Migration Patterns: How to Transition Gracefully

#### Pattern: “Move First, Test Later”

* Duplicate shared logic into `services/`
* Use feature logic first
* Then write unit tests for `services/`

#### Pattern: “Wrapper Injection”

* Refactor GPT calls in `chatbot/` into a `GPTService` class
* Inject into multiple modules via shared `services/`

#### Pattern: “Refactor by Volume”

* Identify most-used utilities
* Extract only those
* Avoid premature generalization

#### Pattern: “Use Mocks to Abstract Away”

* Test `chatbot/` using fake `GPTService` before moving the real one

These patterns let you gradually scale without breaking working modules.

---

### Summary: Hybrid Folder Structures

| Principle                             | Application                                                   |
| ------------------------------------- | ------------------------------------------------------------- |
| Modularity gives ownership            | Keep `features/` or `modules/` for UI/business domains        |
| Scalability gives reuse               | Extract to `/services/`, `/shared/`, `/infra/`                |
| Don’t generalize too early            | Refactor real patterns only                                   |
| Use folder structure as a signal      | Is logic reused? Move it. Is it tightly scoped? Keep it local |
| Test per module, share mocks globally | `test_helpers/` supports the hybrid layer boundary            |

Hybrid isn’t a compromise.  
It’s a **mature middle state**—ready to support real AI workflows, rapid iteration, and future scale.

---