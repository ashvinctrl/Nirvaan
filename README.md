# Nirvaan

[![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)](https://typescriptlang.org)
[![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black)](https://react.dev)
[![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=node.js&logoColor=white)](https://nodejs.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

AI-powered outbound voice call assistant. Enter a phone number, Nirvaan places an AI-driven call via [Retell AI](https://retellai.com), then tracks the call in real time and surfaces a structured summary when it ends.

## How it works

```
User submits phone number
        │
        ▼
  Node.js backend calls Retell API → creates outbound phone call
        │
        ▼
  Frontend polls /call-details/:call_id every 2 s
        │
        ▼
  CallProgress shows live call status (ringing → in-progress → ended)
        │
        ▼
  CallResults renders transcript, sentiment, and call summary
```

## Tech stack

| Layer | Technology |
|---|---|
| Frontend | React 18, TypeScript, Vite, Tailwind CSS |
| Backend | Node.js, Express |
| AI / Voice | Retell AI (outbound call + LLM agent) |
| Containerization | Docker, Docker Compose |

## Prerequisites

- Node.js 18+
- [Retell AI account](https://retellai.com) — you need an API key, an agent ID, and a provisioned phone number

## Quick start

```bash
git clone https://github.com/ashvinctrl/Nirvaan.git
cd Nirvaan
cp .env.example .env
```

Edit `.env`:

```env
RETELL_API_KEY=your_retell_api_key
AGENT_ID=your_retell_agent_id
FROM_PHONE_NUMBER=+1xxxxxxxxxx
PORT=8050
```

### With Docker Compose

```bash
docker-compose up --build
```

Frontend: `http://localhost:5173` — Backend: `http://localhost:8050`

### Without Docker

```bash
# Backend
cd backend && npm install && node app.js

# Frontend (new terminal)
cd .. && pnpm install && pnpm dev
```

## API

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/submit` | Creates an outbound call. Body: `{ "phone_number": "+1..." }`. Returns `call_id`. |
| `GET` | `/call-details/:call_id` | Returns current call status, transcript, and metadata from Retell. |

## Project layout

```
backend/
  app.js            Express server — Retell API integration
src/
  components/
    Form.tsx          Phone number input and submission
    CallProgress.tsx  Real-time call status polling
    CallResults.tsx   Transcript and summary display
    AudioControls.tsx Playback controls (if recording available)
  pages/
  hooks/
```

## Contributing

1. Fork → feature branch → pull request
2. Run `pnpm lint` before opening a PR

## License

MIT
