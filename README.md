# BrewOS — Café Direct-Ordering Platform

> **Version:** 1.0 (spec) · **Status:** Documentation complete, awaiting approval before implementation.
> **First deployment:** AD'S COFFEE HOUSE.

BrewOS is a **premium white-label online ordering platform for cafés**. Instead of relying on
marketplaces (Swiggy, Zomato), cafés take **direct** online orders through their own branded website and
run operations from a simple, elegant dashboard. It is built **multi-tenant from day one** so new cafés
onboard through configuration and branding — not custom code.

The complete product specification lives in [`docs/`](./docs/). Start with
[`docs/01_PRD.md`](./docs/01_PRD.md) (the index) and [`docs/00_Vision.md`](./docs/00_Vision.md).

---

## Current repository state

This repo currently contains the **existing static marketing site** for **Coffee Under Palms (CUP)** — a
one-page HTML/CSS/JS site (`index.html`, `styles.css`, `script.js`, `logo.webp`). The PRD refers to the
first client as **AD'S COFFEE HOUSE**; this brand-name difference is flagged in
[`docs/DECISIONS.md`](./docs/DECISIONS.md#i-001--brand-name-mismatch-ads-coffee-house-vs-existing-coffee-under-palms)
and needs confirmation. The prescribed platform (Next.js + NestJS) has **not** been built yet.

### Run the current static site

```bash
python3 -m http.server 5173
# open http://localhost:5173
```

---

## Documentation map

| Area | Documents |
|---|---|
| **Overview** | [00_Vision](./docs/00_Vision.md) · [01_PRD (index)](./docs/01_PRD.md) · [ROADMAP](./docs/ROADMAP.md) |
| **Product** | [02_Website](./docs/02_Website.md) · [03_Admin_Dashboard](./docs/03_Admin_Dashboard.md) · [USER_FLOWS](./docs/USER_FLOWS.md) |
| **Engineering** | [04_Backend](./docs/04_Backend.md) · [ARCHITECTURE](./docs/ARCHITECTURE.md) · [API](./docs/API.md) · [FOLDER_STRUCTURE](./docs/FOLDER_STRUCTURE.md) · [CODING_STANDARDS](./docs/CODING_STANDARDS.md) |
| **Data** | [06_Database](./docs/06_Database.md) · [DATABASE](./docs/DATABASE.md) |
| **Design** | [05_UI_UX](./docs/05_UI_UX.md) · [DESIGN_SYSTEM](./docs/DESIGN_SYSTEM.md) · [COMPONENT_LIBRARY](./docs/COMPONENT_LIBRARY.md) |
| **Security & Ops** | [07_Security](./docs/07_Security.md) · [08_Deployment](./docs/08_Deployment.md) · [DEPLOYMENT](./docs/DEPLOYMENT.md) · [ENVIRONMENT_VARIABLES](./docs/ENVIRONMENT_VARIABLES.md) |
| **Quality** | [09_Testing](./docs/09_Testing.md) · [TEST_PLAN](./docs/TEST_PLAN.md) · [ACCEPTANCE_CRITERIA](./docs/ACCEPTANCE_CRITERIA.md) |
| **Meta** | [DECISIONS](./docs/DECISIONS.md) · [CHANGELOG](./docs/CHANGELOG.md) |

Operating rules for anyone (human or AI) building on this repo: [`CLAUDE.md`](./CLAUDE.md).

---

## Prescribed technology stack (for V1 implementation)

- **Frontend:** Next.js (App Router), React, TypeScript, Tailwind CSS, shadcn/ui, Framer Motion, React
  Hook Form, Zod.
- **Backend:** NestJS, TypeScript, Prisma, PostgreSQL, Redis, BullMQ.
- **Services:** Cloudinary (storage), Google Maps (address), Resend (email), Razorpay (payments).
- **Deploy:** Docker + Coolify (or comparable). See [ARCHITECTURE](./docs/ARCHITECTURE.md).

## Success targets (V1)

Order in **< 60 s** · staff accept order in **< 5 s** · Lighthouse **95+** · mobile perf **90+** · page
load **< 2 s** · no critical security vulnerabilities · fully responsive.

---

## Status & next step

The documentation system has been generated from the Master PRD. **No application code has been written
yet.** Before implementation begins, please review the docs and resolve the open questions in
[`docs/DECISIONS.md`](./docs/DECISIONS.md) (brand name, greenfield vs. migration, auth mechanism,
delivery model, and more).
