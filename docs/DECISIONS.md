# Decisions & Open Questions (ADR-style)

This log records **documentation-structure decisions**, **inconsistencies found in the source PRD**,
and **open questions** that need the product owner's input. Per the Engineering Constitution
([CODING_STANDARDS.md](./CODING_STANDARDS.md)), when a requirement is ambiguous we make the safest
reasonable assumption and document it here; when two parts of the PRD conflict, we pause and surface
the conflict rather than silently choosing.

No functionality from the PRD has been changed. Everything below is either a documentation-organization
choice or a flagged item for confirmation.

---

## Structural decisions

### D-001 · Chapter-faithful reorganization into 25 docs
The 10-chapter PRD was split into focused documents so each concern has a single home. The mapping is
in [01_PRD.md](./01_PRD.md#source-chapter--document-coverage). Nothing was summarized away —
long lists were preserved verbatim in their home document.

### D-002 · Overlapping documents
Three subjects have both a numbered chapter file and a focused working file:
Database (`06_Database.md` / `DATABASE.md`), Deployment (`08_Deployment.md` / `DEPLOYMENT.md`),
Testing (`09_Testing.md` / `TEST_PLAN.md` + `ACCEPTANCE_CRITERIA.md`). **Decision:** the numbered file
is the requirement of record (full chapter narrative); the focused file is the day-to-day working view
and links back to the numbered file. This satisfies the requested folder structure without duplicating
authority.

### D-003 · README and CLAUDE at repo root
The requested `README.md` and `CLAUDE.md` were placed at the repository root (not inside `docs/`) so
they are discovered by GitHub and by Claude Code automatically. Both link into `docs/`.

---

## Inconsistencies & ambiguities found in the source PRD

> These are flagged for confirmation. None were "resolved" by changing intended functionality.

### I-001 · Brand name mismatch: "AD'S COFFEE HOUSE" vs. existing "Coffee Under Palms" ⚠️
The PRD names the first client **AD'S COFFEE HOUSE**, but the existing repository is a static site for
**Coffee Under Palms (CUP)** in Vasant Kunj, New Delhi (`index.html`, `README.md`). These appear to be
different brand names for the same first deployment, or the PRD is a generic template.
**Question:** Which brand name is authoritative for V1? All docs currently preserve "AD'S COFFEE HOUSE"
as written in the PRD, with the existing site treated as the "existing landing page" to review per
Chapter 1's landing-page-redesign instruction.

### I-002 · Greenfield rebuild vs. evolving the existing static site ⚠️
The PRD prescribes a **Next.js + NestJS** stack, but the repo today is hand-written HTML/CSS/JS with no
build step. **Question:** Is V1 a ground-up rebuild on the prescribed stack (recommended by the PRD's
architecture chapters), or an incremental migration of the current static site? Assumption pending
confirmation: **greenfield build on the prescribed stack**, reusing brand/content/imagery from the
current site.

### I-003 · Authentication: "Better Auth" vs. JWT env vars
Chapter 4 lists **Better Auth (or an equivalent secure authentication system)** as preferred, while
Chapters 7–8 reference a `JWT_SECRET` env var and JWT-style sessions. These are compatible (Better Auth
can issue JWTs / use secure cookies) but the intended primary mechanism should be confirmed. Docs
describe both and mark the choice as open. See [07_Security.md](./07_Security.md#authentication).

### I-004 · Delivery vs. pickup model is under-specified
The customer flow, addresses, delivery fee, delivery radius, and "Out for Delivery" status all imply
**delivery**, yet V1 explicitly **excludes delivery-partner integration**. **Question:** In V1, does
the café self-deliver, is it pickup-only, or is "delivery" a status the café fulfills manually?
Assumption pending confirmation: **café-managed delivery/pickup with manual status updates**;
delivery-partner APIs are future-ready only.

### I-005 · Customer scoping across tenants
Chapter 1 says each café has "its own customers (**or shared customer accounts where appropriate**)".
The data model ties `Customer` to a `User` (global auth identity) but orders/carts are per-restaurant.
**Question:** Are customer accounts global (one login across all cafés) or per-café? Assumption:
**global `User` identity, per-restaurant `Cart`/`Order` scoping**, matching the schema in
[DATABASE.md](./DATABASE.md).

### I-006 · Cash on Delivery (COD)
COD is listed as an **optional** payment method in Chapter 2, while Chapter 6 business rules require
server-side payment **verification before confirming prepaid orders**. COD has no gateway verification.
Assumption: **COD is disabled in V1 unless explicitly enabled per café**; if enabled, COD orders skip
gateway verification but still validate totals/inventory server-side.

### I-007 · Category list vs. dynamic categories
Chapter 2 lists fixed categories (Coffee, Hot Beverages, Cold Coffee, Tea, Desserts, Bakery,
Sandwiches, Meals, Snacks, Seasonal), but Chapter 3/6 make categories **admin-managed and
per-restaurant**. Assumption: the fixed list is **seed/default data** for AD'S COFFEE HOUSE, not a
hard-coded constraint.

### I-008 · "Recently viewed last 10 products" persistence
Chapter 2 requires showing the last 10 viewed products but no table stores this. Assumption:
**client-side storage** (localStorage) in V1; no schema change required. Flagged in case
server-side persistence is desired.

### I-009 · Reviews / ratings source
Product cards and dashboard "newest reviews" reference ratings/reviews, but no `Review` table exists in
the Chapter 6 schema, and ratings are marked "optional." Assumption: **ratings/reviews are out of scope
for V1 data model**; any rating shown is static/optional. Flagged for a future `Review` entity.

### I-010 · Better-Auth managed tables vs. custom `User`
If Better Auth is adopted, it manages its own session/account tables which may overlap with the custom
`User` table in Chapter 6. Reconcile during schema implementation. See
[DATABASE.md](./DATABASE.md#authentication-tables).

---

## Open questions for the product owner

1. Authoritative brand name for V1 (I-001).
2. Greenfield rebuild vs. incremental migration (I-002).
3. Primary auth mechanism: Better Auth vs. custom JWT (I-003).
4. Delivery model for V1: self-delivery, pickup, or hybrid (I-004).
5. Global vs. per-café customer accounts (I-005).
6. Is COD enabled at launch (I-006)?
7. Deployment target: Coolify self-host vs. managed platform (Chapter 8 says "Coolify preferred or a
   comparable production-ready platform").

---

## How to add an entry

Append a new `D-###` (decision) or `I-###` (inconsistency/issue) with: date, context, the options
considered, and the decision or the assumption made pending confirmation. Link to the affected
documents.
