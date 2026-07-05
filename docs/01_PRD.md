# BrewOS — Master Product Requirements Document (Index)

> **Project:** BrewOS · **Version:** 1.0 · **Status:** Production Specification
> **First Client:** AD'S COFFEE HOUSE · **Prepared for:** Claude Code

This file is the **canonical index** for the BrewOS documentation system. The original Master PRD
(`masterprd.pdf`, 10 chapters) has been reorganized into the focused documents listed below. No
requirement from the source PRD has been dropped — every chapter maps to one or more documents
here. Where a requirement appears in multiple chapters, it is stated once in its "home" document and
cross-linked from the others.

## What BrewOS is

BrewOS is a **premium white-label online ordering platform built specifically for cafés**. Unlike
marketplaces such as Swiggy and Zomato, BrewOS lets cafés own their customer relationships, take
direct online orders through their own website, and run operations from a simple, elegant dashboard.

The **first deployment is AD'S COFFEE HOUSE**, but the system must be designed as a reusable,
multi-tenant SaaS platform where new cafés are onboarded through configuration and branding — not
custom code.

The platform must feel **modern, premium, fast, and trustworthy**, while remaining simple enough for
café staff to use without training.

## Document map

| Document | Source chapter(s) | Purpose |
|---|---|---|
| [00_Vision.md](./00_Vision.md) | Ch 1, Ch 10 | Product vision, objectives, scope, success metrics, principles |
| [01_PRD.md](./01_PRD.md) | All | This index and canonical requirement map |
| [02_Website.md](./02_Website.md) | Ch 1, Ch 2 | Public marketing website + customer ordering experience |
| [03_Admin_Dashboard.md](./03_Admin_Dashboard.md) | Ch 3 | Restaurant dashboard / admin portal |
| [04_Backend.md](./04_Backend.md) | Ch 4 | Backend architecture & system design |
| [05_UI_UX.md](./05_UI_UX.md) | Ch 5 | UI/UX design system & frontend specification |
| [06_Database.md](./06_Database.md) | Ch 6 | Database design, tables, relationships, business rules |
| [07_Security.md](./07_Security.md) | Ch 7 | Security, DevSecOps & production readiness |
| [08_Deployment.md](./08_Deployment.md) | Ch 8 | Deployment, infrastructure & DevOps |
| [09_Testing.md](./09_Testing.md) | Ch 9 | Quality assurance, testing & acceptance criteria |
| [API.md](./API.md) | Ch 4, Ch 6 | REST API contract, endpoints, envelopes, validation |
| [ARCHITECTURE.md](./ARCHITECTURE.md) | Ch 4 | System architecture, tech stack, multi-tenancy, modules |
| [DESIGN_SYSTEM.md](./DESIGN_SYSTEM.md) | Ch 5 | Design tokens: type, color, spacing, radius, motion |
| [DATABASE.md](./DATABASE.md) | Ch 6 | Canonical schema reference (Prisma-oriented) |
| [ROADMAP.md](./ROADMAP.md) | Ch 1, Ch 3, Ch 4 | Scope, exclusions, future-ready features, phasing |
| [USER_FLOWS.md](./USER_FLOWS.md) | Ch 1, Ch 2, Ch 9 | End-to-end customer & staff journeys |
| [COMPONENT_LIBRARY.md](./COMPONENT_LIBRARY.md) | Ch 5 | Reusable component inventory & specs |
| [CODING_STANDARDS.md](./CODING_STANDARDS.md) | Ch 9, Ch 10 | Engineering constitution & code quality rules |
| [FOLDER_STRUCTURE.md](./FOLDER_STRUCTURE.md) | Ch 4, Ch 10 | Frontend & backend directory layout |
| [DEPLOYMENT.md](./DEPLOYMENT.md) | Ch 8 | Operational deploy runbook (focused view of 08) |
| [ENVIRONMENT_VARIABLES.md](./ENVIRONMENT_VARIABLES.md) | Ch 7, Ch 8 | Configuration & secrets reference |
| [TEST_PLAN.md](./TEST_PLAN.md) | Ch 9 | Test strategy, matrices, manual QA checklists |
| [ACCEPTANCE_CRITERIA.md](./ACCEPTANCE_CRITERIA.md) | Ch 9, Ch 10 | Definition of Done, release & launch readiness |
| [DECISIONS.md](./DECISIONS.md) | — | Structural decisions, inconsistencies, open questions |
| [CHANGELOG.md](./CHANGELOG.md) | — | Documentation & product change history |

Root-level: [`../README.md`](../README.md) (project overview) and
[`../CLAUDE.md`](../CLAUDE.md) (operating rules for AI/engineers).

### Overlapping documents (intentional)

Three topics have both a numbered chapter-faithful file and a focused working file:

- **Database:** [06_Database.md](./06_Database.md) (full chapter narrative) ↔ [DATABASE.md](./DATABASE.md) (schema reference).
- **Deployment:** [08_Deployment.md](./08_Deployment.md) (full chapter) ↔ [DEPLOYMENT.md](./DEPLOYMENT.md) (runbook).
- **Testing:** [09_Testing.md](./09_Testing.md) (full chapter) ↔ [TEST_PLAN.md](./TEST_PLAN.md) + [ACCEPTANCE_CRITERIA.md](./ACCEPTANCE_CRITERIA.md).

The numbered files are the requirement of record; the focused files are the day-to-day working views.
See [DECISIONS.md](./DECISIONS.md#d-002-overlapping-documents) for rationale.

## Source chapter → document coverage

- **Chapter 1 — Executive Summary** → 00_Vision, 02_Website, ARCHITECTURE, ROADMAP, USER_FLOWS
- **Chapter 2 — Customer Experience & Ordering** → 02_Website, USER_FLOWS, DESIGN_SYSTEM
- **Chapter 3 — Restaurant Dashboard** → 03_Admin_Dashboard, ROADMAP
- **Chapter 4 — Backend Architecture** → 04_Backend, ARCHITECTURE, API, FOLDER_STRUCTURE
- **Chapter 5 — UI/UX Design System** → 05_UI_UX, DESIGN_SYSTEM, COMPONENT_LIBRARY
- **Chapter 6 — Database & API** → 06_Database, DATABASE, API, USER_FLOWS
- **Chapter 7 — Security & DevSecOps** → 07_Security, ENVIRONMENT_VARIABLES
- **Chapter 8 — Deployment & DevOps** → 08_Deployment, DEPLOYMENT, ENVIRONMENT_VARIABLES
- **Chapter 9 — QA & Testing** → 09_Testing, TEST_PLAN, ACCEPTANCE_CRITERIA, USER_FLOWS
- **Chapter 10 — Engineering Constitution** → CODING_STANDARDS, FOLDER_STRUCTURE, ACCEPTANCE_CRITERIA, 00_Vision

## How to use these docs

1. **Before writing any code, read the whole system** — start at [00_Vision.md](./00_Vision.md), then the
   chapter relevant to your task, then the matching reference/working docs.
2. Treat this documentation as the project's **single source of truth**.
3. If a better implementation exists that stays aligned with the vision, propose it with rationale
   (log it in [DECISIONS.md](./DECISIONS.md)) before implementing.
4. If two parts of the PRD conflict, **pause and surface the conflict** rather than silently choosing —
   see the running list in [DECISIONS.md](./DECISIONS.md).
