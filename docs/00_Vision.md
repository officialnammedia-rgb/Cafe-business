# 00 · Product Vision & Objectives

_Source: Master PRD Chapter 1 (Executive Summary) and Chapter 10 (Final Product Vision)._

## Product vision

BrewOS is a premium white-label online ordering platform built specifically for cafés. Unlike
marketplace-based platforms such as Swiggy and Zomato, BrewOS enables cafés to **own their customer
relationships**, accept direct online orders through their own website, and manage operations from a
simple, elegant dashboard.

The first deployment will be for **AD'S COFFEE HOUSE**, but the system must be designed as a
**reusable SaaS platform** where new cafés can be onboarded with minimal configuration.

The platform must feel **modern, premium, fast, and trustworthy**. Every interaction should reinforce
the quality of the café's brand while remaining simple enough for staff to use without training.

> The goal is not to build "AD'S COFFEE HOUSE's website." The goal is to build **BrewOS** — a
> reusable, production-grade direct-ordering platform that can serve many cafés while delivering a
> premium customer experience. The first deployment should be so polished it becomes the **reference
> implementation** for every future café onboarded to the platform.

## Product objectives

**Customers should be able to:**

- Browse the café's menu.
- Search and filter products.
- Customize items where applicable.
- Place online orders.
- Pay securely.
- Track order status.
- Reorder previous purchases.
- Save favourite items.
- Manage addresses and profile details.
- View order history.
- Apply promotions or loyalty rewards if available.

**The café should be able to:**

- Manage menu items.
- Update prices.
- Mark items as available or unavailable.
- Receive and process incoming orders.
- Track order progress.
- View customer information relevant to their orders.
- Manage coupons and promotions.
- View basic analytics.
- Configure operating hours and settings.

**The platform administrator should be able to:**

- Manage cafés.
- Manage subscriptions (future).
- Monitor system health.
- Manage support requests.
- View logs and analytics.
- Configure platform-wide settings.

## Success metrics

Version 1.0 is successful if it achieves:

- Customers can place an order in **under 60 seconds**.
- Restaurant staff can accept an order **within 5 seconds**.
- **Lighthouse score above 95**.
- **Mobile performance above 90**.
- Website loads in **under 2 seconds** on a standard mobile connection.
- **No critical security vulnerabilities.**
- Fully responsive across desktop, tablet, and mobile.

## Scope

See [ROADMAP.md](./ROADMAP.md) for the full inclusion/exclusion list and future-ready features.

**Version 1.0 includes:** Marketing website · Customer ordering system · Authentication · Restaurant
dashboard · Payment gateway · Email notifications · Basic analytics · Menu management · Order
management · Responsive UI · SEO optimization.

**Version 1.0 excludes:** Native mobile apps · ONDC integration · AI recommendations · Loyalty program
· Subscription ordering · Kitchen display system · Franchise management · Delivery partner integration
(architecture should be ready for future integration).

## Product philosophy

The platform should **never feel like an enterprise dashboard**. It should feel like a **consumer
product**. Users should never wonder _"Where do I click?"_

- Every interaction should be obvious.
- Every page should have a clear purpose.
- Every animation should communicate intent.

## Design principles

Minimal · Elegant · Fast · Accessible · Mobile-first · Consistent · Secure · Performant · Delightful.

Avoid: visual clutter · unnecessary text · complex navigation.

## Inspiration (quality, not imitation)

Apple · Linear · Stripe · Raycast · EatClub (speed & simplicity) · Framer · Vercel · Notion (clarity &
spacing). **Do not copy any interface directly** — use these only as references for quality,
responsiveness, and clarity. The final product must have its own identity.

## Multi-tenant architecture (from day one)

The platform must support multiple cafés from day one. Each café has its own: branding · menu ·
products · customers (or shared customer accounts where appropriate) · orders · settings · analytics ·
domain (future). **All data must be isolated so one café cannot access another café's information.**
See [ARCHITECTURE.md](./ARCHITECTURE.md#multi-tenancy) and [07_Security.md](./07_Security.md).

## User roles

| Role | Can do | Cannot do |
|---|---|---|
| **Guest** | Browse website, menu, product details | Place an order until checkout requirements are met |
| **Customer** | Sign in, manage profile, save addresses, view/place/track orders, save favourites | Access other customers' data |
| **Staff** | Accept orders, update order status, mark items unavailable, view order queue | Access billing or platform settings (unless granted extra permissions) |
| **Restaurant Manager** | Manage menu, pricing, orders, staff; view analytics; configure business settings; create promotional offers | Delete the restaurant; access platform admin |
| **Platform Admin** | Manage restaurants & users, view platform analytics, configure platform settings, access audit logs, manage feature flags & support requests | — |

Detailed per-role permissions are in [07_Security.md](./07_Security.md#role-permissions).

## Website goals

The public website serves **two purposes**: (1) build trust in AD'S COFFEE HOUSE as a premium café,
and (2) convert visitors into direct online customers. **The website is not merely informational — it
is the primary sales channel.** Details in [02_Website.md](./02_Website.md).

## Product-first mandate (the "Head of Product" instruction)

Claude should **not simply implement the UI described in the PRD**. Before building each page,
evaluate whether the proposed experience can be improved while staying consistent with the product
vision. If a cleaner, faster, or more intuitive interaction exists, **implement the improved solution
and document the rationale** (in [DECISIONS.md](./DECISIONS.md)).

In other words: act as **Head of Product**, not a code generator. This permission to improve the
experience — rather than blindly following the spec — is one of the biggest advantages of building
with an AI engineer.

> Every decision should improve **User Experience**, **Developer Experience**, and **Business Value**.
> Never optimize for writing code quickly — optimize for software that can still be maintained
> **three years from now**.
