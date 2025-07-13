---
hide:
    - toc
---

## Appendix

---

### **A. Glossary of Terms**

| Term                       | Definition                                                                                                                                     |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| **Modular Structure**      | Organizing folders by features or domains, where each contains its own logic, UI, tests, and services. Promotes isolation and encapsulation.   |
| **Scalable Structure**     | Organizing folders by layers (api, services, models, infra, etc.) to support large teams, shared logic, and consistent abstraction boundaries. |
| **Hybrid Structure**       | A blend of modular (per feature) and scalable (per layer) architecture. Often starts modular and evolves into scalable.                        |
| **Feature Slice**          | A complete vertical set of files (UI, logic, tests) related to one business domain.                                                            |
| **Service Layer**          | A dedicated layer for reusable business logic, utilities, or model abstraction. Keeps features lightweight and testable.                       |
| **Separation of Concerns** | Design principle that promotes separating code based on distinct responsibilities (UI, logic, data, etc.)                                      |
| **High Cohesion**          | Keeping related pieces of logic close together to improve maintainability and understanding.                                                   |
| **Low Coupling**           | Reducing dependencies between components to ensure changes in one don’t break others.                                                          |
| **RAG**                    | Retrieval-Augmented Generation. Combines vector search with GPT-style generation to create grounded, document-aware answers.                   |

---

### **B. Tools and Libraries Reference**

#### Frontend

* **React** – Core frontend framework
* **Vite** – Lightning-fast dev server and bundler
* **Tailwind CSS** – Utility-first styling
* **Zustand / Redux** – State management
* **React Query / SWR** – Async data hooks
* **Framer Motion** – Animations
* **React Router v6** – Routing

#### Backend

* **FastAPI** – Modern, async Python API framework
* **Pydantic** – Data validation via schemas
* **Uvicorn** – FastAPI ASGI server
* **Tortoise ORM / SQLModel / SQLAlchemy** – DB layers
* **Celery** – Background task manager

#### AI/NLP

* **OpenAI (GPT)** – Chat/completion APIs
* **HuggingFace Transformers** – Custom or local LLMs
* **pgvector + Supabase** – Vector storage and similarity search
* **Tesseract / PaddleOCR** – OCR engines

---

### **C. Recommended Folder Structures (Cheat Sheets)**

#### Modular FastAPI (per-feature)

```bash
app/
└── features/
    ├── chat/
    │   ├── routes.py
    │   ├── services.py
    │   └── schemas.py
```

#### Scalable FastAPI (per-layer)

```bash
app/
├── api/
├── services/
├── schemas/
├── core/
├── vectorstore/
```

#### React Modular

```bash
src/
└── features/
    ├── chat/
    │   ├── ChatUI.tsx
    │   ├── chatSlice.ts
    │   ├── hooks.ts
    │   └── chatService.ts
```

#### React Scalable

```bash
src/
├── components/
├── hooks/
├── services/
├── shared/
```

---

### **D. Formatter, Linter, and Dev Config Tips**

| Purpose            | Tool         | Config File               |
| ------------------ | ------------ | ------------------------- |
| JavaScript Linting | `ESLint`     | `.eslintrc.json`          |
| Code Formatting    | `Prettier`   | `.prettierrc`             |
| Python Linting     | `ruff`       | `pyproject.toml`          |
| Python Formatting  | `black`      | `pyproject.toml`          |
| Pre-commit Hooks   | `pre-commit` | `.pre-commit-config.yaml` |

> 💡 **Tip**: Use [Husky](https://typicode.github.io/husky/) (JS) or `pre-commit` (Python) to enforce consistency before commits.

---

### **E. Further Reading and Resources**

* **Clean Architecture** – Robert C. Martin
* **Designing Software Architectures** – Humberto Cervantes
* **FastAPI Documentation** – [https://fastapi.tiangolo.com](https://fastapi.tiangolo.com)
* **React App Structure Best Practices** – Kent C. Dodds
* **Microservices vs Monoliths** – ThoughtWorks Tech Radar
* **Supabase Docs** – [https://supabase.com/docs](https://supabase.com/docs)
* **AI App Examples (LangChain, RAG)** – [https://github.com/hwchase17/langchain](https://github.com/hwchase17/langchain)

---

### **F. Migration Checklist & Structure Audit Guide**

#### Migration Checklist

* [ ] Identify duplicated logic (move to `services/`)
* [ ] Audit cross-feature dependencies (refactor to shared)
* [ ] Move config/env to `core/` or `.env`-based loaders
* [ ] Restructure tests to mirror feature or layer
* [ ] Document folder responsibilities in a `README.md`

#### Structure Audit Questions

* Are components tightly scoped to a domain?
* Is cross-feature logic reusable or duplicated?
* Can new developers find what they need in <5 seconds?
* Are routes, logic, and models clearly separated?
* Can you test a module in isolation?
* Are environments easy to switch between (dev, staging, prod)?

---

## 🎉 Closing Words

A good folder structure is **not set in stone**—but it is **intentional**.  
It evolves with your team, your product, and your ambition.

Structure isn’t just about where files live.
It’s about how **people think**, **collaborate**, and **build things together**.  

So build for clarity.  
Build for scale.  
Build for the next developer—including future-you.

— **Clay Mark Sarte**

---
