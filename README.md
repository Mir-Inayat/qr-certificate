# QR Certificate — Verifiable Certificate Generation & Distribution Platform

> A full-stack platform to generate, distribute, and verify QR-coded certificates at scale — built for CBIT Open Source Community events and workshops.

![TypeScript](https://img.shields.io/badge/TypeScript-0d1117?style=flat-square&logo=typescript&logoColor=3178c6)
![Next.js](https://img.shields.io/badge/Next.js-0d1117?style=flat-square&logo=nextdotjs&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-0d1117?style=flat-square&logo=fastapi&logoColor=00d9a3)
![Python](https://img.shields.io/badge/Python-0d1117?style=flat-square&logo=python&logoColor=58a6ff)
![GitHub Pages](https://img.shields.io/badge/GitHub_Pages-0d1117?style=flat-square&logo=github&logoColor=white)

---

## What it does

QR Certificate is a full-stack certificate generation and verification platform. You upload a certificate template image + an Excel sheet of participant names — it generates individual PNG or PDF certificates with embedded QR codes, builds a verification site, and deploys it to a GitHub Pages repo. Anyone who scans a QR code lands on the hosted verification page and sees the authentic certificate.

**Used at CBIT COSC events:** Git/GitHub workshops, hackathons, and tech fests with 1000+ participants.

---

## End-to-End Pipeline

```
┌────────────────────────────────────────────────┐
│  Next.js Frontend (Design Studio)              │
│  └─ Upload template PNG + Excel (.xlsx)         │
│  └─ Visually position: name, QR, custom fields  │
│  └─ Set base URL, event title, serial prefix    │
└────────────────────────────────────────────────┘
                        ↓ POST /api/generate-certificates
┌────────────────────────────────────────────────┐
│  FastAPI Backend (src/api/index.py)             │
│                                                │
│  For each row in Excel:                        │
│   1. Generate unique code:                     │
│      {name}{serial}{index} → e.g. john001     │
│   2. Build QR URL: base_url + code             │
│   3. Generate QR PNG (qrcode lib)              │
│   4. Overlay name + QR on template (Pillow)    │
│   5. Save as {Name}.png or {Name}.pdf          │
│   6. Inject overlays into SVG template         │
│   7. Write docs/: index.html, script.js,       │
│      style.css, data.json                      │
└────────────────────────────────────────────────┘
                        ↓
┌────────────────────────────────────────────────┐
│  Output                                        │
│  ├── certificates/    ← PNG / PDF per person   │
│  └── docs/                                     │
│       └─ {event-folder}/                        │
│           ├─ index.html   (verification page)   │
│           ├─ data.json    (all cert codes)       │
│           ├─ script.js    (QR lookup logic)      │
│           ├─ style.css                           │
│           └─ fonts/                              │
└────────────────────────────────────────────────┘
                        ↓ POST /api/deploy-github
┌────────────────────────────────────────────────┐
│  GitHub Pages (Verification Hosting)           │
│  PyGithub pushes docs/ folder to target repo   │
│                                                │
│  QR code scan → ?id=john001                    │
│  script.js fetches data.json                   │
│  Matches code → renders SVG certificate        │
│  Displays: name, date, live QR code            │
└────────────────────────────────────────────────┘
                        ↓ (optional)
┌────────────────────────────────────────────────┐
│  Email Distribution (Gmail API)                │
│  POST /api/send-emails                         │
│  Streams SSE → per-recipient send status       │
│  Attaches certificate PNG/PDF per person       │
└────────────────────────────────────────────────┘
```

---

## Features

- **Visual design studio** — Next.js UI to position name, QR, and custom text fields on the certificate template
- **Bulk generation** — reads Excel sheet, generates one certificate per row as PNG or PDF
- **Unique verifiable codes** — each certificate gets a code like `johndoe-COSC-0001`
- **Auto-built verification site** — generates `index.html` + `data.json` + `script.js` for each batch
- **One-click GitHub deploy** — pushes the `docs/` folder to any GitHub repo via PyGitHub API; verification page is live on GitHub Pages instantly
- **QR scan verification** — scanning the QR code opens `?id=johndoe-COSC-0001`, script.js looks up `data.json`, renders the SVG certificate live
- **Email distribution** — sends certificates individually via Gmail API with SSE streaming for real-time progress
- **Custom fonts** — upload TTF fonts per text overlay
- **SVG + PNG output** — both formats supported

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js 14 + TypeScript + Tailwind CSS |
| Backend API | FastAPI (Python) |
| Certificate rendering | Pillow (PIL), lxml (SVG) |
| QR generation | `qrcode` Python library |
| Data input | pandas + openpyxl (Excel parsing) |
| GitHub deploy | PyGitHub API |
| Email | Gmail API (Google OAuth) |
| Verification hosting | GitHub Pages |

---

## Project Structure

```
.
├── src/
│   ├── api/
│   │   └── index.py          # FastAPI backend: generate, deploy, email
│   ├── app/
│   │   ├── generate/         # Certificate generation flow
│   │   ├── distribute/       # Email distribution flow
│   │   └── howto/            # Usage guide
│   └── components/           # Reusable UI components
├── requirements.txt          # Python dependencies
└── package.json              # Next.js dependencies
```

---

## Setup

```bash
# Frontend
npm install
npm run dev

# Backend
pip install -r requirements.txt
uvicorn src.api.index:app --reload --port 8000
```

For GitHub deploy, provide a personal access token with `repo` scope when prompted in the UI.
