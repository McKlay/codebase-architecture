---
hide:
    - toc
---

## Part 4: Hybrid and Advanced Techniques

# 18. Migration Playbook

> “Structure isn’t something you start with.  
> It’s something you grow into—intentionally.”

---

### Why Migration Matters

Most teams don’t start with a perfect folder structure.  
They start with:

* `src/components/`, `api.js`, and `utils.js`
* A backend with 800-line `main.py`
* `test/` as a dumping ground for broken fixtures

This chapter teaches you how to **migrate from flat → modular → scalable** without fear.

> Migration is not a one-shot effort.  
> It’s a guided evolution—powered by patterns and audit discipline.

---

### 3-Stage Evolution

| Stage        | Description                                           | When It Breaks                      |
| ------------ | ----------------------------------------------------- | ----------------------------------- |
| **Flat**     | Everything in one or two folders (`src/`, `api/`)     | 5+ features, unclear ownership      |
| **Modular**  | Feature-first folders (`chatbot/`, `invoice/`)        | Shared logic begins to duplicate    |
| **Scalable** | Layered global logic (`services/`, `core/`, `infra/`) | Team scaling, cross-feature demands |

You don’t need to jump to “enterprise mode” overnight.  
Instead, migrate **feature-by-feature**, backed by rules and refactors.

---

### Folder Audit Checklist

Before refactoring, **audit your project** with this checklist:

#### Flat Signs of Trouble:

* 50+ files inside `src/`
* `utils.js` is over 300 lines
* Route handlers and business logic mixed
* Tests refer to multiple features
* Files are hard to discover (“Where’s that PDF upload again?”)

#### Migration Candidates:

* GPT logic used by both chatbot and invoice modules
* OCR parsing duplicated across endpoints
* Shared data models scattered across files
* Components reused in 3+ pages

If you said “yes” to any → you’re ready to modularize or extract shared logic.

---

### Safe Refactor Patterns

#### 1. Extract to `services/` or `shared/`

* Take a well-known utility or GPT function
* Move it to `/services/gpt.py` or `/shared/gptService.ts`
* Refactor just *one* feature to use it first

#### 2. Use Aliases or Absolute Imports

* Instead of `../../../shared/utils.py`, use:

```ts
import { formatTokens } from "@shared/gpt"
```

```python
from shared.utils.gpt import format_tokens
```

Update `vite.config.ts`, `pyrightconfig.json`, or `PYTHONPATH` accordingly.

#### 3. Extract & Mock

* Extract shared logic
* Temporarily mock it in tests to avoid full rewrites

```ts
vi.mock("@shared/gpt", () => ({
  generateResponse: vi.fn(() => "mocked")
}))
```

#### 4. Strangle-the-Monolith

Refactor a feature one layer at a time:

1. Move `routes/`
2. Move `services/`
3. Move `schemas/`
4. Migrate tests

Repeat per feature.

---

### Common Migration Mistakes

| Mistake                                      | Safer Alternative                                         |
| -------------------------------------------- | --------------------------------------------------------- |
| Massive refactor PR                       | Refactor per module or layer                              |
| Duplicate logic then forget to remove old | Use TODO comments and GitHub issues                       |
| Break imports globally                     | Use safe alias migration (`vite.config.ts`, `PYTHONPATH`) |
| Remove tests during cleanup               | Keep old tests running until migration is verified        |
| Refactor unneeded files                   | Prioritize high-traffic or high-duplication areas         |

You’re not chasing perfection. You’re building **momentum**.

---

### Refactor-Safe Naming Conventions

Use consistent names so that features and shared logic are immediately obvious.

| Folder       | Naming Pattern                                        |
| ------------ | ----------------------------------------------------- |
| `services/`  | `gpt_service.py`, `ocr_parser.ts`, `vector_client.py` |
| `features/`  | `chatbot/`, `invoice/`, `upload/`                     |
| `schemas/`   | `chat.py`, `invoice.py`, `shared.py`                  |
| `__tests__/` | `test_<module>.py` or `<Component>.test.tsx`          |
| `docs/`      | `architecture.md`, `auth-flow.png`                    |

Use **underscores for Python**, **camelCase or PascalCase for TypeScript**.

---

### CI/CD Considerations After Migration

#### Split build/test pipelines:

* Use matrix builds per folder (`frontend`, `backend`)
* Cache shared packages (`packages/`)

#### Set up folder watchers:

* Rebuild only affected layers
* GitHub Actions:

```yaml
paths:
  - "apps/frontend/**"
  - "packages/shared-schemas/**"
```

#### Test the structure:

* Run `tree -L 3` or use `code-stats` CLI to visualize folder shape
* Confirm your `README.md` shows updated structure + dev flow

---

### Migration Summary

| Phase    | Action                                                           |
| -------- | ---------------------------------------------------------------- |
| Audit    | Identify bloated files, duplicated logic, cross-feature coupling |
| Stage 1  | Move to feature folders: `chatbot/`, `invoice/`                  |
| Stage 2  | Extract shared GPT, OCR, and services to global layers           |
| Stage 3  | Add testing strategies, docs, and versioned API structure        |
| Maintain | Lock in CI pipelines and developer onboarding docs               |

> Folder structure is the interface your team uses daily.  
> Migration isn’t cleanup—it’s optimization.

You now have a full framework for evolving any project into a modular, scalable, and maintainable system.

---