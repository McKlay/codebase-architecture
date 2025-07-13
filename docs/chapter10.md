---
hide:
    - toc
---

## Part 2: Modularity

# 10. Testing in Modular Projects

> “A module without a test is a feature waiting to break.”

---

### Why Testing Should Be Modular, Too

You can’t have truly modular code unless your tests are modular as well.

Testing isn't just about coverage numbers or CI checks. It's about **local confidence**:

* Can I safely refactor this GPT parser?
* Will changes in `upload/` break `chatbot/`?
* Can a new dev test this module in isolation?

In modular projects, your tests should **mirror the folder structure** of your features—unit by unit, hook by hook, service by service.

---

### Testing Patterns for Modular Projects

Let’s break testing into four practical layers:

| Type                  | Goal                                        | Where it lives                                 |
| --------------------- | ------------------------------------------- | ---------------------------------------------- |
| **Unit Tests**        | Validate a single function/component        | Inside `__tests__/` per feature                |
| **Integration Tests** | Validate interaction between internal parts | Inside `__tests__/` or root `tests/`           |
| **API Tests**         | Hit real or mock endpoints                  | Root `tests/` or Postman/Playwright collection |
| **E2E Tests**         | Simulate real user flows                    | External repo or `/e2e/` subfolder             |

---

### Recommended Folder Structure for Tests

We recommend the **hybrid approach**:

```bash
project/
├── frontend/
│   └── src/
│       ├── chatbot/
│       │   ├── components/
│       │   └── __tests__/
│       └── shared/
│           └── test_helpers/
├── backend/
│   └── app/
│       ├── invoice/
│       │   ├── services/
│       │   └── __tests__/
│       └── shared/
│           └── test_helpers/
├── tests/             # (root-level)
│   ├── test_api_chatbot.py     # Integration/API tests
│   └── test_upload_flow.py     # Cross-feature behavior
```

This gives you **local testability + global traceability**.

---

### React: Writing Tests Per Module

Use tools like `vitest`, `jest`, or `react-testing-library`.

#### Inside a feature module:

```
chatbot/
├── components/
│   └── ChatInput.tsx
├── __tests__/
│   └── ChatInput.test.tsx
```

```tsx
// ChatInput.test.tsx
import { render, screen } from "@testing-library/react";
import ChatInput from "../components/ChatInput";

test("renders input box", () => {
  render(<ChatInput />);
  expect(screen.getByPlaceholderText("Ask something...")).toBeInTheDocument();
});
```

#### Shared test helpers:

```bash
shared/test_helpers/
├── mockSession.ts
├── testWrapper.tsx
```

Use these to DRY up test scaffolding, especially for:

* Auth context
* Global providers
* Routing mocks

---

### FastAPI: Writing Tests Per Feature

Use `pytest`, `httpx.AsyncClient`, and FastAPI’s `TestClient`.

#### Example: Testing GPT logic in `chatbot/`

```bash
chatbot/
├── services/
│   └── chat_service.py
├── __tests__/
│   └── test_chat_service.py
```

```python
from chatbot.services.chat_service import stream_response

def test_streaming_chunks_return_data():
    prompt = {"message": "Hello"}
    chunks = list(stream_response(prompt))
    assert len(chunks) > 0
```

You can also **mock GPT** by injecting a fake service:

```python
# shared/test_helpers/mocks.py
class FakeGPT:
    def complete(self, prompt):
        return "This is a mock response"
```

---

### Keeping Modules Independently Testable

Key goals:

* Each module runs its own tests independently
* No test reaches into another feature's internals
* Shared mocks and utilities are reused (but not over-abstracted)

Modular testability ensures:

* You can deploy or validate a feature in isolation
* CI/CD pipelines can run **partial tests on changed modules only**
* Refactoring is safe and scoped

---

### Mocking Strategies

| Target                      | Strategy                                                  |
| --------------------------- | --------------------------------------------------------- |
| **GPT, OCR, External APIs** | Fake classes in `shared/test_helpers/`                    |
| **FastAPI dependencies**    | Use `Depends()` and override in tests                     |
| **React context / stores**  | Wrap with custom `TestProvider.tsx`                       |
| **Hooks**                   | Mock inside `jest.mock()` or inject dependencies manually |

Your tests should not rely on real API keys, internet calls, or production databases.

---

### Toolchain Recommendations

| Stack     | Suggested Tools                                                     |
| --------- | ------------------------------------------------------------------- |
| React     | Vitest, React Testing Library, Playwright (E2E)                     |
| FastAPI   | Pytest, HTTPX, TestClient, Faker                                    |
| Fullstack | Playwright, Postman/Newman, Docker Compose for local testing        |
| CI/CD     | GitHub Actions + `pytest`/`vitest` runners with coverage thresholds |

---

### Automating Modular Test Discovery in CI

To speed up CI:

1. Detect changed paths using `git diff`
2. Map changed folders to test scopes:

   * `frontend/src/chatbot/` → run `vitest` only on `chatbot/`
   * `backend/app/invoice/` → run `pytest` on `invoice/__tests__/`

Optional: Use `tox`, `pytest-split`, or Nx if in a monorepo.

---

### Summary: Modular Testing Principles

| Principle                 | Practice                                                   |
| ------------------------- | ---------------------------------------------------------- |
| Local testing per module  | Place `__tests__/` inside each feature folder              |
| Shared mock reuse         | Store fake services and wrappers in `shared/test_helpers/` |
| No cross-feature imports  | Each test should stay inside its domain                    |
| API and integration tests | Put in root `tests/` folder or external suite              |
| CI optimization           | Detect changes and test only affected modules              |

---


