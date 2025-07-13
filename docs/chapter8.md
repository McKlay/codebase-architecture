---
hide:
    - toc
---

## Part 2: Modularity

# 8. Modular Folder Structure for React + Vite

> “Features first. Everything else follows.”

---

### Why Feature-First Wins

React encourages **composability**—building UI from reusable components. But without the right structure, composability becomes clutter.

The most scalable pattern in growing React apps is a **feature-first folder structure**, where each module owns its:

* Components
* Hooks
* API logic
* Styles
* Tests
* Local state (if applicable)

Instead of separating code by *type*, we separate by *purpose*.

You don’t have a `components/` folder.
You have a `chatbot/components/` folder.
You don’t write `useUpload.ts`.
You write `upload/hooks/useFileUpload.ts`.

This allows developers to **reason locally**, **test quickly**, and **refactor confidently.**

---

### Example Modular Folder Layout (React + Vite)

Let’s look at a common structure:

```bash
src/
├── chatbot/
│   ├── components/
│   ├── hooks/
│   ├── api/
│   ├── services/
│   ├── styles/
│   └── __tests__/
├── invoice/
│   ├── components/
│   ├── services/
│   ├── utils/
│   └── __tests__/
├── upload/
│   ├── components/
│   ├── hooks/
│   └── api/
├── shared/
│   ├── components/
│   ├── hooks/
│   ├── ui/
│   ├── constants/
│   └── types/
├── routes/
│   └── index.tsx
└── main.tsx
```

Each feature (e.g., `chatbot/`, `invoice/`, `upload/`) owns its own internal structure. The `shared/` folder holds reusable utilities, types, UI primitives, and global assets.

---

### Breaking It Down: Folder by Folder

#### - `chatbot/`, `invoice/`, `upload/` (Feature Modules)

Each folder represents a **domain of functionality**. Internally, it may contain:

| Subfolder     | Purpose                                          |
| ------------- | ------------------------------------------------ |
| `components/` | React components scoped to the feature           |
| `hooks/`      | Feature-specific logic (e.g., `useChatbotInput`) |
| `api/`        | Feature-specific API calls                       |
| `services/`   | Feature-specific logic or wrappers               |
| `styles/`     | Scoped CSS modules or Tailwind config extensions |
| `__tests__/`  | Unit and integration tests                       |

These folders evolve independently.
You can rewrite `invoice/` logic without touching `chatbot/`.

#### - `shared/`

This is your **common ground**—used by all features but owned by none.

Recommended structure:

```bash
shared/
├── components/      # Globally reused components (e.g., Button, Modal)
├── hooks/           # Truly shared hooks (e.g., useDebounce, useClipboard)
├── ui/              # Design tokens, themes, Tailwind config, icons
├── types/           # Global TypeScript types and interfaces
├── utils/           # Shared utility functions (e.g., dateFormat)
├── constants/       # Global constants and enums
```

Think of `shared/` as your *design system + logic toolbox*.

#### - `routes/`

This is where React Router v6 lives.  
You define your top-level routes here and import feature pages as lazy-loaded modules.

Example:

```tsx
import { ChatbotPage } from '@/chatbot';
import { InvoicePage } from '@/invoice';

const routes = [
  { path: '/chatbot', element: <ChatbotPage /> },
  { path: '/invoice', element: <InvoicePage /> },
];
```

You can also colocate layouts here (`MainLayout.tsx`, `DashboardLayout.tsx`).

---

### Managing Component Isolation

One key to good modularity is **component scope**.  
Not all components are meant to be reused across the app.

| Component Type       | Folder Location                     |
| -------------------- | ----------------------------------- |
| Feature-specific UI  | `invoice/components/InvoiceRow.tsx` |
| Globally reused UI   | `shared/components/Tooltip.tsx`     |
| Third-party wrappers | `shared/ui/CustomModal.tsx`         |

A rule of thumb:

> If only one feature uses it, keep it in the feature.
> If two or more features need it, move it to `shared/`.

---

### Organizing Hooks, Services, and Utils Within Modules

Let’s say the `chatbot/` feature handles:

* Input parsing
* GPT API calls
* Response rendering

You might structure like this:

```bash
chatbot/
├── hooks/
│   └── useStreamingChat.ts
├── services/
│   └── gptService.ts
├── api/
│   └── postMessage.ts
├── components/
│   ├── ChatInput.tsx
│   └── MessageList.tsx
```

You avoid shared state bleed by:

* Keeping logic in `hooks/` instead of dumping it into components
* Wrapping API calls in `api/`, not `components/`
* Using `context/` if you need local state across deeply nested components

Each module becomes **predictable** and **testable.**

---

### Naming Conventions for Modular Files

Consistency matters—especially as you scale. Here are suggestions:

| Pattern                         | Example                                 |
| ------------------------------- | --------------------------------------- |
| Hook prefix                     | `useChatSession.ts`                     |
| Test suffix                     | `ChatInput.test.tsx`                    |
| Service suffix                  | `gptService.ts`                         |
| Component file = component name | `ChatInput.tsx`                         |
| API files as verbs              | `postMessage.ts`, `getConversations.ts` |

Avoid vague names like `utils.ts`, `index.ts`, or `helpers.ts` unless scoped with clear purpose.

---

### Keeping Tests Close to Features

React modular structure benefits from **co-located testing**:

* Put `__tests__/` inside each feature folder
* Use `jest`, `vitest`, or `playwright` with file pattern targeting
* Name tests after their targets (e.g., `ChatInput.test.tsx`)

This gives you:

* Fast local iteration
* Easy mocking of feature-local services
* Clean CI targeting: test only changed modules

---

### Avoiding Shared State Bleed

One common pitfall in modular React apps: **global state leakage**.

Symptoms:

* Features accidentally rely on global context
* Changes in one feature affect others
* Debugging becomes non-local

Solutions:

* Prefer **feature-local state** via hooks and `useReducer`
* Use **React Context only for true cross-feature concerns** (e.g., user session)
* Use state managers like **Zustand or Jotai** to keep scoped atoms

Structure should make **boundaries explicit**.

---

### Summary: React Modularity Best Practices

| Principle           | Practice                                                |
| ------------------- | ------------------------------------------------------- |
| Feature-first       | Create folders per domain (`chatbot/`, `invoice/`)      |
| Component isolation | Only share what’s reused                                |
| Localize logic      | Use `hooks/`, `services/`, `api/` per feature           |
| Co-locate tests     | `__tests__/` live beside logic                          |
| Share intentionally | Extract to `shared/` only when duplication becomes real |

Modular React apps scale better not just in code—but in team velocity.

Next: we apply the same mindset to the **backend side with FastAPI**—where modular services, schemas, and routes need their own design principles.

---