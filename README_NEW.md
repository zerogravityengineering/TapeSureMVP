
# TapSure Agentic MVP

---
**© 2026 Zero Gravity Engineering (Pty) Ltd. All rights reserved worldwide.**

**Chief Architect & Author:** Zebbediah Winston Beck
**Genesis Core Team:** Founding contributors

**NOTICE:**
All code, designs, and documentation in this repository are the exclusive property of Zero Gravity Engineering (Pty) Ltd. No changes, contributions, or derivative works may be made—even by Genesis Core Team or founding members—unless a Non-Disclosure Agreement (NDA) is signed and approved by the Steering Committee of Zero Gravity Engineering (Pty) Ltd. This policy is binding for all contributors, including core engineers, and applies to all current and future contributions. Any attempt to modify, use, or disclose any part of this application without explicit written consent is strictly prohibited.

**Contributors may only join this project by invitation and after signing an NDA.**

---

An event-driven multi-agent insurance MVP with intelligent chatbot. Upload a receipt, chat about purchases, get instant coverage recommendations, and confirm protection—all with code-gated access.

## 🆕 Features

### 🔐 Code-Gated Access (NEW)
Users must enter a valid **access code** before uploading receipts or accessing coverage features.

**Valid Access Codes:**
- `TAPE-2025-PROMO`
- `INSURE-NOW-2025`
- `COVER-2025-SALE`
- `PROTECT-BUNDLE`

### 💬 Intelligent Chatbot (NEW)
The chatbot now understands **specific phrases** and asks **structured questions**:

**Phrases the Chatbot Recognizes:**
- **Greeting:** "Hi", "Hello", "Hey", "Start", "Welcome"
- **Help:** "Help", "Assist", "Support", "What can you do"
- **Upload:** "Upload", "Receipt", "Photo", "Picture", "Scan"
- **Purchase:** "Bought", "Spent", "Paid", "Cost", "Price"
- **Coverage:** "Coverage", "Protection", "Plan", "Insurance"
- **Selection:** "Option", "Choose", "Pick", "Want", "Take"
- **Confirm:** "Confirm", "Yes", "Done", "Great", "Perfect"
- **Features:** "Damage", "Theft", "Accidental", "Covered"
- **Process:** "How", "Works", "Steps", "What next"

**Questions the Bot Asks & Understands:**
1. Would you like to upload a receipt photo?
2. Or would you prefer to tell me about your purchase?
3. Do you want to know about our coverage options first?
4. What category is your purchase: Electronics, Grocery, Clothing, or Pharmacy?
5. How much did you spend on this item?
6. Which coverage plan interests you: 6-month, 12-month, or 24-month?
7. Would you like to activate your coverage now?
8. Do you have another item you'd like to protect?

**Example Conversations:**
```
User: "Hi"
Bot: "👋 Welcome to TapSure! Here are questions you can ask me..." [Lists 8 questions]

User: "I bought a laptop for $1500"
Bot: "✅ Perfect! I understand: Item Type: Electronics, Purchase Price: $1500.00. Here are your coverage options..."

User: "Option 1"
Bot: "🎉 Excellent choice! Activating your coverage plan..."

User: "How does it work?"
Bot: "⚙️ Here's how TapSure works: Step 1: Upload a receipt... Step 5: Your policy is instantly active!"

User: "Tell me about coverage"
Bot: "🛡️ TapSure Coverage Includes: 📍 Damage, 🚨 Theft, ⚡ Accidental, 💧 Liquid..."
```

## Stack

- **Backend:** FastAPI + multi-agent modules (OpenAI optional)
- **Frontend:** Vanilla HTML/CSS/JS (mobile-first, Tesseract.js for OCR)
- **Hosting:** Netlify (serverless static frontend)

## Quick Start

### Option 1: Serverless (No Backend Required - Recommended for Testing)

```bash
# Start the frontend-only server on port 8082
cd /path/to/tapsure1
python3 -m http.server 8082 -d netlify-build
```

Then open: **http://localhost:8082**

Features:
- ✅ 100% serverless (runs entirely in browser)
- ✅ In-browser OCR via Tesseract.js
- ✅ Intelligent chatbot with 8 structured questions
- ✅ Code-gated access (enter: `TAPE-2025-PROMO`)
- ✅ Coverage recommendations & policy generation
- ✅ No backend API required

### Option 2: Full Stack (Backend + Frontend)

#### Backend Setup

```bash
# Create virtual environment
python3 -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\Activate.ps1

# Install dependencies
pip install -r backend/requirements.txt

# Configure environment (optional - for LLM features)
cp backend/.env.example backend/.env
# Set OPENAI_API_KEY if using OpenAI, or configure Ollama below

# Start the API
PYTHONPATH=$(pwd)/backend .venv/bin/python -m uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

#### Frontend Setup

```bash
# In a new terminal, start the static server
python3 -m http.server 8082 -d netlify-build
```

Then open: **http://localhost:8082**

## Key API Endpoints

- `POST /api/receipt/analyze` — Analyze receipt image (multipart)
- `POST /api/coverage/recommend` — Get coverage options (JSON payload)
- `POST /api/flow/confirm` — Confirm and activate policy (JSON selection)
- `POST /api/chat` — Chat with the conversational agent (JSON {message})
- `GET /health` — Health check

## Authentication & Access Codes

The app uses a **code-gated access system**. Users must enter one of these codes on page load:

```
TAPE-2025-PROMO
INSURE-NOW-2025
COVER-2025-SALE
PROTECT-BUNDLE
```

To modify valid codes, edit `netlify-build/assets/app.js`:
```javascript
const VALID_CODES = ['TAPE-2025-PROMO', 'INSURE-NOW-2025', 'COVER-2025-SALE', 'PROTECT-BUNDLE'];
```

## Coverage Categories & Pricing

The chatbot automatically recognizes purchase categories and provides 3 coverage tiers:

| Category | Base Rate | 6-Month (0.45x) | 12-Month (0.75x) | 24-Month (1.1x) |
|----------|-----------|-----------------|------------------|-----------------|
| Electronics | $45 | $20.25 | $33.75 | $49.50 |
| Grocery | $15 | $6.75 | $11.25 | $16.50 |
| Clothing | $25 | $11.25 | $18.75 | $27.50 |
| Pharmacy | $10 | $4.50 | $7.50 | $11.00 |

**Price adjustments:** Premium scales with purchase amount. For example:
- Bought a $1500 laptop → Electronics rate × (1500/100) = $45 × 15 = $675/month base
- 6-month plan: $675 × 0.45 = $303.75/month

## Configuration

### Environment Variables (Optional - for LLM/Backend features)

```bash
# LLM Configuration
LLM_BASE_URL=http://localhost:11434/v1  # For Ollama or OpenAI-compatible API
LLM_API_KEY=your-api-key                # Leave empty for Ollama
LLM_MODEL=llama3.1:8b                   # Model name

# Or use OpenAI directly
OPENAI_API_KEY=sk-...

# OCR Configuration (Backend)
TESSERACT_CMD=C:\Program Files\Tesseract-OCR\tesseract.exe  # Windows only
```

### Free Setup (No Paid APIs)

1. **Install Tesseract OCR** (for backend OCR):
   - Download: https://github.com/tesseract-ocr/tesseract
   - Set `TESSERACT_CMD` in `backend/.env`

2. **Install Ollama** (for local LLM):
   - Download: https://ollama.com/download
   - Pull a model: `ollama pull llama3.1:8b`
   - Set `LLM_BASE_URL=http://localhost:11434/v1`

3. **Configure backend/.env:**
   ```
   LLM_BASE_URL=http://localhost:11434/v1
   LLM_API_KEY=ollama
   LLM_MODEL=llama3.1:8b
   ```

## Deployment

### Deploy to Netlify (Static Frontend Only)

```bash
# Install Netlify CLI (requires Node.js >=20)
npm install -g netlify-cli

# Authenticate
netlify login

# Deploy
netlify deploy --dir=netlify-build --prod
```

Your app will be live at: `https://your-app.netlify.app`

## Architecture

- **Orchestrator** — Coordinates agent workflows (receipt analysis, coverage recommendation, policy confirmation)
- **ReceiptAnalyzer** — Uses Tesseract.js (browser) or pytesseract (backend) for OCR
- **CoverageRecommender** — Deterministic pricing engine (no external calls)
- **ConversationalAgent** — Intelligent chatbot with 8 structured questions and phrase recognition

## 🎯 What's New (Latest Updates)

✅ **Code-gated access** — 4 valid promotional codes required on page load
✅ **Intelligent chatbot** — Recognizes 9 phrase categories and 8 structured questions
✅ **Natural language parsing** — Understands purchase descriptions ("I bought a MacBook for $1500")
✅ **WhatsApp-style chat UI** — Message bubbles, proper formatting, emoji indicators
✅ **Coverage tier recommendations** — 6/12/24-month plans with automatic pricing
✅ **Policy generation** — Unique policy IDs with confirmation details
✅ **Serverless deployment** — 100% client-side, runs on Netlify without backend
✅ **Browser-based OCR** — Tesseract.js in-browser image recognition

## Notes

- The app runs **100% serverless** by default (no backend required)
- OCR happens in the browser using **Tesseract.js** (free, open-source)
- All chat logic is client-side (pure JavaScript)
- Optional backend provides advanced LLM-powered features
- Code-gated access prevents unauthorized uploads
- Policies are generated with unique IDs and can be extended with backend persistence

## Troubleshooting

- **"Code modal won't go away"** — Enter a valid code from the list above
- **"OCR not working"** — Tesseract.js requires a few seconds on first load; ensure image is clear
- **"Backend not reachable"** — The app automatically falls back to 100% client-side mode
- **"Port already in use"** — Try a different port: `python3 -m http.server 8083 -d netlify-build`
- **"Chatbot not understanding me"** — Try phrases from the "Phrases the Chatbot Recognizes" section above
