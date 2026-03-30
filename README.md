# Meeting Insight Platform

A full-stack web application that turns unstructured meeting recordings into structured, actionable intelligence — transcripts, summaries, translated output, and extracted action items — all powered by **local open-source LLMs** with no external API dependency.

---

## What It Does

Most meetings are unstructured. Notes get lost, action items go untracked, and people who missed a meeting have no easy way to catch up. This platform solves that by letting users upload a meeting recording and automatically extracting everything useful from it.

Upload a video → get a transcript → get a summary → get action items → translate if needed. All results are saved per user and accessible from a history panel, so nothing is lost between sessions.

---

## Features

### Core Pipeline
- **Transcription** — Upload any meeting video; the backend invokes Whisper via a Python subprocess to extract a full transcript from the audio
- **Summarisation** — The transcript is sent to Mistral-7B (running locally via Ollama), which returns a structured summary covering main topics, key decisions, suggestions, and agreed follow-ups
- **Action Item Extraction** — A separate LLM call parses the transcript and returns a structured JSON array of action items, each with an `action`, `assignee`, and `deadline`
- **Translation** — Translate the transcript to French using Mistral via Ollama, on demand

### Auth & Session Management
- JWT-based authentication with BCrypt password hashing
- Token stored in `localStorage`; protected routes redirect unauthenticated users to login
- Per-user data isolation — all history and uploads are scoped to the logged-in user

### History & Review
- Every processed video is saved to MongoDB with its transcript, summary, and action items
- Right sidebar lists all previous uploads with filename and timestamp
- Clicking any history entry opens a modal with the full transcript, summary, and action items for that session

### UI Layout
- **Left sidebar** — Settings panel
- **Center** — Upload area with inline video preview, transcript, summary, translation, and action items displayed sequentially after processing
- **Right sidebar** — Previous uploads panel with clickable history cards
- **Modal** — Full detail view for any past upload

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React, React Router |
| Backend | Node.js, Express.js |
| Database | MongoDB (Mongoose) |
| Authentication | JWT + BCrypt |
| Transcription | Whisper (Python subprocess) |
| AI Inference | Mistral-7B via Ollama (local) |
| File Handling | express-fileupload |

---

## Architecture

```
Client (React)
│
├── POST /api/gettranscript     → Saves video → runs Whisper → returns transcript
├── POST /api/getsummary        → Sends transcript to Mistral → returns summary
├── POST /api/getactionitems    → Sends transcript to Mistral → returns JSON action items
├── POST /api/gettranslation    → Sends transcript to Mistral → returns French translation
└── GET  /api/history           → Returns all past uploads for the logged-in user
```

All Mistral inference runs locally via Ollama on `http://localhost:11434`. No data leaves your machine.

---

## How the LLM Pipeline Works

**Summarisation** sends the raw transcript to Mistral with a structured prompt requesting main topics, decisions, suggestions, and follow-ups in bullet points.

**Action item extraction** uses a strict prompt instructing the model to return _only_ a JSON array with no markdown formatting. The response is cleaned (strips any accidental ` ```json ``` ` fences), parsed, and normalised for case-inconsistent keys before being stored and returned.

**Translation** sends the transcript with a simple instruction to translate

---

## Getting Started

### Prerequisites

- Node.js
- MongoDB (local or Atlas)
- Python with `whisper` installed (`pip install openai-whisper`)
- [Ollama](https://ollama.com) installed and running locally

### 1. Pull the Mistral model

```bash
ollama pull mistral:7b-instruct-q2_K
ollama pull mistral:7b-instruct-q4_0
```

### 2. Install backend dependencies

```bash
cd backend
npm install
```



### 4. Start Ollama

```bash
ollama serve
```

### 5. Start the backend

```bash
node index.js
```

### 6. Start the frontend

```bash
cd frontend
npm install
npm start
```

The app will be available at `http://localhost:3000`.

---



# AI-Powered-Meeting-Insight-Platform-
![image](https://github.com/user-attachments/assets/31bfde68-4ed4-44be-9bf7-5713e6386e4d)

![image](https://github.com/user-attachments/assets/ebd56af7-2b49-4ec4-8e87-d6218405b753)

![image](https://github.com/user-attachments/assets/47e8c4a4-b889-4183-bd18-13f5e606875d)


![image](https://github.com/user-attachments/assets/bc0b6322-f7b9-492e-bb49-0baa668d12e7)
![image](https://github.com/user-attachments/assets/60195cfa-7a58-46c1-bbd2-c621fcdedf8a)


![image](https://github.com/user-attachments/assets/a4e729d7-5d84-4b56-ba26-ef1914733446)

![image](https://github.com/user-attachments/assets/24531bc4-9071-4181-9b79-7bc73d20586d)

![image](https://github.com/user-attachments/assets/308cc892-2d43-4230-8699-6f45faa713ac)
![image](https://github.com/user-attachments/assets/098c7964-b8ba-41ec-967d-2859bd43fed0)





