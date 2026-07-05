# System Architecture

_Consolidated from Master PRD Chapter 1 (tech stack, multi-tenancy) and Chapter 4 (backend
architecture). Directory layout is in [FOLDER_STRUCTURE.md](./FOLDER_STRUCTURE.md)._

## Architecture principles

The system must be **Modular · Secure by default · Multi-tenant · API-first · Stateless where practical
· Horizontally scalable · Easy to maintain · Easy to test.** Avoid over-engineering; build clean
abstractions that can evolve.

Designed as if it will eventually serve **500+ cafés** and **hundreds of thousands of orders**, even
though V1 targets a single café.

## Technology stack

### Frontend
Next.js (App Router) · React · TypeScript · Tailwind CSS · shadcn/ui · Framer Motion · React Hook Form ·
Zod.

### Backend
NestJS · TypeScript · Prisma ORM · PostgreSQL · Redis · BullMQ.

### Platform services
- **Storage:** Cloudinary
- **Maps:** Google Maps Platform
- **Email:** Resend
- **Payments:** Razorpay
- **Auth:** Better Auth (or equivalent — open decision, see
  [DECISIONS.md](./DECISIONS.md#i-003--authentication-better-auth-vs-jwt-env-vars))

### Deployment
Docker · Coolify (preferred self-hosting) or a comparable production-ready platform. See
[08_Deployment.md](./08_Deployment.md).

## High-level system diagram

```text
Customer Website (Next.js)
        │
        ▼
   REST / Secure API  (/api/v1)
        │
        ▼
   NestJS Backend
        │
 ┌──────┼─────────┐
 │      │         │
 ▼      ▼         ▼
PostgreSQL  Redis  BullMQ
 │                 │
 ▼                 ▼
Cloudinary   Email Jobs / Notifications (Resend)
```

External services (Google Maps, Razorpay) integrate at the backend boundary. **Future integrations
(Borzo, ONDC, WhatsApp, analytics) connect through dedicated integration modules**, never tightly
coupled to core business logic.

## Feature-based modules

Each backend feature is a self-contained module encapsulating **Controller · Service · Repository · DTOs
· Validation · Tests · Documentation**. Business logic lives in **services, not controllers**.

Modules: Authentication · Users · Restaurants · Menu · Categories · Products · Orders · Customers ·
Coupons · Payments · Analytics · Notifications · Uploads · Settings · Staff · Audit Logs.

## Multi-tenancy

Every record belongs to a restaurant (tenant), except platform-wide entities:

```
Restaurant
 ├── Products
 ├── Categories
 ├── Orders
 ├── Coupons
 ├── Staff
 └── Settings
```

**Every API request must verify tenant ownership before accessing data. Cross-tenant data leakage must
be impossible.** New cafés are onboarded through **configuration and branding**, not custom code.
(Customer identity may be global while cart/order data is per-restaurant — see
[DECISIONS.md](./DECISIONS.md#i-005--customer-scoping-across-tenants).)

## Data layer

**PostgreSQL** via **Prisma**. UUID primary keys, `createdAt`/`updatedAt` everywhere, `deletedAt` soft
deletes where appropriate, FK constraints, indexes on hot fields. Full schema in
[DATABASE.md](./DATABASE.md).

## Caching (Redis)

Cache frequently accessed menu data, categories, featured products, and restaurant settings; use for
rate limiting, temporary tokens, and sessions if applicable. Never cache payments, user profiles,
active orders, or sensitive data without safeguards. Invalidate immediately after updates.

## Background jobs (BullMQ)

Asynchronous work: sending emails · cleaning expired data · notification processing · image optimization
· analytics aggregation. Long-running tasks must never block user requests.

## Real-time

The dashboard receives new orders instantly via **WebSockets** (preferred) — no refresh required.

## Cross-cutting concerns

- **Security:** enforced server-side at every layer — see [07_Security.md](./07_Security.md).
- **Validation:** DTO + schema (Zod) on every input.
- **Errors:** structured envelopes, no internal detail leakage — see [API.md](./API.md).
- **Observability:** structured logs with correlation IDs; health endpoints; monitoring & alerting —
  see [08_Deployment.md](./08_Deployment.md#monitoring).

## Performance targets (system)

- Cached reads: under 100 ms where practical.
- Standard reads: under 300 ms.
- Order creation: under 1 second under expected load.
- Homepage / menu / dashboard: under 2 seconds.

## Extensibility principle

Future integrations — delivery providers, ONDC, loyalty, additional payment gateways — are designed as
**modular extensions** behind stable interfaces, so the core stays clean, maintainable, and ready for
growth. See [ROADMAP.md](./ROADMAP.md).
