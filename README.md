# Wavydream

**NFC Business Card SaaS with Advanced Data Analytics Dashboard**

> Smart NFC business cards that transform networking into measurable, data-driven lead generation.

## Overview

Wavydream is a SaaS platform I co-founded and built from the ground up. It provides NFC-enabled business cards backed by a powerful analytics dashboard, allowing businesses and professionals to never lose a lead again.

### The Problem

Traditional business cards are static, unmeasurable, and easily lost. Networking events generate contacts but there's no way to track engagement or follow up systematically.

### The Solution

Wavydream NFC cards are connected to a cloud platform that:
- **Instantly updates** contact information on the virtual card -- no reprinting needed
- **Books appointments** directly through the card interface
- **Exchanges contacts** straight into the phone's contact list
- **Tracks every interaction** with a comprehensive data dashboard

## Key Features

### Smart NFC Cards
- Tap-to-share contact information
- Real-time editable virtual business card
- Integrated appointment booking
- Direct contact exchange to phone

### Data Analytics Dashboard
- **Fleet management** for businesses with multiple cards
- **Geolocation tracking** of where contacts were made
- **Timestamp analysis** of when interactions happen
- **Device analytics** tracking what devices scanned cards
- **Lead reporting** with exportable data for CRM integration

### Metrics (as of March 2026)
- ~20 active B2B clients
- 30+ active cards deployed
- 300-400 scans per month
- Clients include: dental practices, insurance brokers, networking event organizers

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Frontend** | TypeScript, Tailwind CSS |
| **Backend / DB** | Supabase (PostgreSQL, Auth, Real-time) |
| **Hosting** | Vercel (Edge Functions, CDN) |
| **Development** | Claude Code (AI-assisted development) |

## Architecture

```
User scans NFC card
        |
        v
  Virtual Card Page (Vercel Edge)
        |
        v
  Supabase Real-time DB
   /          \
  v            v
Dashboard    Analytics
(Fleet Mgmt)  (Geo, Time, Device)
        \      /
         v    v
    Business Owner
    (Lead Management)
```

## Partnerships

- **Middle Event** -- Integration with networking events for attendee card distribution
- B2B clients across healthcare (dental), insurance, and professional services

## Why This Project Matters

Building Wavydream taught me:
- **Full-stack product development** from idea to paying customers
- **Data pipeline design** for real-time analytics at scale
- **B2B sales and client management** in a SaaS context
- **AI-assisted development** workflows using Claude Code

---

*Note: Source code is proprietary. This repository serves as a product showcase.*

**Website:** [wavydream.com](https://wavydream.com)
**Built by:** [Jym-Alexandre Nkana](https://github.com/PixisSensei)
