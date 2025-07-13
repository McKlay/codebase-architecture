---
hide:
    - toc
---

## Part 4: Hybrid and Advanced Techniques

# 16. Monorepos and Shared Logic

> “When frontend and backend speak the same language, everything moves faster.”

---

### Why Consider a Monorepo?

As your project grows to include:

* A React + Vite frontend
* A FastAPI backend
* Shared validation schemas
* Auth/session logic reused across layers
* Common constants or types (e.g., document status, GPT response formats)

…it becomes inefficient to split them across multiple repos.

A **monorepo** solves this by keeping everything in one place:

* Easier dev experience
* Shared logic is centralized
* Faster refactoring
* Smoother version control
* Unified CI/CD

> You stop duplicating logic—and start aligning systems.

---

### Example Monorepo Folder Layout

```bash
project-root/
├── apps/
│   ├── frontend/             # React + Vite app
│   └── backend/              # FastAPI app
├── packages/
│   ├── shared-schemas/       # Pydantic + TypeScript shared types
│   ├── utils/                # Shared logic, constants, helpers
│   └── gpt-core/             # Shared GPT prompt logic and formatting
├── .github/
│   └── workflows/
├── docker/
│   ├── docker-compose.yml
├── tsconfig.base.json
├── pyproject.toml / requirements.txt
├── README.md
└── turbo.json / nx.json / package.json
```

---

### Shared Logic That Actually Matters

| Shared Concern       | Example                                            |
| -------------------- | -------------------------------------------------- |
| **Schemas**          | Upload payloads, response DTOs, user objects       |
| **Types/Enums**      | DocumentStatus, ModelSource, RoleType              |
| **Prompt templates** | GPT input formats shared across frontend + backend |
| **Utils**            | Token counters, validators, text normalization     |
| **Constants**        | Max tokens, file size limits, system settings      |

Keeping these in `packages/` avoids:

* Duplication
* Divergent validation logic
* Fragile JSON conversions

---

### Setting Up Shared Packages

Let’s say you use:

* **TypeScript** in frontend
* **Python** in backend
* **Shared schemas** in both

#### For TypeScript:

Use [pnpm workspaces](https://pnpm.io/workspaces) or [Turborepo](https://turbo.build) to manage shared packages.

`package.json` (root):

```json
{
  "private": true,
  "workspaces": [
    "apps/*",
    "packages/*"
  ]
}
```

Example: `packages/shared-schemas/src/index.ts`

```ts
export interface ChatRequest {
  message: string
  model?: "gpt-3.5" | "gpt-4"
}
```

Then in frontend:

```ts
import { ChatRequest } from "@shared-schemas"
```

#### For Python (Backend):

Use `poetry`, `pip install -e .`, or a dedicated `shared/` module.

Example: `packages/shared_schemas/chat.py`

```python
from pydantic import BaseModel

class ChatRequest(BaseModel):
    message: str
    model: str = "gpt-3.5"
```

Then in `backend/app/api/chat.py`:

```python
from shared_schemas.chat import ChatRequest
```

💡 Want even tighter sync? Use [datamodel-code-generator](https://github.com/koxudaxi/datamodel-code-generator) to **generate Pydantic models from TypeScript types**, or vice versa.

---

### Tools for Managing Monorepos

| Tool               | Use Case                                           |
| ------------------ | -------------------------------------------------- |
| **Turborepo**      | JS monorepo with React + shared packages           |
| **pnpm**           | Fast dependency management and workspace linking   |
| **Nx**             | Task orchestration and code boundaries across apps |
| **Poetry**         | Python package management for shared backends      |
| **Docker Compose** | Dev containers for both frontend + backend         |
| **GitHub Actions** | Per-folder CI pipelines and test triggers          |

Use monorepo-aware tooling to:

* Avoid rebuilding everything on every commit
* Run only affected tests or builds
* Deploy changed services selectively

---

### Testing Shared Logic

Create shared test folders inside packages:

```bash
packages/
├── shared-schemas/
│   └── __tests__/
├── gpt-core/
│   └── test_templates.py
```

Run tests locally or in CI by glob pattern:

* `pytest packages/gpt-core/`
* `vitest run packages/shared-schemas/`

This ensures shared logic stays robust **without depending on app-specific tests**.

---

### Versioning Shared Logic

You have two choices:

1. **Tightly coupled:** Always deploy shared changes with apps (e.g., GPT prompt template update used by both frontend and backend)  
2. **Loosely versioned:** Use semver tags, changelogs, and publishing flow (e.g., `shared-schemas@1.2.3`)

For solo projects or fast-moving MVPs → tight coupling is fine.  
For production teams → use versioning tools like:

* [Changesets](https://github.com/changesets/changesets)
* [Lerna](https://lerna.js.org/)
* Private NPM/PyPI registries

---

### Recap: Folder Responsibilities

| Folder                     | Purpose                                 |
| -------------------------- | --------------------------------------- |
| `apps/frontend/`           | Vite, React, TSX UI                     |
| `apps/backend/`            | FastAPI app                             |
| `packages/shared-schemas/` | DTOs, request/response validation       |
| `packages/gpt-core/`       | Prompt templates, chunking, completions |
| `packages/utils/`          | String, token, image utils              |
| `docker/`                  | Container orchestration configs         |
| `.github/workflows/`       | CI/CD pipelines (monorepo-aware)        |

---

### Summary: Monorepos and Shared Logic

| Principle                      | Practice                                               |
| ------------------------------ | ------------------------------------------------------ |
| Keep everything in one place   | Use `apps/` + `packages/` monorepo model               |
| Share schemas and types safely | Export DTOs from `packages/shared-schemas/`            |
| Avoid copy-paste logic         | Extract GPT, OCR, validation logic to shared modules   |
| Use smart tooling              | Turborepo + Poetry + Docker Compose + CI matrix builds |
| Test + version shared logic    | Keep separate test suites and consider semver tagging  |

Monorepos give you the **clarity of modularity** with the **power of shared logic**.  
This is how real products grow—from prototypes to platforms.

---