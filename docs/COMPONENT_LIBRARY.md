# Component Library

_Source: Master PRD Chapter 5 (Reusable Component Library, Buttons, Inputs, Cards, Navigation,
Micro-interactions). Tokens are in [DESIGN_SYSTEM.md](./DESIGN_SYSTEM.md)._

Build a **documented, reusable design system** shared across **both** the public website and the
restaurant dashboard. Prefer composition over large monolithic components; every component has a single
responsibility; if a component becomes hard to understand, split it. Before creating a new UI element,
**check whether an existing reusable component can be used** (see
[CODING_STANDARDS.md](./CODING_STANDARDS.md#ui-consistency)).

Foundation: React + TypeScript + Tailwind CSS + **shadcn/ui** + Framer Motion. Icons: **Lucide** only.

## Component inventory

### Primitives / form
| Component | Key requirements |
|---|---|
| Button | Variants: Primary (filled, large, rounded), Secondary (outline), Ghost, Text. **Loading + disabled states required.** Radius 12 px, 16 px text, hover scale 1.02 @150 ms. |
| Input | Large, comfortable, accessible; **labels always visible**; keyboard nav; autofill support. Radius 12 px. Inline validation. |
| Select | Accessible; keyboard nav. |
| Textarea | Comfortable sizing. |
| Checkbox | Accessible; visible focus. |
| Radio | Accessible; visible focus. |
| Toggle | Used for inventory available/unavailable (updates site instantly). |

### Overlays / feedback
| Component | Key requirements |
|---|---|
| Modal | Fade + scale motion; radius 20 px (dialog). |
| Drawer | Slide motion. Used for the cart slide-over. |
| Toast | Non-blocking notifications. |
| Tooltip | Accessible. |
| Badge | Status/labels (e.g. order status colors — subtle). |

### Layout / content
| Component | Key requirements |
|---|---|
| Card | Consistent padding, radius 16 px, consistent hover (tiny elevation + tiny scale). |
| Avatar | Customer/staff avatars. |
| Tabs | Accessible; keyboard nav. |
| Accordion | e.g. FAQs. |
| Breadcrumb | Dashboard navigation. |
| Pagination | For large tables (or use virtualization). |
| Data Table | Dashboard tables; pagination/virtualization; server-side search for large sets. |
| Skeleton | Loading placeholders resembling final layout. |
| Empty State | Illustration + message + CTA. |
| Error State | Human-language messaging. |

### Domain components
| Component | Key requirements |
|---|---|
| Product Card | Image · Title · Description · Price · Add button · Favorite button; opens Product Detail. |
| Menu Card | Menu-list item variant. |
| Cart Item | Quantity ± · modifiers · price · remove · undo. |
| Order Timeline | Placed → Accepted → Preparing → Ready → Out for Delivery → Delivered. |
| Analytics Card | Chart container for dashboard analytics. |
| KPI Card | Dashboard home metrics (Today's Orders, Revenue, etc.). |
| Search Bar | Instant, typo-tolerant results (products, categories, suggestions, recent searches). |
| Filter Chips | Veg · Non-Veg · Available · Price · Popular · Newest. |
| Navigation Bar | Sticky; hides on scroll down, reveals on scroll up; mobile = bottom nav. |
| Footer | Site footer with working links. |

## Buttons — detail

Primary (filled, large, rounded) · Secondary (outline) · Ghost · Text. **Loading states and disabled
states are required** for all.

## Inputs — detail

Large · comfortable · accessible · labels always visible · keyboard navigation · autofill support ·
inline validation with helpful messages (never validate only after submission).

## Cards — detail

Product · Order · Dashboard · Analytics. Consistent padding, radius, and hover across all.

## Navigation — detail

Sticky header that hides while scrolling down and reveals while scrolling up (smooth transition). Mobile
uses **bottom navigation**: Home · Menu · Cart · Orders · Profile.

## Micro-interactions

Button press animation · cart count animation · product-added confirmation · smooth page transitions ·
toast notifications · pull-to-refresh on mobile (where appropriate) · hover feedback on desktop · success
animations for completed actions. Micro-interactions **enhance usability, not distract** from it.

## Documentation expectation

Each component should be documented (props, variants, states, usage) and reused across website and
dashboard. Keep spacing, typography, colors, and interactions consistent everywhere.
