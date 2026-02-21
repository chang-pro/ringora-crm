# Ringora — Multi-Tenant SaaS CRM & Power Dialer

![Next.js](https://img.shields.io/badge/Next.js-15-black?style=flat-square&logo=next.js)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-Postgres-3FCF8E?style=flat-square&logo=supabase&logoColor=white)
![Stripe](https://img.shields.io/badge/Stripe-Billing-635BFF?style=flat-square&logo=stripe&logoColor=white)
![WebRTC](https://img.shields.io/badge/Telnyx-WebRTC-00B26B?style=flat-square)
![Repo](https://img.shields.io/badge/Source-Private-orange?style=flat-square)

**A full-featured, multi-tenant SaaS platform combining CRM, power dialer, SMS, AI call analysis, and usage-based billing.** Built for sales teams that need an all-in-one outbound communication platform.

---

## Platform Features

### Softphone & Power Dialer
- In-browser calling via Telnyx WebRTC SDK — no hardware required
- Power dialer with sequential, fresh, hot, and retry queue modes
- Mute, hold, consult transfer, and manual voicemail drop
- Google Sheets sync for lead imports

### CRM & Pipeline Management
- Full lead management with table and kanban views
- Bulk import, archive, callback scheduling
- Complete call history and activity tracking per lead

### SMS & Communication
- Single and bulk messaging with real-time delivery tracking
- DNC / suppression list / consent enforcement (TCPA compliant)

### AI-Powered Call Analysis
- Automatic call transcription and scoring
- AI coaching feedback on recorded calls
- Performance analytics per agent

### Billing & Multi-Tenancy
- Stripe usage-based billing (per-seat + metered voice/SMS/recording/AI minutes)
- Full organization isolation with role-based access (super_admin, admin, agent)
- Per-tenant data scoping via Supabase Row Level Security

### AI Chatbot
- OpenRouter-powered assistant with CRM tool calling
- Natural language queries: search leads, pull stats, manage queue

## Architecture

```
┌─────────────────────────────────────────────────┐
│                   Frontend                       │
│         Next.js 15 App Router + React            │
│    Telnyx WebRTC SDK ◄──► SIP Credentials        │
└──────────────────┬──────────────────────────────┘
                   │
    ┌──────────────┼──────────────────┐
    ▼              ▼                  ▼
┌────────┐  ┌───────────┐  ┌──────────────┐
│ API    │  │ Webhooks  │  │ AI Pipeline  │
│ Routes │  │ (Telnyx)  │  │ (OpenRouter) │
└───┬────┘  └─────┬─────┘  └──────┬───────┘
    │             │                │
    ▼             ▼                ▼
┌─────────────────────────────────────────────┐
│              Supabase (Postgres)              │
│  RLS Policies │ org_id scoping │ Real-time   │
└──────────────────┬──────────────────────────┘
                   │
          ┌────────┴────────┐
          ▼                 ▼
    ┌──────────┐     ┌──────────┐
    │  Stripe  │     │ Telnyx   │
    │ Billing  │     │ Voice/SMS│
    └──────────┘     └──────────┘
```

### Call Flow
1. Agent dials → API runs compliance checks (DNC, consent) → Telnyx creates call
2. WebRTC SDK connects browser audio via SIP credentials
3. Webhooks process call events (state changes, recordings)
4. Call logs, recordings, and AI analysis stored with org_id scoping

## Tech Stack

- **Framework:** Next.js 15 (App Router), React, TypeScript
- **Database:** Supabase (PostgreSQL) with Row Level Security
- **Voice/SMS:** Telnyx WebRTC SDK, REST APIs
- **Payments:** Stripe (usage-based + subscription billing)
- **AI:** OpenRouter (transcription, scoring, chatbot)
- **Auth:** Supabase Auth with role-based access control
- **Deployment:** Vercel

## Skills Demonstrated

- Multi-tenant SaaS architecture with data isolation
- Real-time WebRTC integration for browser-based calling
- Usage-based billing system design with Stripe
- TCPA compliance implementation (DNC, consent tracking)
- AI pipeline integration for call analysis
- Full-stack development from database design to UI

---

> *This is a showcase page for a private repository. Source code available upon request for verified opportunities.*
