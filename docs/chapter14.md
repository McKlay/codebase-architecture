---
hide:
    - toc
---

## Part 3: Scalability

# 14. Deploying and Scaling React + FastAPI Projects

> “Your folder structure isn't just for developers—it's for deployment, too.”

---

### Why Structure Impacts Deployment

It’s easy to treat folder structure as a dev-only concern.

But the moment you deploy, you realize:

* CI/CD needs clean build scopes
* Docker needs clear entry points and build contexts
* Secrets must be isolated per environment
* Public assets and static files must follow web server rules
* Infrastructure-as-Code (IaC) prefers predictable file paths

A scalable folder structure ensures:

* Reliable deploys
* Fast builds
* Safe environments
* Consistent developer → staging → production flows

---

### Recommended Project Root Layout (Monorepo Style)

```bash
project-root/
├── frontend/              # React + Vite app
│   ├── src/
│   ├── public/
│   ├── index.html
│   └── vite.config.ts
├── backend/               # FastAPI app
│   ├── app/
│   ├── requirements.txt
│   ├── main.py
│   └── Dockerfile
├── .github/               # GitHub Actions workflows
│   └── workflows/
├── docker/
│   ├── nginx/             # Optional reverse proxy
│   ├── docker-compose.yml
├── .env                   # Default environment config
├── README.md
└── Caddyfile / nginx.conf # Optional web server configs
```

This structure:

* Separates build boundaries
* Allows independent or joint deployment
* Works well for Docker, Heroku, Render, Railway, GCP, AWS

---

### Docker Best Practices for Scalable Structure

**Frontend `Dockerfile` (React + Vite):**

```Dockerfile
FROM node:18 AS build
WORKDIR /app
COPY frontend/ ./
RUN npm install && npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
COPY docker/nginx/nginx.conf /etc/nginx/conf.d/default.conf
```

**Backend `Dockerfile` (FastAPI):**

```Dockerfile
FROM python:3.11
WORKDIR /app
COPY backend/ .
RUN pip install -r requirements.txt
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

**Benefits of folder-aware Docker:**

* Minimal build context
* Clear separation of concerns
* Fast rebuilds (cache efficient)
* Works with mono or multi-container setups

---

### Managing Environment Variables and Secrets

A scalable structure needs isolated environment configs:

```bash
project-root/
├── .env
├── frontend/.env.development
├── frontend/.env.production
├── backend/.env
```

Use tools like:

* **`dotenv`** or **Vite's `import.meta.env`** for React
* **`python-dotenv`** or `os.environ.get()` for FastAPI

**NEVER COMMIT `.env` FILES TO GIT**
Instead, use:

* `.env.example`
* Secret managers (e.g., GitHub Actions Secrets, Railway, GCP Secret Manager)

---

### Static Files and Public Assets

**Frontend:**

* Store public files in `frontend/public/`
* Vite will copy them to `dist/` at build time

**Backend:**

* Mount static files using FastAPI:

```python
from fastapi.staticfiles import StaticFiles

app.mount("/static", StaticFiles(directory="static"), name="static")
```

* Serve docs, previews, or OCR outputs this way

If using **Nginx or Caddy**, route `/static/` or `/docs/` to these folders via reverse proxy.

---

### CI/CD and Folder Structure

A good structure makes CI/CD:

* Easier to configure
* Faster to run
* Safer to deploy

**Example: GitHub Actions Workflow**

```yaml
name: Deploy App

on:
  push:
    branches: [main]

jobs:
  build-frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install & Build Frontend
        working-directory: frontend
        run: |
          npm install
          npm run build

  build-backend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install & Test Backend
        working-directory: backend
        run: |
          pip install -r requirements.txt
          pytest
```

**Tips:**

* Run tests only on changed folders (CI optimization)
* Use paths like `frontend/**` or `backend/**` for job triggers
* Structure build outputs (`dist/`, `coverage/`, `artifacts/`) cleanly

---

### Scaling Deployment: Mono vs Multi-Container

| Strategy             | When to Use              | Notes                                                                      |
| -------------------- | ------------------------ | -------------------------------------------------------------------------- |
| **Mono-container**   | Simple apps, early stage | One container runs both frontend + backend                                 |
| **Multi-container**  | Recommended at scale     | Separate concerns (React container, API container, Nginx, vector DB, etc.) |
| **Docker Compose**   | Local dev orchestration  | Define networks, volumes, and environment links                            |
| **Kubernetes / ECS** | Advanced scale           | Define deployment rules, autoscaling, health checks                        |

Folder structure should support:

* Multiple `Dockerfile`s
* Shared `.env` format
* Central `docker-compose.yml`

---

### Infrastructure as Code (IaC) and Scaling

Once you grow beyond Docker Compose, consider:

* **Terraform**: Define AWS/GCP infrastructure as code
* **Pulumi** or **Ansible**: Automate provisioning
* **GitOps**: Auto-deploy based on Git state (ArgoCD, Flux)

Folder tips:

```bash
infra/
├── terraform/
├── ansible/
└── k8s/
```

Structure enables:

* Environments (dev, staging, prod)
* Secure secrets and access
* CI/CD automation with IaC

---

### Summary: Structure for Deployment and Scaling

| Concern                     | Structure Tip                                                    |
| --------------------------- | ---------------------------------------------------------------- |
| CI/CD pipelines             | Use separate `frontend/`, `backend/` folders with scoped runners |
| Docker build scope          | Keep `Dockerfile` close to code, use `.dockerignore`             |
| Secrets management          | Use `.env` per app, don’t commit secrets                         |
| Public assets               | Use `/public/` and `/static/` with correct mounts                |
| Multi-service orchestration | Use `docker/`, `infra/`, and clear service boundaries            |
| IaC integration             | Add `infra/` or `k8s/` folders for DevOps teams                  |

Your folder structure is the foundation for stable, scalable, reproducible deployments.

---