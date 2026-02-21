# Ringora — Multi-Tenant SaaS CRM & Power Dialer

![Next.js](https://img.shields.io/badge/Next.js-15-black?style=flat-square&logo=next.js)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Convex](https://img.shields.io/badge/Convex-Backend-FF6B6B?style=flat-square)
![Telnyx](https://img.shields.io/badge/Telnyx-WebRTC-00B26B?style=flat-square)
![Stripe](https://img.shields.io/badge/Stripe-Billing-635BFF?style=flat-square&logo=stripe&logoColor=white)
![Rust](https://img.shields.io/badge/Rust-WASM-000000?style=flat-square&logo=rust&logoColor=white)
![Repo](https://img.shields.io/badge/Source-Private-orange?style=flat-square)

**I built a full SaaS platform from scratch — CRM, power dialer, SMS, AI call analysis, and billing — that a sales team could actually use to run their entire outbound operation from a browser.** No phone hardware, no third-party dialer fees, just log in and start calling.

---

## Why This Is Hard

Building a dialer sounds simple until you realize:
- **WebRTC in a browser is brutal** — handling oudio streams, oall state, oold, transfer, and voicemail drop across different browsers and network conditions
- **Multi-tenancy at the data layer** — every query, every webhook, every real-time update has to be scoped to the correct organization. One leak = lawsuit.
- **Usage-based billing** — you're metering voice minutes, SMS segments, recording storage, and AI analysis per org, per agent, in real-time. Stripe doesn't hand you this — you build it.
- **TCPA compliance** — DNC lists, consent tracking, suppression lists. Get this wrong and your customer gets fined $500–$1,500 per call.

## What I Built

### Browser-Based Softphone (Telnyx WebRTC)
Built a full softphone that runs in the browser. No downloads, no plugins. Features:
- **Make and receive calls** directly from the browser via Telnyx WebRTC SDK
- **Mute, hold, consult transfer** — not just basic calling, full call control
- **Manual voicemail drop** — pre-recorded messages that agents drop with one click
- **Real-time call status** with duration tracking

### Power Dialer with 4 Queue Modes
- **Sequential** — call the list in order
- **Fresh** — prioritize never-contacted leads
- **Hot** — prioritize leads with recent activity
- **Retry** — cycle back through no-answers and voicemails

Each mode pulls from the queue, runs compliance gates (DNC check, consent check) before every single dial, logs the attempt, and advances automatically.

### Full CRM & Pipeline
- Table and kanban views for lead management
- Bulk import from CSV and Google Sheets
- Per-lead call history, notes, callbacks, and activity timeline
- Archive and restore workflows

### AI-Powered Call Analysis
- Automatic transcription of recorded calls
- AI scoring and coaching feedback per call
- Performance analytics per agent across the org

### Usage-Based Billing (Stripe)
- Per-seat subscription + metered usage (voice minutes, SMS, recordings, AI analysis)
- Real-time usage tracking per organization
- Automated invoicing and overage handling

### Multi-Tenant Architecture
- Full organization isolation via Row Level Security
- Role-based access: `super_admin`, `admin`, `agent`
- Every database query, every webhook, every real-time subscription scoped by `org_id`

### AI Chatbot with CRM Tool Calling
- Natural language interface powered by OpenRouter
- Agents can ask "how many calls did I make today?" or "find leads in Orlando" and the bot queries the CRM directly

### Rust WASM Module
- Performance-critical client-side processing compiled to WebAssembly from Rust
- Handles compute-heavy operations without blocking the UI thread

## Architecture

```
┌─────────────────────────────────────────────────┐
│                   Frontend                       │
│         Next.js 15 + React + Zustand             │
│    Telnyx WebRTC SDK ◄──► SIP Credentials        │
│         Rust WASM for heavy client ops           │
└──────────────────┬──────────────────────────────┘
                   │
    ┌──────────────┼──────────────────┐
    ▼              ▼                  ▼
┌────────┐  ┌───────────┐  ┌──────────────┐
│ Convex │  │ Webhooks  │  │ AI Pipeline  │
│ Backend│  │ (Telnyx)  │  │ (OpenRouter) │
└───┬────┘  └─────┬─────┘  └──────┬───────┘
    │             │                │
    ▼             ▼                ▼
┌─────────────────────────────────────────────┐
│              Convex (Real-time DB)            │
│  RLS Policies │ org_id scoping │ Live sync   │
└──────────────────┬──────────────────────────┘
                   │
          ┌────────┴────────┐
          ▼                 ▼
    ┌──────────┐     ┌──────────┐
    │  Stripe  │     │ Telnyx   │
    │ Billing  │     │ Voice/SMS│
    └──────────┘     └──────────┘
```

## Tech Stack

`Next.js 15` · `TypeScript` · `Convex` · `Telnyx WebRTC` · `Stripe` · `Tailwind v4` · `Zustand` · `Rust WASM` · `OpenRouter` · `Vercel`

---

> *Closed source — actively in development. Demo and source code available for serious inquiries.*
