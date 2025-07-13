---
hide:
    - toc
---

## Part 5: Case Studies and Templates

# 20. Case Study: Document Intelligence Chatbot

> “You don't just answer questions.
> You reason across pages, paragraphs, and payloads.”

---

### Overview

The **Document Intelligence Chatbot** is a full-stack RAG-powered AI assistant that allows users to:

* Upload documents (PDFs, images, DOCX)
* Run OCR (if needed) on non-text files
* Split and embed text into a vector database
* Query via a chatbot interface using GPT
* Retrieve, synthesize, and stream document-aware answers

This project sits at the intersection of:

* NLP, embeddings, and chunking
* File handling and background processing
* Realtime chat UI and long-context memory
* Vector DBs and API orchestration

---

## Initial Scalable Structure (from day one)

Unlike the Invoice Analyzer, this project **started with scale in mind**.

### Backend (FastAPI)

```bash
backend/
├── app/
│   ├── api/
│   │   ├── chat.py
│   │   └── upload.py
│   ├── services/
│   │   ├── ocr_engine.py
│   │   ├── gpt_client.py
│   │   ├── chunker.py
│   │   ├── embedding.py
│   │   └── retriever.py
│   ├── schemas/
│   │   ├── chat.py
│   │   └── document.py
│   ├── vectorstore/
│   │   └── supabase.py
│   └── core/
│       ├── config.py
│       └── logging.py
```

### Frontend (React + Vite)

```bash
frontend/
└── src/
    ├── features/
    │   ├── chat/
    │   └── upload/
    ├── components/
    │   ├── ChatBox.tsx
    │   ├── FileDropZone.tsx
    │   └── StreamingBubble.tsx
    ├── services/
    │   └── apiClient.ts
    ├── hooks/
    │   └── useStreamingChat.ts
    └── shared/
        └── utils/
            └── chunkPreview.ts
```

---

## Core AI Flow

```text
[Upload Document]
    ↓
OCR (if needed)
    ↓
Text Chunking
    ↓
Embeddings → Supabase (pgvector)
    ↓
[Ask Question]
    ↓
Query Vector DB
    ↓
Pass relevant chunks to GPT
    ↓
Stream answer token-by-token
    ↓
[Render Chat UI]
```

---

## Testing Breakdown

### Unit Tests

```bash
tests/
├── services/
│   ├── test_ocr_engine.py
│   ├── test_chunker.py
│   └── test_embedding.py
```

### Integration Tests

```bash
tests/
├── api/
│   ├── test_chat.py
│   └── test_upload.py
```

### Mocks

```bash
shared/test_helpers/
├── mock_pdf.pdf
├── fake_embeddings.json
└── mock_gpt_response.py
```

---

## Architectural Wins

| Problem                            | Solution                                                        |
| ---------------------------------- | --------------------------------------------------------------- |
| Mixing OCR and GPT in one pipeline | Broke into `ocr_engine.py` and `gpt_client.py`                  |
| Need for streaming chat responses  | Used FastAPI’s `StreamingResponse` with async token yield       |
| Scattered vector DB logic          | Moved to `vectorstore/supabase.py` as a wrapper                 |
| Complex GPT prompts per feature    | Template-driven prompts in `prompts/` or inline Jinja           |
| Upload race conditions             | Background task queue (e.g., Celery or FastAPI background task) |
| Slow test speed due to real GPT    | Replaced with `mock_gpt_response.py` + snapshot tests           |

---

## Deployment Stack

| Layer           | Tool                               |
| --------------- | ---------------------------------- |
| Backend         | FastAPI (hosted on Render)         |
| Frontend        | React + Vite (deployed to Netlify) |
| Chat LLM        | OpenAI GPT-4 via streaming         |
| Vector DB       | Supabase with `pgvector`           |
| Storage         | Supabase Buckets                   |
| OCR             | PaddleOCR (fallback to Tesseract)  |
| Embedding Model | `text-embedding-3-small`           |
| CI/CD           | GitHub Actions + PR checks         |

---

## Dev Experience Enhancements

* Local `.env` loaded via `dotenv` with fallback to Render secrets
* Tailwind UI for fast design of chat & file upload zones
* Monorepo setup using `pnpm workspaces` with:

```bash
  apps/
    ├── frontend/
    └── backend/
  packages/
    └── shared-schemas/
```

---

## Edge Case Handlings

| Edge Case                      | Resolution                                               |
| ------------------------------ | -------------------------------------------------------- |
| PDF upload with no text layer  | OCR fallback with PaddleOCR                              |
| Image uploads with handwriting | Post-OCR cleanup with regex correction                   |
| GPT context overflow           | Auto-summarization of long chunks                        |
| Broken token stream            | Frontend `AbortController` with retry                    |
| Multi-document queries         | Multi-file embedding tracking with `document_id` tagging |

---

## Final Structure Snapshot

### Backend:

```bash
app/
├── api/
│   └── chat.py
├── services/
│   ├── ocr_engine.py
│   ├── embedding.py
│   └── retriever.py
├── vectorstore/
│   └── supabase.py
├── schemas/
│   └── chat.py
├── prompts/
│   └── rag_template.txt
```

### Frontend:

```bash
src/
├── features/
│   ├── upload/
│   └── chat/
├── components/
│   ├── ChatBox.tsx
│   └── StreamingBubble.tsx
├── hooks/
│   └── useStreamingChat.ts
```

---

## Outcomes

* Real-time chat performance with \~2s latency
* GPT answers grounded in actual user documents
* 92%+ retrieval accuracy via `text-embedding-3-small`
* Horizontal scalability with Render’s autoscaling + pgvector indexing
* Successfully demoed for 3 different use cases (contracts, academic PDFs, invoices)

---

## Key Takeaways

✅ Build scalable **from the start** if the product involves multiple AI pipelines  
✅ Use **dedicated service layers** to isolate OCR, GPT, chunking, and vector logic  
✅ Design frontend **UX-first** to support streaming and async behaviors  
✅ Leverage **Supabase + FastAPI** for full open-source vertical integration  
✅ Embrace **modular tests, mocks, and monitoring** as part of the structure

---