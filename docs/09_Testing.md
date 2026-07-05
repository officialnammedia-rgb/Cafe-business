# 09 · Quality Assurance, Testing & Acceptance Criteria

_Source: Master PRD Chapter 9. Working test matrices are in [TEST_PLAN.md](./TEST_PLAN.md); the
acceptance/Done definitions are in [ACCEPTANCE_CRITERIA.md](./ACCEPTANCE_CRITERIA.md)._

## Objective

Every feature must satisfy the same quality standards before being considered complete.

- A feature is **not complete** when it compiles.
- A feature is **not complete** when it "works on my machine."
- A feature is complete only when it has been: **Designed · Developed · Reviewed · Tested · Optimized ·
  Documented.**

## Development lifecycle

```text
Requirement → UX Review → Implementation → Code Review → Unit Tests →
Integration Tests → Manual Testing → Performance Check → Accessibility Check →
Security Review → Production Ready
```

Claude should follow this process for every major feature.

## Testing strategy

### Unit tests
Validate individual business logic. Examples: price calculation · tax calculation · coupon validation ·
order total calculation · delivery radius calculation · product availability.

### Integration tests
Ensure modules work together. Example chain: `Customer → Cart → Checkout → Payment → Order`. All APIs
should behave correctly.

### End-to-end tests
Simulate a real customer:

```text
Visit Homepage → Browse Menu → Add Coffee → Checkout → Pay →
Receive Confirmation → View Order
```

The entire flow should succeed.

## Manual QA checklist (every release)

### Website
Homepage loads correctly · navigation works · mobile responsiveness verified · images optimized · SEO
metadata present · footer links work.

### Menu
Categories display correctly · search works · filters work · product images load · prices display
correctly · product variants calculate correctly.

### Cart
Add item · remove item · increase quantity · decrease quantity · totals update correctly · taxes
calculate correctly · discounts apply correctly.

### Checkout
Address selection works · payment succeeds · payment failure handled gracefully · order confirmation
displays correctly.

### Dashboard
Login works · orders update in real time · product editing works · availability toggles work · coupons
create successfully · analytics load without errors.

## Browser support

**Desktop:** Chrome · Safari · Edge · Firefox. **Mobile:** Chrome (Android) · Safari (iPhone). Support
the latest stable versions.

## Responsive testing

Test at: 320 · 375 · 390 · 414 · 768 · 1024 · 1280 · 1440 · 1920 px. **No layout breakage should
occur.**

## Accessibility testing

Verify: keyboard navigation · focus visibility · screen reader labels · color contrast · semantic HTML ·
form labels · error announcements. Aim to meet **WCAG AA**.

## Performance testing

Measure: homepage load time · menu load time · dashboard load time · API latency · image optimization ·
bundle size. **Target: Lighthouse score of 95+ for Performance, Accessibility, Best Practices, and SEO
where practical.**

## Security testing

Verify: authentication · authorization · rate limiting · file upload restrictions · input validation ·
payment verification · session handling · protected routes. **No sensitive data should be exposed.**

## Error handling tests

Test: internet disconnected · payment declined · product unavailable · restaurant closed · invalid
coupon · expired session · server error. The application should **fail gracefully with helpful
messages**.

## Acceptance criteria

A feature is accepted only if: requirements are met · UI matches the design system · mobile experience
is complete · tests pass · accessibility requirements are met · no critical console errors · no
TypeScript errors · no build warnings requiring immediate attention · documentation updated if
necessary. See [ACCEPTANCE_CRITERIA.md](./ACCEPTANCE_CRITERIA.md).

## Bug severity levels

| Level | Examples | Action |
|---|---|---|
| **Critical** | Payment failure causing incorrect orders · authentication bypass · data leak · server crash | Must be fixed before release |
| **High** | Checkout broken · orders not received · dashboard unusable | Fix before production |
| **Medium** | UI misalignment · slow-loading section · minor workflow issue | Fix before next stable release, if not before launch |
| **Low** | Minor animation issue · copy typo · spacing inconsistency | Schedule appropriately |

## Regression testing

Whenever a feature changes, verify: existing functionality still works · APIs remain compatible ·
dashboard unaffected · customer flow unaffected.

## Release criteria

Version 1 can launch only if: ordering works end-to-end · payments verified successfully · dashboard
fully operational · customer accounts functioning · menu management complete · security review completed
· performance targets met · core documentation complete.

## Product quality philosophy

Do not ship quickly — **ship confidently**. A smaller, polished product is preferable to a larger,
unreliable one. Every feature should feel intentional.

## Claude quality rules

Refactor when code becomes difficult to understand · prefer readability over clever implementations ·
remove dead code · remove duplicate code · avoid unnecessary dependencies · write self-documenting code
where possible · keep functions focused on a single responsibility · **leave the codebase cleaner after
each implementation.**

## Definition of Done

A feature is **Done** only when it is: ✅ Functionally complete · ✅ Visually polished · ✅ Responsive ·
✅ Accessible · ✅ Secure · ✅ Tested · ✅ Documented · ✅ Optimized · ✅ Reviewed against this PRD.

## Final instruction

Treat **quality as a product feature**. If an implementation technically works but creates a confusing
experience, inconsistent interface, or difficult-to-maintain code, improve it before considering the
feature complete.
