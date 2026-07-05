# 04 · Backend Architecture & System Design

_Source: Master PRD Chapter 4. System-level architecture and modules also appear in
[ARCHITECTURE.md](./ARCHITECTURE.md); the API contract is in [API.md](./API.md); the schema is in
[DATABASE.md](./DATABASE.md) / [06_Database.md](./06_Database.md)._

## Objective

The backend should be designed as if the platform will eventually serve **500+ cafés** and **hundreds
of thousands of orders**. Although V1 is only for AD'S COFFEE HOUSE, **every architectural decision
should support future growth without requiring a complete rewrite.** Prioritize simplicity,
maintainability, security, and scalability over premature complexity.

## Architecture principles

The system must be: **Modular · Secure by default · Multi-tenant · API-first · Stateless where
practical · Horizontally scalable · Easy to maintain · Easy to test.**

Avoid over-engineering. Build clean abstractions that can evolve.

## High-level system architecture

```text
Customer Website (Next.js)
        │
        ▼
   REST / Secure API
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
Cloudinary   Email Jobs / Notifications
```

Future integrations (Borzo, ONDC, WhatsApp, analytics, etc.) should connect through **dedicated
integration modules** rather than being tightly coupled to core business logic.

## Project structure (feature-based modules)

Example modules: Authentication · Users · Restaurants · Menu · Categories · Products · Orders ·
Customers · Coupons · Payments · Analytics · Notifications · Uploads · Settings · Staff · Audit Logs.

Each module encapsulates: **Controller · Service · Repository · DTOs · Validation · Tests ·
Documentation.** **Business logic lives in services, not controllers.** See directory layout in
[FOLDER_STRUCTURE.md](./FOLDER_STRUCTURE.md).

## Multi-tenancy

Every record must belong to a restaurant (tenant), except platform-wide entities:

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
be impossible.** See [07_Security.md](./07_Security.md#authorization).

## Database

Primary database: **PostgreSQL**. ORM: **Prisma**. Full schema (fields, indexes, relationships,
constraints, soft deletes, audit fields) is in [DATABASE.md](./DATABASE.md) and
[06_Database.md](./06_Database.md).

### Core entities (summary)
Restaurant · User · Customer Profile · Staff · Category · Product · Product Variant · Order · Order
Items · Coupons · Payments · Audit Logs. Field-level definitions in
[DATABASE.md](./DATABASE.md#core-tables).

## Authentication

**Preferred:** Better Auth (or an equivalent secure authentication system). Support: Email login ·
Phone OTP (future) · Google Sign-In (future).

- Passwords must **never** be stored in plain text; store only strong hashes using modern algorithms.
- Sessions must be secure, expire appropriately, and support **logout from all devices**.

> Note: the PRD also references JWT-style sessions and a `JWT_SECRET`. The primary mechanism (Better
> Auth vs. custom JWT) is an open decision — see
> [DECISIONS.md](./DECISIONS.md#i-003--authentication-better-auth-vs-jwt-env-vars).

## Authorization

**Role-Based Access Control (RBAC).** Roles: Guest · Customer · Staff · Manager · Platform Admin.

**Every endpoint must verify both authentication and authorization. Never rely on frontend
permissions.** Full matrix in [07_Security.md](./07_Security.md#role-permissions).

## API design

REST, **versioned** (`/api/v1/...`), resource-oriented. Examples: `GET /products`, `POST /orders`,
`PATCH /orders/{id}`, `DELETE /coupons/{id}`. Use standard HTTP status codes, consistent response
formats, and meaningful error messages **without leaking internal implementation details**. Full
contract in [API.md](./API.md).

## Validation

Every input is validated on the server using **DTOs with schema validation**. Reject malformed
requests and unexpected fields. **Never trust client-side validation.**

## Error handling

Return **structured errors**, e.g.:

```json
{
  "error": {
    "code": "PRODUCT_UNAVAILABLE",
    "message": "The selected item is currently unavailable."
  }
}
```

Never expose stack traces or database errors to clients.

## Payments

V1: **Razorpay**. Flow:

1. Customer initiates payment.
2. Backend creates a payment order.
3. Frontend completes payment.
4. Backend verifies the payment signature.
5. Order is confirmed **only after successful verification**.
6. Confirmation is returned to the client.

**Never trust the frontend to confirm a successful payment.** See
[07_Security.md](./07_Security.md#payment-security).

## Notifications

V1: **email confirmations** (via Resend). Future-ready architecture for WhatsApp, SMS, and push
notifications. **Notification generation is asynchronous** using background jobs (BullMQ).

## File storage

Use **Cloudinary** for product images, restaurant logos, and banners. **Do not store uploaded files on
the application server.** Validate file types and sizes before upload. See
[07_Security.md](./07_Security.md#file-upload-security).

## Redis

Use Redis for: caching frequently accessed data · sessions (if applicable) · rate limiting · temporary
tokens · frequently requested menu data. **Avoid caching sensitive or user-specific data without proper
safeguards.** Caching strategy detail in [06_Database.md](./06_Database.md#caching-strategy).

## Background jobs

Use **BullMQ** for: sending emails · cleaning expired data · future notification processing · image
optimization · analytics aggregation. **Long-running tasks must never block user requests.**

## Logging

Implement **structured logging**. Log: application errors · authentication events · order lifecycle
events · payment events · critical administrative actions. **Never log passwords, payment credentials,
or other secrets.**

## Security requirements

Security is a product requirement, not optional. Protect against SQL injection, XSS, CSRF, broken
access control, authentication attacks, mass assignment, clickjacking, and brute-force logins. Use
secure HTTP headers, enforce HTTPS in production, rate-limit sensitive endpoints, validate uploads,
keep secrets in env vars, implement proper CORS, protect cookies, and validate all authorization on the
server. **Review the codebase for security issues throughout development, not as a final step.** Full
detail in [07_Security.md](./07_Security.md).

## Performance targets

- Cached reads: **under 100 ms** where practical.
- Standard reads: **under 300 ms**.
- Order creation: **under 1 second** under expected load.

Optimize and index database queries; avoid N+1 problems; paginate large datasets; lazy-load expensive
relationships where possible.

## Testing

Every module includes: **unit tests** (business logic), **integration tests** (critical workflows), and
**end-to-end tests** (essential customer journeys). Critical flows requiring automated tests:
Authentication · Checkout · Payment verification · Order creation · Order status updates. See
[09_Testing.md](./09_Testing.md) and [TEST_PLAN.md](./TEST_PLAN.md).

## Claude implementation guidelines

- Build reusable services and components.
- Keep business logic independent of UI.
- Avoid hardcoded values; use configuration files where appropriate.
- Prefer composition over inheritance.
- Document key architectural decisions (in [DECISIONS.md](./DECISIONS.md)).
- Write code that is easy for another engineer to understand and extend.
- Optimize for long-term maintainability over short-term shortcuts.

## Product direction

Although V1 focuses on AD'S COFFEE HOUSE, every technical decision should support evolving BrewOS into a
multi-restaurant platform: new cafés onboarded via **configuration and branding**, not custom code.
Future integrations (delivery providers, ONDC, loyalty, additional payment gateways) should be
**modular extensions** to the core platform rather than requiring codebase-wide changes.
