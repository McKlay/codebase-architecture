---
hide:
    - toc
---

## Part 4: Hybrid and Advanced Techniques

# 17. Organizing Assets, Tests, and Configuration

> “Folders aren't just for code.  
> They're for everything that keeps your project alive.”

---

### Why This Chapter Matters

As your project matures, you’ll deal with more than just components and services. You’ll accumulate:

* Images, videos, and static assets
* Mock data and test fixtures
* Environment configurations
* Lint rules, formatters, and pre-commit tools
* Docs for onboarding and architecture

If these aren't structured well, they rot.  
If they’re structured intentionally, they **accelerate development, testing, and collaboration**.

This chapter walks you through how to organize the *non-code side* of your project at scale.

---

### Organizing Assets: Images, Videos, Fonts, etc.

#### React Frontend (Vite)

```bash
frontend/
├── public/
│   ├── logo.svg
│   ├── blackhole.webm
│   └── manifest.json
├── src/
│   ├── assets/
│   │   ├── images/
│   │   ├── videos/
│   │   └── fonts/
```

| Folder    | Purpose                                           |
| --------- | ------------------------------------------------- |
| `public/` | Static assets copied directly to `/dist/`         |
| `assets/` | Imported assets via `import img from './img.png'` |

Use `assets/` for:

* Project-scoped, importable files (e.g., logo, icons, sounds)
* CSS-in-JS references
* Fonts, background videos

---

#### FastAPI Backend

```bash
backend/
├── static/
│   ├── examples/
│   ├── uploads/
│   └── docs/
```

Mount like this:

```python
app.mount("/static", StaticFiles(directory="static"), name="static")
```

Common backend assets:

* Uploaded files (PDFs, images)
* OCR outputs
* Generated previews or processed artifacts

---

### Organizing Tests and Fixtures

#### Folder Patterns

**Frontend:**

```bash
chatbot/
├── components/
│   └── ChatBox.tsx
├── __tests__/
│   └── ChatBox.test.tsx
```

**Backend:**

```bash
invoice/
├── services/
│   └── invoice_parser.py
├── __tests__/
│   └── test_invoice_parser.py
```

**Shared:**

```bash
shared/
├── test_helpers/
│   ├── mock_gpt.py
│   ├── fake_vectorstore.py
│   └── dummy_files/
```

Create `dummy_files/` for test image uploads, mock PDFs, etc.

---

#### Feature-Based vs Centralized Testing

| Style                                           | When to Use                                    |
| ----------------------------------------------- | ---------------------------------------------- |
| **Feature-based** (`__tests__/` in each module) | Unit tests, close to logic                     |
| **Centralized** (`tests/` at root)              | Integration/E2E, CI coverage reports           |
| **Hybrid (recommended)**                        | Use both—fast local tests, full-stack coverage |

---

### Organizing Environment Files and Secrets

Use per-app `.env` files, stored outside of Git.

**Frontend:**

```dotenv
.env.development
.env.production
```

**Backend:**

```dotenv
backend/
├── .env
```

Use `.env.example` in your repo for reference:

```dotenv
# backend/.env.example
OPENAI_API_KEY=your-key-here
DATABASE_URL=postgres://...
```

**Tip:** Use secret managers (GCP, Railway, GitHub) for CI environments.

---

### Configuration: Linting, Formatting, Git Hooks

Store in root:

```bash
project-root/
├── .eslintrc.js / .pylintrc
├── .prettierrc / pyproject.toml
├── .editorconfig
├── .gitignore
├── .pre-commit-config.yaml
```

Use tools like:

* ESLint + Prettier (React)
* Black, Ruff, or Flake8 (FastAPI)
* `pre-commit` for Git hook automation (format before commit)
* Husky (Node) or `pre-commit` (Python) to enforce standards

Run pre-checks on every PR via CI.

---

### Project Documentation

Create a `docs/` folder at the root:

```bash
docs/
├── architecture.md
├── backend-schema.png
├── ai-pipeline.md
├── onboarding.md
```

You can also power:

* A live developer portal (e.g., Docusaurus)
* MkDocs (`mkdocs.yml`)
* README embeds with diagrams

**Tip:** Store architectural diagrams (e.g., GPT + vector flow) alongside Markdown files using tools like [Excalidraw](https://excalidraw.com/) or [Diagrams.net](https://app.diagrams.net/).

---

### Bonus: Folder Conventions for Miscellaneous Concerns

| Folder             | Purpose                                        |
| ------------------ | ---------------------------------------------- |
| `scripts/`         | One-off migration scripts, utility CLI scripts |
| `jobs/`            | Scheduled background tasks or cronjobs         |
| `migrations/`      | Alembic or Prisma-style DB migrations          |
| `k8s/` or `infra/` | IaC configs for staging/prod                   |
| `mock-data/`       | JSON/CSV test data for manual frontend mocking |

---

### Summary: Organizational Hygiene at Scale

| Concern              | Best Practice                                        |
| -------------------- | ---------------------------------------------------- |
| Static assets        | Use `public/` and `assets/` clearly                  |
| Feature tests        | Use `__tests__/` per module                          |
| Integration tests    | Centralize under `tests/` or `e2e/`                  |
| Mocks and test files | Store in `test_helpers/` or `dummy_files/`           |
| Env vars             | Use `.env`, ignore real keys, commit `.env.example`  |
| Docs                 | Store architecture, onboarding, diagrams in `docs/`  |
| Config               | Keep linters, formatters, pre-commit in project root |

This isn’t “just organization.” It’s about **reducing friction** as your team and product grow.

---