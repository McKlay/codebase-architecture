---
hide:
  - toc
---

## **Codebase Architecture: Designing for Scale, Modularity, and Sanity**
### A Practical Guide to Structuring React + FastAPI Projects in the Age of AI

---

### **Contents**

---

#### 📖 [Preface](Preface.md)

- [Why This Book Exists](Preface.md#why-this-book-exists)

- [Who Should Read This](Preface.md#who-should-read-this)

- [From Chaos to Clarity: How This Book Was Born](Preface.md#from-chaos-to-clarity-how-this-book-was-born)

- [What You’ll Learn (and What You Won’t)](Preface.md#what-youll-learn-and-what-you-wont)

- [How to Read This Book (Even If You’re Mid-Project)](Preface.md#how-to-read-this-book-even-if-youre-mid-project)

---

#### Part I – [Foundations](PartI_overview.md)

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 1: [Introduction](chapter1.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.1 What this book solves?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.2 Who this book is for

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 2: [Why Folder Structure Matters](chapter2.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.1 Anatomy of growing software chaos

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.2 Real-world symptoms of bad structure

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.3 The role of architecture in scale and collaboration

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 3: [Modularity vs. Scalability](chapter3.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.1 Definitions and core differences

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.2 Why most projects need both

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.3 Thinking in layers vs. features

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 4: [How Structure Impacts Software Quality](chapter4.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.1 Maintainability

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.2 Testability

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.3 Developer onboarding

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.4 Refactoring ease

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 5: [Core Principles of Architecture](chapter5.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.1 Separation of Concerns

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.2 High Cohesion, Low Coupling

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.3 DRY and Reusability

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.4 Convention over Configuration

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5.5 Structure as API for teams

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 6: [Understanding the Stack](chapter6.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.1 Overview of React + Vite (Frontend)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.2 Overview of FastAPI (Backend)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.3 How they communicate (REST, WebSockets, Streaming)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.4 Shared challenges: auth, state sync, rate limiting, API versioning

---

#### Part II – [Modularity](PartII_overview.md)

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 7: [Modular Architecture Overview](chapter7.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.1 What is modular structure?

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.2 MWhen to use it

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;7.3 Benefits vs. drawbacks

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 8: [Modular Folder Structure for React + Vite](chapter8.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.1 Feature-first organization

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.2 Example folder breakdown

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.3 Managing component isolation

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.4 Organizing hooks, services, and utils within modules

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.5 Naming conventions for modular files

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;8.6 Avoiding shared state bleed

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 9: [Modular Folder Structure for FastAPI](chapter9.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;9.1 Feature-centric backend design

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;9.2 Organizing routes, schemas, services

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;9.3 Keeping shared logic clean (e.g., OCR, GPT clients, DB connectors)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;9.4 Handling circular imports

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;9.5 Structuring testable service layers

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;9.6 Dependency injection and inversion

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 10: [Testing in Modular Projects](chapter10.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;10.1 Writing tests per module

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;10.2 Shared mock strategies

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;10.3 Organizing test folders and fixtures

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;10.4 Keeping modules independently testable

---

#### Part III – [Scalability](PartIII_overview.md)

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 11: [Scalable Architecture Overview](chapter11.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;11.1 When modular is not enough

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;11.2 Symptoms of scale pressure

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;11.3 Layered design thinking

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 12: [Scalable Folder Structure for React + Vite)](chapter12.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;12.1 Layer-first approach: `/api`, `/services`, `/hooks`, `/components`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;12.2 Managing cross-feature logic and global state

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;12.3 Shared UI systems and design tokens

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;12.4 Component libraries and scalable styling

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;12.5 Supporting multiple teams on the same codebase

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 13: [Scalable Folder Structure for FastAPI](chapter13.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;13.1 Layered backend: `/api`, `/services`, `/schemas`, `/core`, `/infra`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;13.2 Clean separation of routers, models, and business logic

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;13.3 Supporting background jobs (e.g., Celery, FastAPI tasks)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;13.4 Auth, permissions, and role-based modules

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;13.5 Versioned routing and scalable endpoints

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;13.6 Repository pattern for data access

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;13.7 Handling vector DBs and external APIs gracefully

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 14: [Deploying and Scaling React + FastAPI Projects](chapter14.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;14.1 Folder structure impact on CI/CD pipelines

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;14.2 Docker best practices for structure

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;14.3 Managing environment variables and secrets

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;14.4 Static files and public assets

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;14.5 Horizontal vs. vertical scaling

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;14.6 Infra as code (IaC) tips for growth

---

#### Part IV – [Hybrid and Advanced Techniques](PartIV_overview.md)

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 15: [Hybrid Folder Structures](chapter15.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15.1 What hybrid means (modular + scalable)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15.2 Use cases for hybridization

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15.3 Evolving modular apps to scalable ones

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15.4 Gradual migration strategies

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15.5 Refactor patterns and heuristics

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 16: [onorepos and Shared Logic](chapter16.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;16.1 When to use a monorepo

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;16.2 Structuring React + FastAPI in one codebase

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;16.3 Sharing types, schemas, and validations

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;16.4 Using pnpm workspaces, Turborepo, or Nx

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;16.5 Versioning strategies for shared logic

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 17: [Organizing Assets, Tests, and Configuration](chapter17.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;17.1 est practices for `/assets`, `/env`, `/docs`, and `/configs`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;17.2 Feature-based vs. centralized testing

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;17.3 Managing mock data and API fakes

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;17.4 Global configs vs. per-feature config files

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;17.5 Linting, formatting, and pre-commit structuring

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 18: [Migration Playbook](chapter18.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;18.1 From flat → modular → scalable

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;18.2 Folder audit checklist

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;18.3 Common mistakes and how to resolve them

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;18.4 Refactor-safe conventions

---

#### Part V – [Case Studies and Templates](PartV_overview.md)

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 19: [Case Study: Smart Receipt/Invoice Analyzer](chapter19.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;19.1 OCR-first modular architecture

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;19.2 GPT pipeline integration

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;19.3 Frontend dynamic table + backend processing

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;19.4 How modular evolved into hybrid

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 20: [Case Study: Document Intelligence Chatbot](chapter20.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;20.1 Fully scalable design

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;20.2 Embeddings and vector DBs

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;20.3 Chat UI, streaming, RAG pipeline

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;20.4 React frontend folder → FastAPI backend folder

&nbsp;&nbsp;&nbsp;&nbsp; Chapter21: [Case Study: AI-Powered Mockup-to-Code Tool](chapter21.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;21.1 GPT-driven frontend generator

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;21.2 Image parsing modules

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;21.3 Event and layout-driven architecture

&nbsp;&nbsp;&nbsp;&nbsp; Chapter 22: [Complete Starter Templates](chapter22.md)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;22.1 React + FastAPI Modular Template

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;22.2 React + FastAPI Scalable Template

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;22.3 React + FastAPI Hybrid AI App Template

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;22.4 Folder walk-throughs and clone instructions

---

#### [Appendices](appendices.md)

**A. Glossary of Terms**  
**B. Tools and Libraries Reference**  
**C. Recommended Folder Structures (Cheat Sheets)**  
**D. Formatter, Linter, and Dev Config Tips**  
**E. Further Reading and Resources**  
**F. Migration Checklist & Structure Audit Guide**

---