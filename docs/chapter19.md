---
hide:
    - toc
---

## Part 5: Case Studies and Templates

# 19. Case Study: Smart Receipt / Invoice Analyzer

> “OCR is messy. Accounting rules are worse.  
> But structure makes sense of both.”

---

### Overview

The **Smart Receipt/Invoice Analyzer** is a full-stack AI application that:

* Accepts images or PDFs of receipts and invoices
* Performs OCR (Optical Character Recognition)
* Extracts structured fields (merchant, total, date, tax)
* Parses text into key-value data using GPT
* Allows frontend edits + corrections
* Stores parsed data in a database

This is a perfect case to demonstrate:

* Modular architecture for isolated AI pipelines
* Clean foldering for OCR, GPT, and DB services
* How to evolve from modular → hybrid structure

---

## Initial Modular Structure

We began with a **feature-first modular approach**.

### Frontend (React + Vite)

```bash
frontend/
└── src/
    └── features/
        └── invoice/
            ├── components/
            │   ├── UploadForm.tsx
            │   └── ParsedPreview.tsx
            ├── hooks/
            │   └── useUpload.ts
            ├── services/
            │   └── invoiceApi.ts
            └── types/
                └── InvoiceFields.ts
```

### Backend (FastAPI)

```bash
backend/
└── app/
    └── features/
        └── invoice/
            ├── routes.py
            ├── services/
            │   ├── ocr_service.py
            │   └── gpt_parser.py
            ├── schemas/
            │   └── invoice.py
            └── models.py
```

Each feature folder handled everything it needed:

* **Frontend:** form, preview, upload logic
* **Backend:** OCR, parsing, validation, and response shaping

---

## Evolution to Hybrid Structure

Once we added:

* **Chatbot module** that also needed OCR
* **Document classification** logic for PDFs

We extracted shared logic into **global services**.

### New Shared Backend Structure

```bash
backend/
├── app/
│   ├── features/
│   │   └── invoice/
│   ├── services/
│   │   ├── ocr_engine.py         # Shared OCR logic (Tesseract, PaddleOCR)
│   │   ├── gpt_client.py         # Central GPT communication layer
│   │   └── file_utils.py
│   └── schemas/
│       └── shared.py
```

Now:

* Any feature can call `ocr_engine.extract_text(file)`
* GPT prompts are templated and reused across modules

---

## Testing Breakdown

### Unit Tests

```bash
tests/
└── invoice/
    ├── test_parser.py
    ├── test_ocr.py
    └── test_routes.py
```

### Mocks

```bash
shared/test_helpers/
├── fake_invoice_image.png
└── mock_gpt.py
```

Mocking GPT + OCR was essential to keep test speed fast and predictable.

---

## Tech Stack Snapshot

| Layer      | Tool                                     |
| ---------- | ---------------------------------------- |
| OCR        | Tesseract / PaddleOCR                    |
| LLM        | OpenAI GPT-3.5 / GPT-4                   |
| Backend    | FastAPI                                  |
| Frontend   | React + Vite                             |
| State      | React Context                            |
| Deployment | Railway + Vercel                         |
| DB         | Supabase (PostgreSQL)                    |
| Auth       | JWT (Backend) + Supabase Auth (Frontend) |

---

## Key Lessons

| Challenge                                 | Resolution                                              |
| ----------------------------------------- | ------------------------------------------------------- |
| GPT prompts reused in multiple modules    | Moved prompt logic to `services/gpt_client.py`          |
| OCR used across invoice/chatbot features  | Created `ocr_engine.py` in shared service layer         |
| Uploads had differing file handling logic | Created `file_utils.py` with unified image/PDF handlers |
| Type duplication between front and back   | Introduced `packages/shared-schemas/` in monorepo       |
| Components growing too large              | Split into `UploadForm`, `FieldPreview`, `ErrorBanner`  |
| Test bloat in one location                | Adopted hybrid test structure (feature + root tests)    |

---

## Final Hybrid Folder Snapshot

### Backend:

```bash
app/
├── features/
│   └── invoice/
│       ├── routes.py
│       ├── services/
│       └── schemas/
├── services/
│   ├── gpt_client.py
│   ├── ocr_engine.py
│   └── file_utils.py
├── schemas/
│   └── shared.py
```

### Frontend:

```bash
src/
├── features/
│   └── invoice/
│       ├── components/
│       ├── services/
│       └── hooks/
├── shared/
│   └── utils/
│       └── filePreview.ts
```

---

## Project Outcomes

* 100+ invoices processed in a demo database
* Accurate OCR + GPT parsing of total/tax/date in 90% of test cases
* Scalable architecture allowed new features (chatbot, classifier) to be added without breaking invoice logic
* Deployed to Vercel + Railway with monorepo CI/CD

---

## What This Case Proves

This case demonstrates:

✅ How to bootstrap modularly with minimal complexity  
✅ When and how to extract shared AI logic (OCR, GPT)  
✅ How to support multi-feature AI pipelines in a clean structure  
✅ The power of hybrid folder strategies for AI-driven apps

---