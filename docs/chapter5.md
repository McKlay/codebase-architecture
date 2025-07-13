---
hide:
    - toc
---

## Part 1: Foundations

# 5. Core Principles of Architecture

> Folder structure without principles is just rearranged chaos.

---

### Why Principles Matter First

Before you argue over whether `components/` should live inside `features/` or alongside them—**step back.**

Folder structure isn’t just about putting files in neat rows.  
It’s about making architectural principles *visible*.

Every good structure is guided by invisible laws.  
These laws define:

* How files are grouped
* How modules depend on each other
* Where logic lives—and where it doesn’t
* How flexible, testable, and scalable your app becomes

Let’s walk through the five foundational principles that underpin every high-quality folder structure.

---

### 1. Separation of Concerns (SoC)

> “One folder. One responsibility.”

Every folder should represent a **single concern**: a page, a service, a business domain, or a shared utility.

If a folder contains:

* API route handlers,
* UI components,
* GPT service logic,
* and database calls...

…it’s no longer a folder—it’s a **collision zone**.

SoC helps you answer:

* Where does rendering end and logic begin?
* Where do AI-specific utilities go?
* Where should I move this auth middleware so others can reuse it?

Apply SoC at every level:

* File level: one purpose per file
* Folder level: one domain per folder
* App level: clear boundaries between frontend/backend/shared

A well-separated structure gives you **local reasoning**: you don’t have to understand the whole app to fix one part.

---

### 2. 🧱 High Cohesion, Low Coupling

> “Cohesion groups what belongs together. Coupling tears apart what should stay apart.”

**High Cohesion** means:

* Related files live together
* A module’s logic and tests are close
* Changes happen in one place

**Low Coupling** means:

* Modules know as little as possible about each other
* Shared logic is exposed via clear interfaces
* Breaking changes are contained within their boundaries

Example:

```bash
chatbot/
├── api/
├── services/
├── components/
```

All chatbot-specific logic is *cohesive*.  
But if chatbot’s code directly calls `invoice/ocr_utils.py`—that’s *tight coupling*.  
Instead, shared OCR logic should live in a neutral layer like `shared/ocr/`.

Cohesion makes modules *strong*.  
Decoupling makes systems *resilient*.

---

### 3. DRY and Reusability

> “Don’t Repeat Yourself—unless it makes things clearer.”

DRY isn't about obsessively deduplicating lines of code.  
It’s about **eliminating accidental repetition**—the kind that creates bugs and bloated deployments.

Folder structure supports DRY when:

* Shared hooks and services are isolated in `shared/` or `core/`
* Feature code doesn’t reinvent the wheel because it *can find* the wheel
* GPT-related logic lives in one `gpt_service.py`, not five copied files

That said, be careful: **premature sharing** leads to unwanted coupling.  
If two features need similar but divergent behavior, copy first.  
Only extract shared code once the pattern emerges.

Reusability works best when **structure makes reuse obvious**.

---

### 4. Convention over Configuration

> “Structure isn’t just for machines. It’s for humans.”

A strong folder layout reduces the need for explanations.

Good conventions answer questions like:

* Where do I find the services for `invoice/`?
* Where should new routes go?
* Where are the frontend hooks stored?
* What’s the standard file name for a test or schema?

Examples:

* `api/` always holds routes.
* `services/` always handles external integrations and business logic.
* `__tests__/` lives alongside the module it tests.

These conventions become muscle memory.  
They create a **shared mental model** that reduces onboarding time, avoids misplacement, and prevents structural drift.

Choose conventions early. Then stick to them.

---

### 5. Structure as API for Teams

> “If your codebase is a city, folders are the zoning laws.”

Structure isn’t just technical. It’s **social**.

Each team, each developer, each contributor relies on structure to:

* Understand boundaries
* Avoid stepping on others' work
* Know what they own and what they don’t
* Merge code with confidence

Great folder structures:

* Create lanes for frontend vs backend teams
* Prevent merge conflicts by minimizing overlap
* Enable component reuse without cross-team negotiation

When structure reflects architecture—and architecture reflects ownership—teams move faster, with fewer surprises.

Structure is a communication tool.  
Make it speak clearly.

---

### The Principles in Practice

| Principle                   | In Practice Example                                         |
| --------------------------- | ----------------------------------------------------------- |
| Separation of Concerns      | `chatbot/api/`, `chatbot/services/`, `chatbot/ui/`          |
| High Cohesion, Low Coupling | `gpt_service.py` reused across modules via DI container     |
| DRY & Reusability           | `shared/hooks/useFileUpload.ts`, not duplicated in features |
| Convention > Configuration  | All test files end in `.test.ts` and live beside logic      |
| Structure as Team API       | Clearly isolated folders per team or domain                 |

---
