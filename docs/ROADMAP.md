# Roadmap & Scope

_Consolidated scope and future-ready features from Master PRD Chapters 1, 2, 3, 4, 6, and 7._

## Version 1.0 — in scope

Marketing website · Customer ordering system · Authentication · Restaurant dashboard · Payment gateway
(Razorpay) · Email notifications (Resend) · Basic analytics · Menu management · Order management ·
Responsive UI · SEO optimization.

**Multi-tenant from day one** — although V1 serves a single café (AD'S COFFEE HOUSE), the architecture
supports onboarding new cafés via configuration and branding, not custom code.

### V1 launch gate
See [ACCEPTANCE_CRITERIA.md](./ACCEPTANCE_CRITERIA.md#launch-readiness-checklist) and
[09_Testing.md](./09_Testing.md#release-criteria).

## Version 1.0 — explicitly excluded

Native mobile applications · ONDC integration · AI recommendations · Loyalty program · Subscription
ordering · Kitchen display system · Franchise management · Delivery-partner integration (architecture
should be **ready** for future integration).

## Future-ready features (do NOT implement in V1, but architect for)

From the dashboard chapter and elsewhere:

- Kitchen Display System (KDS)
- POS integration
- ONDC integration
- Delivery-partner APIs: Borzo / Shadowfax / Rapido
- Inventory synchronization
- Multi-location management
- QR table ordering
- Customer loyalty
- Gift cards
- Subscriptions
- Multiple branches / franchises

### Features marked "(future)" throughout the PRD
- **Auth:** Phone OTP, Google Sign-In, Apple Login, Magic Link, 2FA, API keys.
- **Payments:** Free-delivery coupon type; saved payments.
- **Notifications:** WhatsApp, SMS, push notifications (V1 is email only).
- **Customer:** Save-for-later cart, loyalty points.
- **Product:** Nutrition/calories display.
- **Analytics/export:** PDF export.
- **Billing/subscriptions:** platform subscription management.
- **Custom domains** per café.

## Guiding principle for phasing

Future integrations (delivery providers, ONDC, loyalty, additional payment gateways) must be built as
**modular extensions** behind stable interfaces — never woven through core business logic — so the core
platform stays clean and maintainable as it grows. See [ARCHITECTURE.md](./ARCHITECTURE.md).

## Suggested delivery phases (proposed — pending approval)

> This ordering is a proposal for discussion, not a PRD requirement. Confirm with the product owner and
> resolve the open questions in [DECISIONS.md](./DECISIONS.md) first.

1. **Foundation** — repo scaffolding (Next.js + NestJS), design system tokens, Prisma schema &
   migrations, auth, multi-tenancy guardrails, CI/CD skeleton.
2. **Menu & catalog** — categories, products, variants, images, public menu APIs + caching.
3. **Cart & checkout** — cart, single-page checkout, Razorpay create/verify, order creation.
4. **Order lifecycle & dashboard** — order queue, real-time updates, status transitions, menu
   management, store settings.
5. **Customer account** — profile, addresses, order history, reorder, favorites.
6. **Coupons & analytics** — coupon engine, dashboard analytics, exports.
7. **Hardening** — security review, performance/Lighthouse targets, accessibility, monitoring, backups,
   documentation, launch readiness.

Each phase ends only when its features meet the [Definition of Done](./ACCEPTANCE_CRITERIA.md).
