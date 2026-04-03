# Lauder AI - AI verson of Mr. Lauder

An AI version of Mr Lauder that can give you personalized feedback on your presentation, and help you cook it!! 

It is an AI-powered feedback tool that provides feedback on presentation an gives life advice, built around a fictional teacher persona. Students can ask questions or submit code for chat-based review or upload a presentation for slide-by-slide analysis — all in the voice of **Mr. Lauder**.

---

## Features

- **Chat feedback** — submit code snippets or personalized question and receive structured, educational, inspiring responses in real time
- **Slide review** — upload a `.pptx` presentation and get card-based feedback on every slide, plus an overall summary and a inspiring closing question
- **No framework overhead** — vanilla HTML, CSS, and ES Module JavaScript; no build step required

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML, CSS, JavaScript (ES Modules) |
| Server | Node.js + Express |
| File uploads | multer |
| PPTX parsing | jszip |
| AI — chat | Groq API · `llama-3.1-8b-instant` |
| AI — slide review | Groq API · `llama-3.3-70b-versatile` |

---

## Project Structure

```
LauderAI/
├── server.js               ← Express server: static files + both API routes
├── extractSlides.js        ← PPTX ZIP parsing and text extraction
├── .env                    ← Secret API keys (never commit this)
├── .gitignore
├── package.json
├── tmp-uploads/            ← Temporary storage for uploads (auto-created, auto-deleted)
└── public/
    ├── images/
    ├── audio/
    ├── index.html          ← Intro page: asks for the user's name
    ├── chatbot.html        ← Chat feature + presenation review
    ├── main.js             ← Main backend chat rendering, tab switching, slide submission
    ├── feedback.js         ← API client for chat
    ├── slideReview.js      ← API client for slide uploads
    ├── slideUI.js          ← Parses AI review text and renders slide cards
    ├── Personality.js      ← Mr. Lauder + SlideReviewer character definitions
    └── chatbotstyles.css   ← All styles (chat + slide review)
```

Files in `public/` are served directly to the browser. Files in the root (`server.js`, `extractSlides.js`, `.env`) run on the server only — the browser has no direct access to them. `tmp-uploads/` is intentionally outside `public/` so uploaded student files are never publicly accessible.

<!-- 
---


## Architecture

```
BROWSER
  │  POST /api/feedback        { systemPrompt, userMessage }
  │  POST /api/review-slides   multipart/form-data (.pptx)
  │  (API key never leaves the server)
  ▼
server.js (Node.js · Express)
  ├─ express.static()          → serves public/
  ├─ POST /api/feedback        → Groq llama-3.1-8b-instant
  └─ POST /api/review-slides
       multer → tmp-uploads/
       extractSlides.js parses .pptx XML
       → Groq llama-3.3-70b-versatile
       cleanupFile() deletes tmp file
  ▼
Groq API (HTTPS · API key added here)
```
-->
---

## Running it locally

### Prerequisites

- Node.js v18 or later
- A free Groq API key from [console.groq.com](https://console.groq.com)


### Installation


1. Clone the repository

2. Install dependencies
```bash
npm install
```

3. Create a .env file in the project root

| Variable | Default | Description |
|---|---|---|
| `GROQ_API_KEY` | — | Your Groq API key (required) |
| `AI_MODEL` | `llama-3.1-8b-instant` | Model used for chat feedback |
| `SLIDE_REVIEW_MODEL` | `llama-3.3-70b-versatile` | Model used for slide review |
| `PORT` | `3000` | Port the server listens on |
| `UPLOAD_SIZE_LIMIT_MB` | `15` | Maximum file size for PPTX uploads |

4. Start the server
```bash
node server.js
```
You must add your own API key to be able to use the website entirely. 

### Verify it's working

The terminal should print:

```
Server running at http://localhost:3000
API key loaded: YES
Slide review model: llama-3.3-70b-versatile
```

If `API key loaded: NO` appears, the `.env` file is missing, misnamed, or in the wrong directory.


## Thanks for checking it out!
Developed by: Annie, Erin, Jocelyn, Salma

---
## License

MIT
