# Design System (Tokens)

_Design tokens extracted from Master PRD Chapter 5. Narrative UI/UX guidance is in
[05_UI_UX.md](./05_UI_UX.md); the component inventory is in
[COMPONENT_LIBRARY.md](./COMPONENT_LIBRARY.md)._

This document is the **single source of truth for design tokens**. Implement these as Tailwind theme
values / CSS variables so both the public website and the dashboard share one system. Note: the current
static site (`styles.css`) already defines a warm palette and Fraunces/Cormorant/Inter/DM Mono type —
reconcile with these tokens during implementation (see
[DECISIONS.md](./DECISIONS.md#i-002--greenfield-rebuild-vs-evolving-the-existing-static-site)).

## Typography

**Primary font:** Geist. **Fallback:** Inter, system fonts. Use a consistent type scale.

| Token | Role | Size |
|---|---|---|
| `text-hero` | Hero | 56–72 px |
| `text-section` | Section titles | 36–48 px |
| `text-card-title` | Card titles | 20–24 px |
| `text-body` | Body | 16–18 px |
| `text-small` | Small text | 14 px |
| `text-button` | Buttons | 16 px |
| `text-nav` | Navigation | 15–16 px |

## Color

Evaluate existing brand colors; **maintain recognizability, do not redesign the brand.** Avoid saturated
colors.

| Token | Role | Suggested value |
|---|---|---|
| `--color-primary` | Primary | Coffee Brown |
| `--color-secondary` | Secondary | Cream |
| `--color-accent` | Accent | Warm Beige |
| `--color-neutral` | Neutral | White |
| `--color-surface` | Surface | Soft Gray |
| `--color-text` | Text | Near Black |
| `--color-error` | Error | Muted Red |
| `--color-success` | Success | Muted Green |
| `--color-warning` | Warning | Amber |

> Exact hex values should be derived from the AD'S COFFEE HOUSE / Coffee Under Palms brand during
> implementation and locked here. Ensure all foreground/background pairings meet **WCAG AA** contrast.

## Spacing

Base unit **4 px**. Scale (never use arbitrary values):

`4 · 8 · 12 · 16 · 24 · 32 · 48 · 64 · 96 · 128`

## Grid & layout

- 12-column responsive layout.
- Max content width: **1440 px**.
- Reading width: **680–760 px**.
- Cards aligned consistently.

## Border radius

| Element | Radius |
|---|---|
| Buttons | 12 px |
| Inputs | 12 px |
| Cards | 16 px |
| Images | 16 px |
| Dialogs | 20 px |

## Shadows / elevation

Subtle elevation only — **no harsh drop shadows**. Cards appear gently lifted.

## Icons

**Lucide Icons** only. Consistent style. **Never mix icon libraries.**

## Imagery

High-resolution, authentic café photography. Optimized · responsive · lazy-loaded · WebP/AVIF where
practical. No watermarked stock photos.

## Motion

Animation **communicates state, not decoration**. Framer Motion.

| Interaction | Motion | Duration |
|---|---|---|
| Button hover | scale 1.02 | 150 ms |
| Modal | fade + scale | ~150–250 ms |
| Drawer | slide | ~150–250 ms |
| Page transition | fade + slide | ~150–250 ms |
| Card hover | tiny elevation + tiny scale | ~150 ms |

General range **150–250 ms**; never block interaction; respect **reduced-motion** preferences. Avoid
excessive bouncing/flashy effects.

## State patterns

- **Loading:** skeleton screens resembling the final layout — never spinners unless unavoidable.
- **Empty:** illustration + message + CTA (e.g. "No orders yet → Explore Menu").
- **Error:** human language, never technical (e.g. "Something went wrong while loading your order.
  Please try again.").

## Accessibility baseline

WCAG AA: keyboard navigation · visible focus states · semantic HTML · screen-reader compatibility ·
accessible contrast · reduced-motion support.
