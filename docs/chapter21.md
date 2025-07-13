---
hide:
    - toc
---

## Part 5: Case Studies and Templates

# 21. Case Study: AI-Powered Mockup-to-Code Tool

> “A mockup isn’t just a picture.  
> It’s a latent interface waiting to be decoded.”

---

### Overview

The **AI-Powered Mockup-to-Code Tool** is a frontend-first AI application that transforms UI mockups (Figma exports, PNGs, screenshots) into structured HTML/CSS or React components.

The pipeline involves:

* Uploading a mockup image
* Performing layout analysis using vision models
* Extracting UI elements and classifying them (buttons, input fields, headers, etc.)
* Generating code snippets (React or Tailwind-based) via GPT
* Displaying a live preview and downloadable scaffold

This project demonstrates:

✅ Cross-domain AI integration (CV + LLM)  
✅ Highly interactive frontend logic  
✅ Modular and layered backend orchestration  
✅ Clear separation of vision pipeline and code generation

---

## Initial Modular Structure

The project started **modular** for experimentation, especially for isolating layout detection and prompt engineering.

### Frontend (React + Vite)

```bash
src/
└── features/
    ├── upload/
    ├── preview/
    └── codegen/
```

### Backend (FastAPI)

```bash
app/
└── features/
    ├── layout/
    ├── codegen/
    └── upload/
```

---

## Final Scalable Hybrid Structure

As accuracy and user demand increased, shared services emerged across modules, triggering a hybrid structure shift.

### Backend:

```bash
app/
├── api/
│   ├── upload.py
│   ├── layout.py
│   └── codegen.py
├── services/
│   ├── image_processing.py
│   ├── layout_detector.py
│   ├── gpt_prompt_builder.py
│   └── html_generator.py
├── schemas/
│   ├── layout.py
│   └── codegen.py
├── core/
│   └── config.py
```

### Frontend:

```bash
src/
├── components/
│   ├── MockupCanvas.tsx
│   ├── LayoutBoundingBox.tsx
│   └── CodePreview.tsx
├── services/
│   └── codegenClient.ts
├── hooks/
│   └── useCodeGeneration.ts
├── features/
│   ├── upload/
│   ├── detect/
│   └── render/
```

---

## Core AI Flow

```text
[Image Upload]
    ↓
CV Layout Detection (YOLO or Detectron2)
    ↓
Component Classification (headers, buttons, divs)
    ↓
Prompt Assembly → GPT-4
    ↓
Code Output (React + Tailwind / HTML + CSS)
    ↓
[Live Preview + Download]
```

---

## CV + GPT Model Integration

| Step                           | Tech                                            |
| ------------------------------ | ----------------------------------------------- |
| Object detection               | YOLOv8 or Detectron2 (trained on UIs)           |
| Component label classification | Custom CV classifier                            |
| Prompt injection               | GPT-4 via OpenAI API                            |
| Code formatting                | Prettier (client-side)                          |
| Model inference                | Hosted on Replicate (for YOLO) or local PyTorch |

---

## Testing Approach

| Layer                    | Strategy                                         |
| ------------------------ | ------------------------------------------------ |
| Layout detector          | Snapshot-based CV test (bounding boxes)          |
| GPT prompt               | Deterministic unit tests with static inputs      |
| Frontend                 | Visual regression tests (Storybook + Playwright) |
| Upload/codegen endpoints | End-to-end integration with sample assets        |

---

## Key Problems & Fixes

| Problem                              | Resolution                                                 |
| ------------------------------------ | ---------------------------------------------------------- |
| LLM-generated code sometimes invalid | Added static HTML validator + auto-fix layer               |
| Misclassified layout regions         | Introduced confidence thresholding + post-processing rules |
| Slow image inference on render       | Added async loading + placeholder thumbnails               |
| GPT prompt bloated for large mockups | Chunked component zones and stitched final code            |
| Loss of user layout intent           | Allowed user to manually adjust box types before codegen   |

---

## Frontend Interaction Flow

1. User uploads mockup image  
2. Bounding boxes appear with labels (adjustable)  
3. User selects preferred framework (HTML/CSS or React/Tailwind)  
4. “Generate Code” triggers GPT prompt call  
5. Code appears in preview pane + download button enabled

---

## Tech Stack Summary

| Layer        | Tool                              |
| ------------ | --------------------------------- |
| CV Inference | YOLOv8 (Replicate or local Torch) |
| GPT Codegen  | GPT-4 (OpenAI API)                |
| Frontend     | React + Tailwind CSS              |
| State        | Zustand (for image + code state)  |
| Formatter    | Prettier (optional: ESLint)       |
| Deployment   | Netlify + Railway                 |
| Previews     | Monaco Editor + HTML live iframe  |

---

## Outcomes

* Converted mockups to usable React code in <15 seconds
* Allowed manual overrides on labels before GPT input
* Reduced layout hallucination via hybrid prompt template
* Supported 2 output modes: **HTML/CSS** and **React + Tailwind**
* Used by junior frontend devs as a scaffolding tool

---

## What This Case Demonstrates

✅ Vision pipeline and GPT codegen **must be decoupled**  
✅ Hybrid prompting is more stable than pure freeform GPT  
✅ Component previewing and user correction is vital  
✅ Modular frontend separation (upload / detect / render) improves UX  
✅ Shared CV and GPT logic should live in `services/` not features/  

---
