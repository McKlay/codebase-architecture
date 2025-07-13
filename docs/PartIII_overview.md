---
hide:
    - toc
---

## Part 3: Scalability

# Overview: From Modular to Scalable Architecture

> “Modularity gives you structure.  
> Scalability gives you strategy.”

---

### Why Modularity Isn’t Enough

Modular architecture is a powerful starting point. It gives you:

* Feature isolation
* Testability
* Clear developer ownership

But as your app evolves—more features, more shared services, more contributors—you’ll hit **structural friction** that modularity alone can't solve.

That’s when you shift to **scalable architecture**.

Not because modular is wrong. But because:

* Modules start depending on shared services
* Global concerns like auth, logging, or analytics spread across features
* Your folder structure stops answering the question: *How do I grow this app with 10 developers and 100 routes?*

Scalability is about evolving your structure to match **team scale, system complexity, and domain boundaries.**

---

### What Makes an Architecture “Scalable”?

A scalable folder structure does four things well:

| Property                       | Description                                                                                                         |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------- |
| **Separation of layers**       | Clearly splits routing, services, models, and infra                                                                 |
| **Global service integration** | Supports logging, error handling, AI pipelines, permissions                                                         |
| **Developer parallelism**      | Allows multiple teams to work without merge conflicts                                                               |
| **Evolvability**               | You can replace or extend parts (e.g., swap GPT for Claude, or SQL for vector DB) without rewriting half the system |

It’s not about having more folders.
It’s about creating a structure that scales with both **technical load** and **team complexity**.

---

### What Changes When You Scale?

| Concern                | Modular Focus               | Scalable Focus                                         |
| ---------------------- | --------------------------- | ------------------------------------------------------ |
| Feature ownership      | Each module owns everything | Global services must be shared cleanly                 |
| Code reuse             | Optional and local          | Mandatory and enforced                                 |
| Testing                | Unit-first, colocated       | Unit + integration + CI pipelines                      |
| Dev velocity           | 1–2 devs/module             | 2+ teams, overlapping features                         |
| Cross-cutting concerns | Minimal                     | Core (auth, observability, versioning) must be layered |
| App orchestration      | Simple request→response     | Background jobs, queues, task orchestration, streams   |

Scalability introduces **architectural patterns**: services, adapters, repositories, core layers, shared infrastructure.

You move from “just building features” → to **engineering systems**.

---

### How This Part Is Structured

This part gives you everything you need to transition into a scalable architecture.

We’ll walk through:

---

### 📘 Chapter 11: Scalable Architecture Overview

* How to recognize scale pressure in your project
* When modular breaks down
* The shift toward layered thinking

---

### 📘 Chapter 12: Scalable Folder Structure for React + Vite

* Moving from `features/` to `layers/` (`api/`, `services/`, `components/`, etc.)
* Handling cross-feature UI and state
* Building reusable component libraries
* Team-oriented React structures

---

### 📘 Chapter 13: Scalable Folder Structure for FastAPI

* Layering your backend: `api/`, `services/`, `schemas/`, `infra/`, `core/`
* Handling async background jobs (e.g., Celery, FastAPI background tasks)
* Auth, role-based access, and scalable routing
* Repository pattern and clean architecture principles
* How to structure integrations with vector DBs, LLM clients, and queues

---

### 📘 Chapter 14: Deploying and Scaling React + FastAPI Projects

* Folder structure impact on Docker, CI/CD, and deployment flow
* Secrets, environment configs, and managing `.env` files across environments
* Asset management and public files
* Scaling horizontally vs. vertically
* How folder structure supports Infrastructure-as-Code (IaC)

---

