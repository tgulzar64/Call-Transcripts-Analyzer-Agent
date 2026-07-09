# 📞 Call Transcript Analyzer

> Upload a call transcript and get instant AI-powered analysis — empathy scores, emotion timelines, coaching suggestions, customer personas, and call highlights.

**🌐 Live Demo:** This agent is published on the [NeuralSeek LATAM Demos page](https://demo.neuralseek.com/demos-page/transcript-expert)

---

## What it does

You upload or paste a `.txt` file containing one or more call transcripts. The app splits them by `Call ID`, sends each one to a NeuralSeek Maistro agent, and surfaces structured intelligence for each call.

| Input | Output |
|-------|--------|
| `.txt` file with call transcript(s) | Empathy score (0–10) |
| Paste transcript directly | Emotion timeline (minute by minute) |
| Multiple calls in one file | Customer persona |
| | Coaching suggestions for the agent |
| | Call highlights / key moments |
| | Call category (IT support, billing, etc.) |

---

## Screenshots

### Upload Interface
![Upload Screen](./docs/upload.png)

### Analysis Results
![Analysis Output](./docs/results.png)

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| AI Engine | [NeuralSeek](https://neuralseek.com) — Maistro agent (`Transcript_Expert`) |
| File Ingestion | NeuralSeek Explore Upload API |
| Framework | Next.js 15 (App Router) |
| Frontend | React 19 + Tailwind CSS v4 |
| Language | TypeScript |
| HTTP Client | Axios |

---

## Project Structure

```
transcripts-app/
├── constants/
│   └── neuralseek.config.ts   # NeuralSeek instance URLs (API key via env var)
├── src/app/
│   ├── api/
│   │   └── neuralseek/
│   │       ├── maistro/
│   │       │   └── route.ts        # POST → calls NeuralSeek Maistro agent
│   │       └── upload-file/
│   │           └── route.ts        # POST → uploads file to NeuralSeek Explore
│   ├── components/
│   │   └── transcripts.tsx         # Main UI — upload, paste, analysis display
│   ├── layout.tsx
│   └── page.tsx
├── public/
├── next.config.ts
├── tsconfig.json
└── package.json
```

---

## Getting Started

### Prerequisites
- Node.js 18+
- A [NeuralSeek](https://neuralseek.com) account with a configured Maistro agent (`Transcript_Expert`)
- Your NeuralSeek API key

### 1. Clone the repo

```bash
git clone https://github.com/your-username/transcript-analyzer.git
cd transcript-analyzer
```

### 2. Set up environment variables

Create a `.env.local` file in the root:

```env
NEURALSEEK_API_KEY=your_api_key_here
NEXT_PUBLIC_API_BASE_URL=http://localhost:3000/api
```

### 3. Install dependencies

```bash
npm install
```

### 4. Run the development server

```bash
npm run dev
# Open http://localhost:3000
```

---

## How it works

```
User uploads .txt file or pastes transcript
          ↓
File → NeuralSeek Explore Upload API (stores transcript)
          ↓
App splits text by "Call ID:" markers into individual blocks
          ↓
Each block → POST /api/neuralseek/maistro
          ↓
Next.js API route calls NeuralSeek Maistro (Transcript_Expert agent)
          ↓
Agent returns structured analysis:
  - Call category
  - Empathy score
  - Emotion timeline
  - Customer persona
  - Coaching suggestions
  - Call highlights
          ↓
UI renders results per call
```

---

## Transcript Format

The app expects plain text files. For multi-call files, prefix each call with a `Call ID:` line — the app will split and analyze them individually:

```
Call ID: it_call_001
Agent: Hi, thanks for calling IT support. How can I help you today?
Customer: My internet has not been working from home since May 14...
...

Call ID: it_call_002
Agent: Good afternoon, billing department...
Customer: I was charged twice this month...
```

Single calls without a `Call ID:` header also work — the app will treat the entire file as one call.

---

## API Routes

### `POST /api/neuralseek/maistro`

Proxies a call to the NeuralSeek Maistro agent.

**Request body:**
```json
{
  "url_name": "staging-talha",
  "agent": "Transcript_Expert",
  "params": [
    { "name": "callTranscripts", "value": "<transcript text>" }
  ],
  "options": {
    "returnVariables": false,
    "returnVariablesExpanded": false
  }
}
```

### `POST /api/neuralseek/upload-file`

Uploads a transcript file to NeuralSeek Explore.

**Request:** `multipart/form-data` with fields `file` and `url_name`.

---

## Deployment

### Vercel (recommended for Next.js)
```
Build command:   npm run build
Output dir:      .next
```
Set `NEURALSEEK_API_KEY` and `NEXT_PUBLIC_API_BASE_URL` as environment variables in your Vercel project settings.

### Self-hosted
```bash
npm run build
npm start
# Runs on port 3000
```

---

## Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `NEURALSEEK_API_KEY` | ✅ Yes | Your NeuralSeek API key |
| `NEXT_PUBLIC_API_BASE_URL` | ✅ Yes | Base URL of the app (e.g. `http://localhost:3000/api` locally, your deployment URL in production) |

---

## Built by

**Esteban** — Babson College, November 2025

Powered by [NeuralSeek](https://neuralseek.com) · Built with Next.js 15

---

## License

MIT — free to use, fork, and build on.

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

Check out our [Next.js deployment documentation](https://nextjs.org/docs/app/building-your-application/deploying) for more details.
