---
hide:
    - toc
---

## Part 5: Case Studies and Templates

# 22. Complete Starter Templates

> “Templates aren't shortcuts. They're scaffolds for clarity.”

---

This chapter presents **three real-world starter folder structures** based on the philosophies and trade-offs explored throughout the book:

1. Modular React + FastAPI Template
2. Scalable React + FastAPI Template
3. Hybrid AI Application Template (Modular features + Scalable backbone)

Each template includes:

* ✅ Folder layout
* ✅ Key file responsibilities
* ✅ CLI-ready structure (for cloning or bootstrapping)
* ✅ Recommendations for growth

---

## Modular React + FastAPI Template

> For rapid prototyping and clean feature isolation

### Root Structure

```bash
project-root/
├── backend/
│   └── app/
│       └── features/
│           ├── users/
│           └── invoices/
├── frontend/
│   └── src/
│       └── features/
│           ├── users/
│           └── invoices/
├── tests/
│   ├── users/
│   └── invoices/
```

### Ideal For:

* Small teams or solo devs
* Feature-driven apps
* Projects that may later scale

### Growth Path:

* Move shared logic into `services/` as features multiply
* Extract common types to `shared/` or external packages

---

## Scalable React + FastAPI Template

> For enterprise-grade apps or multi-team collaboration

### Root Structure

```bash
project-root/
├── backend/
│   └── app/
│       ├── api/
│       ├── services/
│       ├── schemas/
│       ├── core/
│       ├── vectorstore/
│       └── workers/
├── frontend/
│   └── src/
│       ├── features/
│       ├── components/
│       ├── services/
│       ├── hooks/
│       └── shared/
├── tests/
│   ├── unit/
│   └── integration/
├── .env, Dockerfile, docker-compose.yml
```

### Ideal For:

* Teams > 3 developers
* Microservices or LLM-powered systems
* RAG pipelines, background workers, CI/CD

### Growth Path:

* Add `/jobs/` or `/tasks/` for scheduled jobs
* Implement `infra/` for IaC (e.g., Terraform, Pulumi)

---

## Hybrid AI App Template (Best of Both Worlds)

> Modular features + scalable service layers

### Root Structure

```bash
project-root/
├── apps/
│   ├── frontend/
│   └── backend/
├── packages/
│   └── shared-schemas/
├── backend/
│   └── app/
│       ├── features/
│       │   ├── chatbot/
│       │   └── receipt/
│       ├── services/
│       ├── api/
│       ├── vectorstore/
│       └── core/
├── frontend/
│   └── src/
│       ├── features/
│       │   ├── chatbot/
│       │   └── receipt/
│       ├── shared/
│       ├── hooks/
│       └── components/
├── tests/
│   ├── chatbot/
│   └── receipt/
├── Dockerfiles, CI/CD, .envs
```

### Ideal For:

* AI apps with multiple pipelines (OCR + GPT + vector search)
* Shared embedding or file logic
* Production-ready Chatbots, Invoice Parsers, or Document RAG tools

### Growth Path:

* Add monorepo tools: `nx`, `TurboRepo`, or `pnpm workspaces`
* Use `shared-schemas/` or `shared-utils/` across frontend + backend

---

## Bootstrapping Tips

| Task                       | Tool                                                                 |
| -------------------------- | -------------------------------------------------------------------- |
| Dev containers             | `.devcontainer/` + VSCode Remote                                     |
| Dockerized local dev       | `docker-compose.yml` with separate `frontend` and `backend` services |
| Env management             | `.env` per app, `dotenv` in Python, Vite env vars in React           |
| Linting & Formatting       | `black`, `ruff` (Python), `eslint`, `prettier` (JS)                  |
| Monorepo support           | `pnpm workspaces` or `TurboRepo`                                     |
| Types & validation sharing | `zod` or `pydantic` in `shared-schemas`                              |

---

## GitHub Repo Format Recommendation

Structure your repo like so:

```bash
ai-app-starter/
├── apps/
│   ├── frontend/
│   └── backend/
├── packages/
│   ├── shared-schemas/
│   └── shared-utils/
├── .github/
│   └── workflows/
│       ├── test.yml
│       └── deploy.yml
├── docker-compose.yml
├── README.md
```

---

## Final Advice

* **Structure reflects team dynamics.** Modular for individuals, scalable for squads.
* **Structure is a trade-off map.** No “right” answer, only intentional trade-offs.
* **Structure is code philosophy.** Enforce conventions that encode your values: reusability, clarity, testability.

---