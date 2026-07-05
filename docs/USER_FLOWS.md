# User Flows

_Consolidated from Master PRD Chapter 1 (customer journey), Chapter 2 (ordering), Chapter 3 (dashboard
order flow), and Chapter 9 (end-to-end journeys)._

## Customer journey (high level)

```
Landing Page
  ↓
Browse Menu
  ↓
Open Product
  ↓
Customize Item
  ↓
Add to Cart
  ↓
Review Cart
  ↓
Login (if required)
  ↓
Choose Address
  ↓
Payment
  ↓
Order Confirmation
  ↓
Live Status
  ↓
Order History
```

Expanded from Chapter 1: (1) land on homepage → (2) explore featured items or open full menu → (3)
select products → (4) customize if options exist → (5) review cart → (6) log in / continue per checkout
flow → (7) enter/select delivery address → (8) choose payment method → (9) complete payment → (10) see
order confirmation → (11) view order status → (12) access order history & reorder later.

**Target: complete an order in under 60 seconds.**

## Payment flow

```
Cart → Address → Payment → Confirmation
```

Server-side sequence (never trust the frontend to confirm payment):

1. Customer initiates payment.
2. Backend creates a Razorpay payment order (`POST /payments/create`).
3. Frontend completes payment.
4. Backend **verifies the signature** (`POST /payments/verify`).
5. Order is confirmed **only after** successful verification.
6. Confirmation returned to the client.

See [API.md](./API.md#checkout--payments).

## Live order status (customer view)

```
Order Received → Preparing → Ready → Out for Delivery → Delivered
```

Backend enum: `Pending → Accepted → Preparing → Ready → OutForDelivery → Delivered` (plus `Cancelled`,
`Refunded`). The restaurant updates statuses; customers see live updates.

## Reorder flow

One click from order history → cart fills automatically → proceed to checkout.

## Staff / dashboard order flow

```
New order arrives (instant, WebSocket, subtle sound + highlight)
  ↓
Accept (single click)  ──or──  Reject
  ↓
Order timeline: Placed → Accepted → Preparing → Ready → Out for Delivery → Delivered
  ↓
Update status / Print receipt / Call customer / add Kitchen note (internal)
```

Speed goals: accept/reject in one click; no unnecessary page transitions (slide-overs, inline editing,
real-time updates). See [03_Admin_Dashboard.md](./03_Admin_Dashboard.md#orders-module).

## Authentication flow

```
Guest browses → attempts checkout → Login/Register (if required) → returns to checkout
```

V1: email + password. Sessions via secure HTTP-only cookies (Secure/SameSite/HttpOnly); logout from all
devices supported. See [07_Security.md](./07_Security.md#authentication).

## Menu discovery flow

```
Menu page (sticky categories) → Search (instant, typo-tolerant) / Filter (Veg, Non-Veg, Available,
Price, Popular, Newest) → Product card → Product detail → Customize → Add to cart (slide-over)
```

## End-to-end test journey (from QA)

```
Visit Homepage → Browse Menu → Add Coffee → Checkout → Pay → Receive Confirmation → View Order
```

The entire flow must succeed. See [09_Testing.md](./09_Testing.md#end-to-end-tests) and
[TEST_PLAN.md](./TEST_PLAN.md).

## Error & edge-case flows

Every error explains **what happened** and **how to fix it**. Cover: payment failed · item unavailable ·
network lost · store closed · out of delivery range · expired session · order already accepted by
another staff member. Offline: keep cart, retry automatically, notify user. See
[02_Website.md](./02_Website.md#error-handling) and [09_Testing.md](./09_Testing.md#error-handling-tests).
