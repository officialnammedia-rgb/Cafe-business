# 02 · Public Website & Customer Ordering Experience

_Source: Master PRD Chapter 1 (Website Structure/Goals/Landing redesign) and Chapter 2 (Customer
Experience & Ordering System). Visual/design rules live in [05_UI_UX.md](./05_UI_UX.md) and
[DESIGN_SYSTEM.md](./DESIGN_SYSTEM.md); flows are diagrammed in [USER_FLOWS.md](./USER_FLOWS.md)._

## Overview & primary design goal

The ordering experience is **the most important part of the product**. Customers should complete an
order in **under 60 seconds**. Every interaction should feel effortless — no unnecessary clicks, no
confusing pages, no information overload. The interface should feel closer to **EatClub, Apple, and
Linear** than a traditional restaurant website.

> **Primary design goal:** the customer should never have to think. Every next action should be
> obvious.

## Website goals

The public website serves two purposes: **build trust** in AD'S COFFEE HOUSE as a premium café, and
**convert visitors into direct online customers**. It is **not merely informational — it is the primary
sales channel**.

The homepage should answer four questions immediately:

1. Who are we?
2. What do we sell?
3. Why should I order directly?
4. How do I order?

## Website structure (sections)

Sticky navigation · Hero · Featured products · Best sellers · About the café · Signature drinks ·
Seasonal menu · Why order direct? · Customer reviews · Instagram gallery · Location and hours · FAQs ·
Footer.

## Landing-page redesign requirements

Review the **existing landing page before making changes**. Preserve elements that already communicate
the brand effectively; improve areas where UX, conversion flow, or visual hierarchy can be enhanced.

Focus the redesign on: faster access to ordering · better storytelling · stronger CTAs · improved
mobile experience · improved accessibility · better SEO · cleaner spacing and typography · premium
visual presentation.

Avoid unnecessary animations, excessive effects, or visual noise. **Every design decision should
support usability and performance.**

## Responsive design

Mobile-first. Supported devices: Mobile · Tablet · Laptop · Desktop. **The mobile experience receives
the highest priority.**

## Navigation bar

**Desktop navigation:** Logo · Home · Menu · About · Contact · Order Now · Cart · Profile.

**Mobile navigation:** Minimal · Sticky · Animated. **Bottom navigation preferred during ordering**
(tabs: Home · Menu · Cart · Orders · Profile — see [05_UI_UX.md](./05_UI_UX.md#mobile-navigation)).

## Hero section

Requirements: large premium photography · headline · subheadline · **primary CTA: Order Now** ·
**secondary CTA: View Menu** · small delivery information · store timings · rating · location.

## Featured products

Horizontal scrolling cards. Each displays: image · name · price · rating (optional) · **Quick Add**
button.

## Best sellers

Automatically configurable. Admin can choose the collection to surface: Top Picks · New · Trending ·
Seasonal · Limited Edition.

## Why order direct

Explain the benefits, e.g.: better offers · exclusive rewards · fresh preparation · support local
business · lower prices than marketplaces (if applicable).

## Menu system (the most important page)

### Categories
Coffee · Hot Beverages · Cold Coffee · Tea · Desserts · Bakery · Sandwiches · Meals · Snacks ·
Seasonal. Categories remain **sticky while scrolling**. (In production these are admin-managed and
per-restaurant — see [DECISIONS.md](./DECISIONS.md#i-007--category-list-vs-dynamic-categories).)

### Search
Fast · Instant · Typo tolerant.

### Filters
Veg · Non-Veg · Available · Price · Popular · Newest.

### Product card
Image · Title · Description · Price · **Add** button · **Favorite** button. Clicking a card opens
**Product Details**.

## Product detail page

Contains: large image · description · ingredients · allergens (optional) · nutrition (future) ·
customization · quantity selector · **Add to Cart** · related items.

### Customization
Supports: Size · Milk Type · Sugar Level · Extra Shot · Ice Level · Add-ons. **Each option is
configurable by admin.**

### Quantity selector
Simple `+` / `-` control.

### Add-to-cart animation
Smooth · fast · cart icon updates instantly · **no full page reload**.

## Cart

A **slide-over panel** — never redirect unnecessarily.

**Shows:** Items · Quantity · Modifiers · Price · Subtotal · Taxes · Delivery Fee · Discount · Grand
Total · Estimated Delivery · **Checkout** button.

**Features:** increase quantity · decrease quantity · remove item · **undo remove** · clear-cart
confirmation · save for later (future).

> **Security:** totals are always computed on the server — never trust client-side prices. See
> [06_Database.md](./06_Database.md#business-rules) and [07_Security.md](./07_Security.md#payment-security).

## Checkout

Minimal. **Single-page checkout preferred.**

### Address
Current location · saved addresses · new address · **Google Maps autocomplete**.

### Delivery instructions (optional)
Examples: "Call on arrival." · "Leave at gate." · "Extra napkins."

### Payment methods (Razorpay)
UPI · Cards · Net Banking · Wallets · Cash on Delivery (optional — see
[DECISIONS.md](./DECISIONS.md#i-006--cash-on-delivery-cod)).

### Payment flow
`Cart → Address → Payment → Confirmation`. **Never ask for unnecessary information.** Payment is
verified server-side before an order is confirmed (see [API.md](./API.md#checkout--payments)).

## Order confirmation

Large success animation · Order ID · estimated time · items · total · payment status.
CTAs: **Track Order** · **Return Home**.

## Live order status

Timeline: `Order Received → Preparing → Ready → Out for Delivery → Delivered`. The restaurant updates
these statuses. (Backend enum uses `Pending / Accepted / Preparing / Ready / OutForDelivery /
Delivered / Cancelled / Refunded` — see [DATABASE.md](./DATABASE.md#order-status).)

### Notifications
V1: **email confirmation**. Future: WhatsApp confirmation, SMS (optional), push notifications.

## Account

Customer can manage: Profile · Addresses · Orders · Favorites · Saved Payments (future) · Settings ·
Logout.

### Order history
Every previous order shows: Date · Items · Total · Status · Invoice · **Reorder** button.

### Reorder
One click — the cart fills automatically.

### Favorites
Heart icon, available on: cards · product page · search.

### Recently viewed
Show the last 10 products (client-side in V1 — see
[DECISIONS.md](./DECISIONS.md#i-008--recently-viewed-last-10-products-persistence)).

### Search experience
Instant results showing: products · categories · suggestions · recent searches.

## States, accessibility & performance (customer-facing)

### Empty states
Never leave blank screens. Examples: "No orders yet → Explore Menu." · "No favorites → Start adding
your favorite drinks."

### Loading states
**Skeleton loaders.** Never use spinning loaders unless absolutely necessary.

### Error handling
Examples: payment failed · item unavailable · network lost · store closed · out of delivery range.
**Every error explains what happened and how to fix it.**

### Accessibility
Keyboard navigation · proper focus states · ARIA labels · screen-reader friendly · minimum contrast
ratio. Target **WCAG AA** (see [05_UI_UX.md](./05_UI_UX.md#accessibility)).

### Animations
Use **Framer Motion**. Speed **150–250 ms**. Never block interaction. Prefer subtle fades, slides, and
scale transitions; avoid excessive bouncing or flashy effects.

### Performance
Images: lazy load · responsive sizes · WebP/AVIF. Caching: use Redis where applicable. Prefetch: menu ·
product · cart. Optimization: no unnecessary API calls; minimize JavaScript.

### Offline handling
If the connection drops: keep the cart · retry automatically · notify the user.

### Security (customer-facing summary)
Never trust client-side prices; the server calculates totals. Validate inventory, coupons, taxes, and
payment. Full detail in [07_Security.md](./07_Security.md).

### SEO
Each product has: unique URL · meta title · description · Open Graph · structured data. See
[05_UI_UX.md](./05_UI_UX.md#seo).

### Analytics events
Track: homepage visit · menu opened · product viewed · add to cart · checkout started · payment success
· payment failure · order completed. Design events so analytics tools can be integrated later without
major refactoring.

## Product-first note (Chapter 2)

Before building each page, evaluate whether the proposed experience can be improved while staying
consistent with the vision. If a cleaner/faster/more intuitive interaction exists, implement it and
document the rationale in [DECISIONS.md](./DECISIONS.md). Act as **Head of Product**, not a code
generator.
