# Wavydream

**NFC business cards backed by a B2B lead and analytics platform.**

> Smart NFC business cards that turn networking into measurable, data-driven lead generation.

**Website:** [wavydream.com](https://wavydream.com) · **Built by:** [Jym-Alexandre Nkana](https://github.com/PixisSensei), CEO & Co-founder

## Overview

Wavydream is a SaaS platform I co-founded and built from the ground up. A tap on a Wavydream card opens the holder's live digital card. From there, a visitor can save the contact, book a meeting or leave their details. Companies manage their whole fleet of cards and follow every scan and lead in a dashboard.

### The problem

Paper business cards are static, unmeasurable and easily lost. Networking events produce contacts, but no way to track engagement or follow up systematically.

### The solution

- **Always up to date.** The card is a web page, so a change of job or number needs no reprint.
- **One tap to act.** Save to contacts (vCard), add to Apple Wallet, book a meeting, or leave your details as a lead.
- **Measured.** Every scan and lead is recorded and reported per card and per company.
- **Managed at company scale.** Fleets of cards, roles, enforced branding, exports and CRM webhooks.

### Traction (as of March 2026)

- ~20 active B2B clients
- 30+ active cards deployed
- 300–400 scans per month
- Clients include dental practices, insurance brokers and networking-event organizers

## Partnerships

- **Middle Event**: card distribution to attendees at networking events.
- B2B clients across healthcare (dental), insurance and professional services.

## Architecture

```
 NFC tap / QR scan
        │
        ▼
 /go/<slug>  ── state? ──► unclaimed → claim flow
        │                  inactive  → notice page
        ▼ active
 /v/<slug>   card page, pre-rendered (ISR) from a materialized view,
        │    regenerated when the card changes
        │
        ├─► scan beacon ─► API route (zod validation, dedup, DB rate limit)
        │                    └─► analytics_events ─► role-checked SQL functions ─► dashboards
        ├─► vCard / Apple Wallet pass
        ├─► lead form ───► leads ─► CSV/XLSX export · signed webhooks (HMAC-SHA256) → CRM
        └─► booking ─────► slot engine ─► bookings (Postgres exclusion constraint: no double booking)
                                            └─► email outbox, drained every minute by pg_cron + pg_net

 Supabase Postgres: Row-Level Security on every tenant table (users · managers · admins · companies)
```

## Engineering highlights

### Fast card pages
- Card pages are statically pre-rendered (Next.js ISR) and regenerated on change, so after the tap redirect the card itself comes from the CDN.
- A weekly CI benchmark fails the build when a card page goes over its byte budget. Lab baseline on a 4G profile: **62 ms TTFB, 0.5 s LCP, 247 kB**.

### A booking engine that can't double-book
- The availability engine is adapted from [Cal.com](https://github.com/calcom/cal.diy)'s open-source code (MIT).
- Overlapping bookings are rejected by the database itself, with a Postgres `EXCLUDE USING gist` constraint on time ranges. A concurrent race test exercises it.
- Transactional emails go through an outbox processed every minute by `pg_cron` + `pg_net`. Manage links use hashed tokens, and calendars subscribe through ICS feeds.

### Multi-tenant security in the database
- Three roles (user, manager, admin). Companies own fleets of cards, with brand overrides and locked fields enforced by Postgres, not by the UI.
- 26 RLS-protected tables with 133 policies. Triggers guard role and tenant columns.

### Analytics and leads
- Beacon-based scan capture: source (NFC, QR, direct), device and browser, referrer, and city-level location.
- Events are validated with zod, deduplicated by a partial unique index and rate-limited in the database.
- Dashboards cover per-company and per-card activity, device and location breakdowns, leaderboards and period comparisons.
- Leads export to CSV/XLSX and push to any CRM that accepts webhooks, signed with HMAC-SHA256.

### Product details
- Available in French, English and Dutch.
- Installable as a PWA, with Apple Wallet passes (PassKit) and vCard 3.0 contact files.

## Quality

| | |
|---|---|
| **Type safety** | Strict TypeScript, with `noUncheckedIndexedAccess` |
| **Tests** | 2,400+ Vitest tests, 118 pytest API tests, SQL smoke tests including a concurrent booking race |
| **Database** | 78 versioned SQL migrations |
| **CI (GitHub Actions)** | Lint + typecheck · tests with coverage · security (gitleaks, npm audit, Semgrep, scan of the live bundle) · hourly production probes that open an issue on failure · weekly performance bench with a byte budget |

## Tech stack

| Layer | Technology |
|-------|-----------|
| **Frontend** | Next.js 15 (App Router), React 19, TypeScript, Tailwind CSS, shadcn/ui, Recharts, FullCalendar |
| **Backend** | Next.js route handlers and server actions, zod |
| **Database & auth** | Supabase: PostgreSQL (RLS, PL/pgSQL, materialized views, `pg_cron`, `pg_net`, `btree_gist`), Supabase Auth |
| **Integrations** | Apple Wallet (PassKit), vCard, ICS, SMTP email, signed webhooks |
| **Testing & CI** | Vitest, Testing Library, pytest, GitHub Actions, Codecov, Semgrep, gitleaks |
| **Hosting** | Vercel (CDN, ISR) |
| **Development** | Claude Code (AI-assisted development) |

## What building Wavydream taught me

- **Full-stack product development**, from idea to paying customers.
- **Putting invariants in the database**: tenant isolation, field locks, no double booking. A UI bug can't break them.
- **Shipping with a safety net**: tests, security scanning, performance budgets and production probes in CI.
- **B2B sales and client management** in a SaaS context.
- **AI-assisted development** workflows with Claude Code.

---

*Source code is proprietary. This repository is a product showcase. Happy to walk through the code in an interview.*
