# 🛠️ Migaki — Tech Stack

> Every technology chosen here serves one purpose: making ASI-1's outputs reach the user as fast, cleanly, and reliably as possible.

---

## Frontend

### React Native
- **Why:** Single codebase for both Android and iOS. India's mobile market is split — we can't afford to build for one platform only.
- **Role:** All 3 screens (Home, Smart Disposal, Community), chat UI, FAB navigation, accordion cards, voting interface.

### Tailwind CSS (via NativeWind)
- **Why:** Utility-first styling keeps the UI consistent across screens without writing component-specific stylesheets.
- **Role:** Dark theme, spacing, typography, card layouts.

---

## Backend

### Django (Python)
- **Why:** Python is the natural language for AI-adjacent backends. Django's batteries-included approach means faster development for a solo builder.
- **Role:** API server, business logic, ASI-1 request handling, routing between services, complaint processing, voting timer logic (24hr/12hr windows).

---

## Database & Storage

### Supabase (PostgreSQL)
- **Why:** Managed PostgreSQL with real-time subscriptions built in — perfect for the live community feed where posts and votes need to update instantly.
- **Role:**
  - User profiles and auth records
  - Community posts and feed data
  - Voting records per design concept
  - ASI-1 conversation history per user
  - Civic complaint records and status tracking

### Supabase Storage
- **Why:** Already in the stack — no need for a separate service like S3.
- **Role:**
  - User-uploaded waste item photos
  - ASI-1 generated DIY preview images
  - Community before/after project photos
  - Evidence photos for civic complaints

---

## Authentication

### Google SSO + Apple SSO
- **Why:** Zero friction onboarding. No passwords. Users in India are already signed into Google on their Android devices — one tap signup.
- **Role:** User identity, session management, community membership verification.

---

## AI

### ASI-1 API
- **Model:** `asi1-mini`
- **Endpoint:** `https://api.asi1.ai/v1/chat/completions`
- **Why:** Required by Ideathon 2026. ASI-1 is OpenAI-compatible making integration straightforward. Native text-to-image generation means no separate image API needed.
- **Role across all features:**

| Feature | ASI-1 Usage |
|---------|-------------|
| Home Upcycle | DIY idea generation, step-by-step guides, image generation of finished product |
| Smart Disposal | Waste classification, home disposal tips generation |
| Community Canvas | Design concept generation, image preview of installations, build guide generation, X post drafting |
| Civic Complaint | Formal complaint letter drafting, SWM Rules 2026 citation, escalation pathway, X post drafting |

---

## Third Party APIs

### Google Maps API
- **Why:** Most reliable maps API with strong India coverage including Tier 2/3 cities.
- **Role:** Surface nearest recycling centres, kabadiwala locations, and waste disposal facilities when home disposal isn't possible.

### Gmail API
- **Why:** Enables sending emails directly from the community's shared account without leaving the app.
- **Role:** Send ASI-1 drafted civic complaint emails to municipal authorities (BMC, local ward offices) on behalf of the community.

### X (Twitter) API
- **Why:** X remains the primary platform for civic pressure campaigns in India. Direct API integration means zero friction for community posts.
- **Role:**
  - Auto-post completed Community Canvas projects (before/after)
  - Auto-post civic complaint public pressure threads
  - Hashtag and tagging strategy embedded in ASI-1 output

---

## Architecture Overview

```
User Device (React Native)
        ↓
Django Backend (API Server)
        ↓
    ┌───────────────────────────────┐
    │         ASI-1 API             │  ← Brain of the app
    │   (asi1-mini + image gen)     │
    └───────────────────────────────┘
        ↓               ↓
  Supabase DB     Supabase Storage
  (data layer)    (media layer)
        ↓
  Third Party APIs
  ┌──────────────────────────────┐
  │ Google Maps | Gmail | X API  │
  └──────────────────────────────┘
```

---

## Why This Stack Works for Migaki

| Requirement | How the stack meets it |
|-------------|----------------------|
| Mobile-first India audience | React Native — Android + iOS from one codebase |
| Real-time community feed | Supabase real-time subscriptions |
| AI at the core | ASI-1 handles ideation, classification, drafting, and image generation |
| Solo developer friendly | Django + Supabase = minimal DevOps overhead |
| Scalable | Supabase scales automatically; Django can be containerised |
| Cost-efficient MVP | Supabase free tier + ASI-1 API pay-per-use = near-zero infrastructure cost to start |

---

> 磨 Migaki — Every waste has a story worth finishing.