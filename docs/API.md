# API Specification

_Consolidated from Master PRD Chapters 4 (API design) and 6 (API standards & endpoints). This is the
canonical API contract reference. Business rules are in
[06_Database.md](./06_Database.md#business-rules)._

## Conventions

- **Base URL:** `/api/v1`
- **Style:** REST, resource-oriented, **versioned from the start**.
- **Status codes:** standard HTTP status codes.
- **Auth:** every endpoint verifies authentication **and** authorization (Auth → Role → Restaurant
  Ownership → Permission → Resource Ownership). Never rely on frontend permissions.
- **Validation:** validate input types · reject unknown fields · validate IDs · validate ownership ·
  validate restaurant/tenant context · return meaningful errors.

### Success envelope

```json
{ "success": true, "data": {}, "message": "Optional message" }
```

### Error envelope

```json
{
  "success": false,
  "error": {
    "code": "ORDER_NOT_FOUND",
    "message": "The requested order could not be found."
  }
}
```

Never expose stack traces or database errors. Error `code`s are stable machine-readable identifiers
(e.g. `PRODUCT_UNAVAILABLE`, `ORDER_NOT_FOUND`); `message` is human-friendly.

## Authentication APIs

| Method | Path | Purpose |
|---|---|---|
| POST | `/auth/register` | Create an account |
| POST | `/auth/login` | Authenticate |
| POST | `/auth/logout` | End session |
| POST | `/auth/refresh` | Refresh session/token |
| GET | `/auth/me` | Current authenticated identity |

Rate-limit `login`, `register`, and password reset aggressively (see
[07_Security.md](./07_Security.md#rate-limiting)).

## Customer APIs

| Method | Path | Purpose |
|---|---|---|
| GET | `/customer/profile` | Get profile |
| PATCH | `/customer/profile` | Update profile |
| GET | `/customer/orders` | List own orders |
| GET | `/customer/favorites` | List favorites |
| POST | `/customer/address` | Add address |
| PATCH | `/customer/address/:id` | Update address |
| DELETE | `/customer/address/:id` | Delete address |

Customers can only access their **own** orders and profile data.

## Menu APIs (public reads, cacheable)

| Method | Path | Purpose |
|---|---|---|
| GET | `/menu` | Full menu for the restaurant |
| GET | `/menu/categories` | Categories |
| GET | `/products` | Product list |
| GET | `/products/:slug` | Product detail by slug |
| GET | `/featured` | Featured collection |
| GET | `/search` | Instant, typo-tolerant search |

Cache menu, categories, featured, and settings; invalidate immediately on update.

## Cart APIs

| Method | Path | Purpose |
|---|---|---|
| GET | `/cart` | Get active cart |
| POST | `/cart/add` | Add item (validate availability) |
| PATCH | `/cart/update` | Update quantity/modifiers |
| DELETE | `/cart/item/:id` | Remove item |
| DELETE | `/cart` | Clear cart |

Unavailable products cannot be added; carts are revalidated at checkout.

## Checkout & payments

| Method | Path | Purpose |
|---|---|---|
| POST | `/checkout` | Create order (server computes all totals) |
| POST | `/payments/create` | Create Razorpay payment order |
| POST | `/payments/verify` | Verify payment signature server-side |
| GET | `/orders/:id` | Order detail / live status |

**Payment flow:** customer initiates → backend creates payment order → frontend completes → backend
**verifies signature** → order confirmed only after verification → confirmation returned. **Never trust
the frontend to confirm payment.** Totals and coupon eligibility are always recalculated on the server.

## Dashboard APIs (staff/manager, tenant-scoped)

| Method | Path | Purpose |
|---|---|---|
| GET | `/dashboard` | KPIs, charts, recent activity |
| GET | `/orders` | Order queue (filter/search) |
| PATCH | `/orders/:id/status` | Update order status |
| POST | `/products` | Create product |
| PATCH | `/products/:id` | Update product |
| DELETE | `/products/:id` | Delete product |
| GET | `/analytics` | Analytics data |
| GET | `/customers` | Customer list (tenant-scoped) |

Staff can only manage data belonging to their assigned restaurant. Real-time order updates use
WebSockets (see [03_Admin_Dashboard.md](./03_Admin_Dashboard.md#live-updates)).

## Validation rules (all endpoints)

Validate input types · reject unknown fields · validate IDs · validate ownership · validate restaurant
context · return meaningful errors. Reject: malformed JSON · oversized payloads · negative quantities ·
invalid prices · invalid coupon codes.

## Business rules affecting the API

- **Unavailable products** cannot be added to the cart; existing carts are revalidated at checkout.
- **Order totals** are always calculated on the server.
- **Coupon eligibility** is recalculated at checkout.
- **Payment verification** is required before marking prepaid orders confirmed.
- **Closed restaurants** cannot accept orders (unless future scheduled pre-orders).
- **Customers** access only their own orders/profile; **staff** only their assigned restaurant.

## Caching & rate limiting (API layer)

- **Cache:** menu, categories, featured products, restaurant settings.
- **Never cache:** payments, user profiles, active orders, sensitive account info.
- **Stricter rate limits:** login, registration, password reset, OTP (future), checkout, payment
  verification, admin APIs, search.

## Future-ready endpoints

Delivery-provider, ONDC, loyalty, and additional payment-gateway endpoints should live in **dedicated
integration modules** behind the same envelope and versioning conventions — not woven into core routes.
See [ROADMAP.md](./ROADMAP.md).
