# 05 · UI / UX Design System & Frontend Specification

_Source: Master PRD Chapter 5. Design tokens are extracted into [DESIGN_SYSTEM.md](./DESIGN_SYSTEM.md);
the component inventory is in [COMPONENT_LIBRARY.md](./COMPONENT_LIBRARY.md)._

## Objective

The UI should make AD'S COFFEE HOUSE feel like **one of the most premium cafés in the city**. When a
customer visits, they should immediately think: _"This café cares about quality."_ The website should
communicate **trust, craftsmanship, warmth, and simplicity**, and the interface should disappear into
the background so customers can focus on discovering products and placing an order.

## Core design philosophy

Embody: Simplicity · Elegance · Warmth · Speed · Premium quality · Consistency.

Avoid: visual clutter · generic restaurant templates · excessive animations · overly dark interfaces ·
loud gradients · unnecessary decorative elements.

## Inspiration (quality, not imitation)

Apple · Linear · Raycast · Stripe · Framer · Vercel · EatClub (responsiveness & flow) · Notion (clarity
& spacing). **The final design must have its own identity.**

## Brand personality

Premium · Modern · Minimal · Welcoming · Comfortable · Warm · Trustworthy · Craft Coffee · Handcrafted ·
High-end.

## Visual language

Every screen should have: large spacing · large typography · large imagery · soft shadows · rounded
corners · generous white space · elegant animations · minimal color palette. **Never make the interface
feel crowded.**

## Typography

**Primary font:** Geist. **Fallback:** Inter, system fonts.

| Role | Size |
|---|---|
| Hero | 56–72 px |
| Section titles | 36–48 px |
| Card titles | 20–24 px |
| Body | 16–18 px |
| Small text | 14 px |
| Buttons | 16 px |
| Navigation | 15–16 px |

Use a consistent type scale. See [DESIGN_SYSTEM.md](./DESIGN_SYSTEM.md#typography).

## Color system

Evaluate the existing AD'S COFFEE HOUSE branding. If improvements are needed, **maintain
recognizability — do not redesign the brand.**

Potential palette: Primary **Coffee Brown** · Secondary **Cream** · Accent **Warm Beige** · Neutral
**White** · Surface **Soft Gray** · Text **Near Black** · Error **Muted Red** · Success **Muted Green** ·
Warning **Amber**. **Avoid saturated colors.** Tokens in
[DESIGN_SYSTEM.md](./DESIGN_SYSTEM.md#color).

## Grid system

12-column responsive layout. Max content width **1440 px**. Reading width **680–760 px**. Cards aligned
consistently.

## Spacing system

Base **4 px**. Scale: `4 · 8 · 12 · 16 · 24 · 32 · 48 · 64 · 96 · 128`. **Never use arbitrary spacing
values.**

## Border radius

Buttons **12 px** · Cards **16 px** · Dialogs **20 px** · Images **16 px** · Inputs **12 px**. Maintain
consistency.

## Shadows

Use **subtle elevation only** — no harsh drop shadows. Cards should appear gently lifted.

## Icons

Use **Lucide Icons**. Keep icon style consistent. **Never mix icon libraries.**

## Images

High-resolution photography only. No stock photos with visible watermarks. Prefer authentic café
imagery. All images: optimized · responsive · lazy-loaded · modern formats (WebP/AVIF where practical).

## Animation principles

Animation should **communicate state, not decorate**. Examples:

- **Button hover:** scale 1.02, duration 150 ms.
- **Modal:** fade + scale.
- **Drawer:** slide.
- **Page transition:** fade + slide.
- **Card hover:** tiny elevation + tiny scale.

Nothing excessive. (Customer-facing animation speed range: 150–250 ms — see
[02_Website.md](./02_Website.md#animations).)

## Loading experience

Never show blank pages. Use **skeleton screens** that resemble the final layouts.

## Empty states

Every empty state should help the user. Example — "No Orders Yet": illustration · message · CTA
(**Explore Menu**).

## Error states

Use **human language**; never expose technical messages.

- ❌ Bad: `500 Internal Server Error`
- ✅ Good: "Something went wrong while loading your order. Please try again."

## Forms

Minimal · clean · few fields · inline validation · helpful messages. **Never validate only after
submission.**

## Buttons

Variants: **Primary** (filled, large, rounded) · **Secondary** (outline) · **Ghost** · **Text**.
Loading states and disabled states required.

## Inputs

Large · comfortable · accessible · labels always visible · support keyboard navigation · support
autofill.

## Cards

Product Cards · Order Cards · Dashboard Cards · Analytics Cards. Consistent padding, radius, and hover.

## Navigation

Sticky header that **hides while scrolling down** and **reveals while scrolling up**, with a smooth
transition.

## Mobile navigation

**Bottom navigation.** Preferred tabs: Home · Menu · Cart · Orders · Profile.

## Cart UX

Floating cart button · sticky checkout button · live total updates · **no unnecessary redirects**.

## Checkout UX

**Single-page checkout.** Sections: Address · Items · Payment · Review · Confirmation. Minimal
scrolling.

## Accessibility

Meet **WCAG AA**. Requirements: keyboard navigation · visible focus states · semantic HTML · screen
reader compatibility · accessible color contrast · reduced-motion support.

## Responsive behaviour

Desktop · Tablet · Large Mobile · Small Mobile. **No horizontal scrolling. No broken layouts.** Test
matrix in [TEST_PLAN.md](./TEST_PLAN.md#responsive-testing).

## SEO

Server-side rendering where appropriate · metadata for every page · Open Graph support · structured
data · canonical URLs · XML sitemap · robots.txt.

## Performance guidelines

Minimize bundle size · use dynamic imports where appropriate · lazy-load heavy components · optimize
fonts · optimize images · avoid unnecessary client-side rendering.

## Reusable component library

Build a documented, reusable design system (usable across **both** the public website and the
dashboard). Full inventory and specs in [COMPONENT_LIBRARY.md](./COMPONENT_LIBRARY.md):

Button · Input · Select · Textarea · Checkbox · Radio · Toggle · Modal · Drawer · Toast · Tooltip ·
Badge · Card · Avatar · Tabs · Accordion · Breadcrumb · Pagination · Data Table · Skeleton · Empty
State · Error State · Product Card · Menu Card · Cart Item · Order Timeline · Analytics Card · KPI Card ·
Search Bar · Filter Chips · Navigation Bar · Footer.

## Micro-interactions

Subtle feedback that enhances usability (not distraction): button press animation · cart count
animation · product-added confirmation · smooth page transitions · toast notifications · pull-to-refresh
on mobile (where appropriate) · hover feedback on desktop · success animations for completed actions.

## Claude design instructions

Before implementing any page:

1. Evaluate the purpose of the page.
2. Remove unnecessary UI elements.
3. Reduce the number of clicks required to complete tasks.
4. Prioritize mobile usability.
5. Ensure consistent spacing and typography.
6. Optimize for accessibility.
7. Test responsive layouts.
8. Review animation performance.
9. Validate visual consistency against the design system.

Continuously ask: _"Can this page be simpler, faster, or clearer without reducing functionality?"_ If
yes, implement the improved design and document the reasoning in [DECISIONS.md](./DECISIONS.md).

## Final direction

The goal is not flashy visuals — it is an **effortless experience**. A customer should visit the site,
find what they want, place an order, and leave with the impression that the digital experience reflects
the same quality as the café itself.
