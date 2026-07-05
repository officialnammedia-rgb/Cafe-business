# 03 · Restaurant Dashboard (Admin Portal)

_Source: Master PRD Chapter 3._

## Objective

The Restaurant Dashboard is the **operational heart** of the platform. It should let café staff manage
the business with minimal training.

> **Primary goal:** any staff member should be able to learn the dashboard in **under 15 minutes**.

The dashboard should feel modern, lightweight, responsive, and never overwhelming.

## Design philosophy

Take inspiration from: **Linear · Stripe Dashboard · Vercel · Notion.**

Avoid: Bootstrap admin templates · heavy enterprise dashboards · too many colors · clutter · tiny
unreadable tables.

## Layout

**Desktop:**

```
──────────────────────────────
 Sidebar │ Header
         │
         │ Content
         │
──────────────────────────────
```

**Mobile:** bottom navigation **or** collapsible drawer.

### Sidebar
Dashboard · Orders · Menu · Categories · Customers · Coupons · Analytics · Store Settings · Staff ·
Profile · Logout. The sidebar should **collapse** and **remember its previous state**.

## Dashboard home

Shown immediately after login. Purpose: show everything important at a glance.

**Cards:** Today's Orders · Revenue Today · Pending Orders · Preparing · Completed · Cancelled · Average
Order Value · Store Status.

**Charts:** Orders Today · Revenue · Popular Products · Peak Hours · Top Customers.

**Quick Actions:** Accept Order · Pause Store · Add Product · Create Coupon · View Analytics.

**Recent Activity:** newest orders · newest reviews · staff actions.

## Orders module (most important module)

### Statuses (each with a subtle, non-overwhelming color)
New · Accepted · Preparing · Ready · Out for Delivery · Completed · Cancelled · Refunded.

### Order card
Order ID · Customer Name · Items · Payment Status · Delivery Address · Order Time · Amount · Current
Status.

### Actions
Accept · Reject · Print Receipt · Update Status · Call Customer.

### Live updates
Orders appear **instantly** with **no refresh required** — **WebSockets preferred**.

### Order details
Customer · Phone · Address · Items · Notes · Payment · Timeline.

### Order timeline
`Placed → Accepted → Preparing → Ready → Out for Delivery → Delivered`.

### Kitchen notes
Staff can add **internal** notes; customers cannot see them.

### Search orders
By: Order ID · Phone · Customer · Date · Status.

### Filters
Today · Yesterday · This Week · Completed · Cancelled · Pending.

### Bulk actions
Mark Ready · Cancel · Export.

## Menu management (simple + powerful)

### Products list
Image · Name · Category · Price · Availability · Popularity · Edit · Delete.

### Add product — fields
Name · Description · Category · Price · Discount Price · Images · Preparation Time · Availability · Tags.

### Images
Multiple · drag-and-drop · crop · compression · automatic optimization (stored in Cloudinary).

### Categories
Create · Rename · Delete · Reorder · Hide.

### Availability
Available · Out of Stock · Hidden · Seasonal.

### Variants (each configurable)
Small · Medium · Large · Hot · Cold · Milk Type · Sugar Level · Extra Shot · Extras.

### Inventory toggle
A simple switch: **Available / Unavailable**. Should **update the website instantly**.

## Customer module

Purpose: relationship management.

### Customer profile
Name · Phone · Email · Orders · Lifetime Spend · Average Order · Last Order · Favorite Items.

### Search customer
Phone · Email · Name.

### Customer notes (internal only)
VIP · Frequent · Refund Risk · Birthday.

## Coupons

### Create coupon — fields
Code · Discount · Percentage · Minimum Order · Maximum Discount · Expiry · Usage Limit · Applicable
Categories · Applicable Products.

### Coupon types
Percentage · Fixed Amount · Free Delivery (future).

## Analytics

**Overview:** Revenue · Orders · Conversion · Top Products · Repeat Customers · Peak Hours · Coupon
Usage.

**Reports:** Today · Week · Month · Year · Custom Range.

**Top Products by:** Sales · Quantity · Revenue.

**Customers:** Repeat % · New % · Returning %.

**Revenue graph:** Daily · Weekly · Monthly.

**Export:** CSV · Excel · PDF (future).

## Store settings

Store Name · Logo · Banner · Description · Phone · Email · GST · Address · Opening Hours · Holiday Hours
· Social Links.

### Store status
Open · Busy · Closed · Maintenance.

### Store hours
Per day (Monday, Tuesday, …) with support for **multiple time slots**, e.g. `9:00–14:00` and
`17:00–22:00`.

### Delivery radius
Configurable (e.g. 2 km / 5 km / 8 km). Future delivery integrations must respect this configuration.

### Minimum order value
Configurable.

### Preparation time
Default value, with per-product override.

## Staff management

Roles: **Owner · Manager · Cashier · Kitchen · Support.** Each role has configurable permissions.

Examples:
- **Kitchen:** can update order status; cannot edit menu pricing.
- **Manager:** can edit products and create coupons; cannot delete the restaurant.

Full permission matrix in [07_Security.md](./07_Security.md#role-permissions).

## Notifications

Notify the restaurant for: new order · payment failed · refund · store review · support message.

Delivery channels: in-app · email (configurable). Future-ready for WhatsApp and push notifications.

## Profile

Restaurant details · Password · 2FA (future) · API Keys (future) · Billing (future).

## Audit log

Track important actions, e.g.: product edited · price changed · coupon created · order cancelled · staff
invited · store closed. Each entry includes: **Who · What · When · IP (where appropriate).** See
[07_Security.md](./07_Security.md#audit-logging).

## Dashboard performance

Pages should load in **under 2 seconds**. Tables should use **pagination or virtualization**. Search
should be **server-side** for large datasets.

## Security (dashboard)

Managers cannot access platform-level administration. Staff permissions are enforced **server-side**.
Every action verifies **authorization, not just authentication**. Sensitive actions require
confirmation. Never expose internal IDs unnecessarily. Protect against CSRF, XSS, SQL injection, broken
access control, and mass assignment. Full detail in [07_Security.md](./07_Security.md).

## Error handling

Examples: payment gateway unavailable · product deleted while editing · network lost · order already
accepted by another staff member. **Gracefully inform the user and offer the next appropriate action.**

## Future-ready features (do NOT implement in V1)

Kitchen Display System (KDS) · POS Integration · ONDC Integration · Borzo / Shadowfax / Rapido API
integration · Inventory synchronization · Multi-location management · QR table ordering · Customer
loyalty · Gift cards · Subscriptions. See [ROADMAP.md](./ROADMAP.md).

## Claude instructions for the dashboard

- Build reusable dashboard components.
- Keep interfaces consistent across all modules.
- Prefer cards over dense tables where possible.
- Optimize for touch interactions on tablets.
- Design every screen to work well on **13-inch laptops** (many café managers use them).
- Ensure every module is keyboard accessible.
- Write modular code so future features can be added without major refactoring.

## Product improvement note — speed of operations

Beyond a typical restaurant dashboard, prioritize **speed of operations**:

- A new order appears **instantly** with a subtle (configurable) sound and visual highlight.
- Staff can accept or reject an order with a **single click**.
- Avoid unnecessary page transitions — use **slide-over panels, inline editing, and real-time
  updates** wherever appropriate.
- Common actions require the **fewest clicks possible**.

The objective is a dashboard that feels responsive and effortless during busy hours, where saving even
a few seconds per order makes a real operational difference.
