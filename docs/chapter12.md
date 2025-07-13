---
hide:
    - toc
---

## Part 3: Scalability

# 12. Scalable Folder Structure for React + Vite

> “When your features multiply, your structure must evolve from *modules* to *layers*.”

---

### From Modular to Layered Thinking

In a modular frontend, each feature folder owns everything it needs:

* UI components
* Hooks
* API calls
* Styles

But at scale—especially in apps powered by GPT, vector DBs, and real-time state—you need:

* Shared data access across features
* Global UI consistency
* Reusable design tokens, hooks, services
* Better state orchestration

That’s when **feature-first** breaks down.
And **layer-first architecture** takes over.

---

### Recommended Scalable Folder Layout

Here’s a common, proven structure:

```bash
src/
├── api/                # Centralized backend request clients
├── services/           # Business logic, GPT helpers, session managers
├── components/         # Reusable UI elements (e.g., Button, Modal)
├── hooks/              # Shared, cross-feature hooks
├── pages/              # Route-level views
├── features/           # Optional: feature-specific UI slices
├── context/            # React Context providers for global state
├── store/              # Zustand/Jotai/Recoil state atoms/selectors
├── styles/             # Global styles, Tailwind config, tokens
├── ui/                 # Shared design system
├── utils/              # Generic utility functions
├── constants/          # Enum definitions, global config
└── main.tsx            # App entry point
```

This structure scales **both with code and with teams**.

---

### Layer Breakdown

Let’s zoom in on each layer.

---

#### `/api/`

* Purpose: Handle all **network requests** (REST, GraphQL, streaming)
* Contents: `chat.ts`, `invoice.ts`, `user.ts`
* Pattern: Export clean functions → `getMessages()`, `postInvoice()`
* Optional: Create axios/fetch client with interceptors

Benefits:

* All APIs are discoverable in one place
* Easily mockable for testing
* No direct fetch logic inside components

---

#### `/services/`

* Purpose: App-level logic (e.g., GPT workflows, token management, document preprocessing)
* Contents: `gptService.ts`, `authService.ts`, `uploadManager.ts`
* May call `api/` internally

Use services to:

* Encapsulate async logic
* Coordinate multi-step flows
* Reduce logic bloat in components/hooks

---

#### `/components/` and `/ui/`

* Purpose: Core UI primitives + custom reusable components
* Contents:

  * `components/`: App-specific components (`ChatBox`, `InvoiceForm`)
  * `ui/`: Design system components (`Button`, `Tooltip`, `Card`)

Tip: Start with `components/`. Create `ui/` when things repeat.

---

#### `/hooks/`

* Purpose: Global reusable hooks
* Examples:

  * `useCopyToClipboard.ts`
  * `useLocalStorage.ts`
  * `useWindowFocus.ts`

Each hook should:

* Be scoped and named clearly
* Avoid feature-specific logic
* Be mockable in tests

---

#### `/context/` and `/store/`

* Purpose: Global or scoped state management
* `/context/`: React Contexts
* `/store/`: Zustand or Jotai atoms/selectors

Scalable apps use context only where it makes sense:

* Auth session
* Theme
* UI notifications

Use store for:

* Shared state across features
* Subscribable, testable state atoms

---

#### `/pages/`

* Purpose: Route entry points
* Pattern:

  * `pages/index.tsx` → Home
  * `pages/chat.tsx` → Chat interface
  * `pages/invoice.tsx` → Upload + GPT result view

These load views and pass props → logic and UI come from other layers.

---

#### `/features/` (Optional at scale)

If you want to preserve modularity at a high level, you can still group:

```bash
features/
├── chatbot/
│   ├── ChatPage.tsx
│   ├── hooks/
│   ├── services/
```

This is useful if your team is organized by feature verticals.
Otherwise, it may blur with the layer-first model—choose one primary style.

---

### Managing Cross-Feature State and Logic

At scale, you need **shared flows**:

* Multiple features might rely on GPT logic
* Upload state might be accessed in both `invoice/` and `chat/`
* Auth roles need to affect route visibility and UI

Strategies:

* Keep shared logic in `services/`, not in components
* Keep global state in `store/`, scoped state in `hooks/`
* Avoid relying on `window` or ad-hoc `localStorage` access

Structure enables **state control** and **predictable data flow**.

---

### Shared UI Systems and Design Tokens

Create a design system via:

```bash
styles/
├── tailwind.config.js
├── tokens/
│   ├── colors.ts
│   ├── spacing.ts
│   └── typography.ts
```

And combine it with:

```bash
ui/
├── Button.tsx
├── Input.tsx
├── Card.tsx
```

Benefits:

* Consistent styling across the app
* Easily themeable
* Component libraries become pluggable

---

### Supporting Multiple Teams on the Same Codebase

If your project has >2 frontend contributors:

* Split teams by **domain** (e.g., Chat, Upload, Dashboard)
* Define clear **ownership** of `/features/`, `/services/`, and `/components/`
* Use **codeowners**, PR checklists, or monorepo tooling (Nx, Turborepo) to divide boundaries
* Consider creating **visual regression snapshots** or **storybook previews**

Structure gives teams room to scale **without stepping on each other**.

---

### Summary: Scalable React Structure Principles

| Layer                | Purpose                                 |
| -------------------- | --------------------------------------- |
| `api/`               | Encapsulate backend interaction         |
| `services/`          | Handle app-level logic and AI workflows |
| `hooks/`             | Share non-UI logic across components    |
| `components/`, `ui/` | Reusable UI building blocks             |
| `pages/`             | Entry point views for routes            |
| `context/`, `store/` | Global state providers and management   |
| `styles/`, `tokens/` | Design system configuration and theming |

Scaling React isn't about folders.
It’s about **building structure that reflects flow, ownership, and reuse**.

---