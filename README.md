# MeetingMind

A frictionless voice-to-notes web app. Record or paste a transcript — Claude analyzes it and pushes a structured note to Notion automatically.

---

## What It Does

One tap to record. When you stop, it transcribes with Groq Whisper, analyzes with Claude AI, and saves a formatted note to your Notion database. No copy-paste, no manual steps.

Detects note type automatically:
- **Meeting** — summary, participants, key moments, decisions, action items, next steps
- **Todo** — priorities, tasks, personalized affirmation
- **BrainDump** — core thought, idea clusters, open questions

---

## How to Use

1. Open the app in any browser
2. Enter your API keys in Settings (saved locally on your device)
3. Tap the record button → speak → tap to stop
4. Note is analyzed and pushed to Notion automatically

Or paste a transcript directly using the "or paste a transcript" toggle.

---

## Setup

You need three things:

**Claude API Key**
- [console.anthropic.com](https://console.anthropic.com) → API Keys → Create Key
- Starts with `sk-ant-...`

**Groq API Key**
- [console.groq.com](https://console.groq.com) → API Keys → Create key
- Free tier — used for audio transcription
- Starts with `gsk_...`

**Notion Integration** (optional — skip if you just want the formatted copy)
- [notion.so/my-integrations](https://notion.so/my-integrations) → New integration → copy token
- Open your Notion database → `…` → Connections → add your integration
- Database needs columns: `Name` (title), `Type` (select), `Key Topics` (multi-select), `Date` (date)
- Select options for Type: `Meeting`, `Todo`, `BrainDump`

---

## Stack

| Layer | Service | Cost |
|---|---|---|
| Hosting | GitHub Pages | Free |
| Transcription | Groq Whisper (`whisper-large-v3`) | Free (7,200s/day) |
| AI Analysis | Anthropic Claude Haiku 4.5 | ~$0.005/note |
| Notion Proxy | Cloudflare Workers | Free (100k req/day) |
| Storage | Notion API | Free |

Total cost per note: ~$0.005. Effectively free for personal use.

---

## Architecture

```
Browser
  ├── Groq Whisper API       — audio → transcript
  ├── Anthropic Claude API   — transcript → structured JSON
  └── Cloudflare Worker      — proxies Notion API (CORS bypass)
        └── Notion API       → saves note to database
```

Credentials are stored in `localStorage` per device and never sent anywhere except the respective APIs.

---

## iPhone Shortcut

A companion iOS Shortcut uses the Action Key to trigger recording hands-free. Audio is sent to Groq for transcription, then to a Cloudflare Worker `/pipeline` endpoint that handles the Claude analysis and Notion push server-side. A notification shows the note title when done.

---

*Built March 2026*
