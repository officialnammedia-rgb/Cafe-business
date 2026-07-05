# Test Plan

_Working test matrices from Master PRD Chapter 9. The full chapter narrative is in
[09_Testing.md](./09_Testing.md); pass/fail gates are in
[ACCEPTANCE_CRITERIA.md](./ACCEPTANCE_CRITERIA.md)._

## Test layers

| Layer | Purpose | Examples |
|---|---|---|
| **Unit** | Individual business logic | Price calc · tax calc · coupon validation · order total · delivery radius · product availability |
| **Integration** | Modules work together | `Customer → Cart → Checkout → Payment → Order`; all APIs behave correctly |
| **End-to-end** | Real customer simulation | `Homepage → Menu → Add Coffee → Checkout → Pay → Confirmation → View Order` |

Critical flows requiring automated tests: Authentication · Checkout · Payment verification · Order
creation · Order status updates.

## Manual QA checklist (every release)

### Website
- [ ] Homepage loads correctly
- [ ] Navigation works
- [ ] Mobile responsiveness verified
- [ ] Images optimized
- [ ] SEO metadata present
- [ ] Footer links work

### Menu
- [ ] Categories display correctly
- [ ] Search works
- [ ] Filters work
- [ ] Product images load
- [ ] Prices display correctly
- [ ] Product variants calculate correctly

### Cart
- [ ] Add item
- [ ] Remove item
- [ ] Increase quantity
- [ ] Decrease quantity
- [ ] Totals update correctly
- [ ] Taxes calculate correctly
- [ ] Discounts apply correctly

### Checkout
- [ ] Address selection works
- [ ] Payment succeeds
- [ ] Payment failure handled gracefully
- [ ] Order confirmation displays correctly

### Dashboard
- [ ] Login works
- [ ] Orders update in real time
- [ ] Product editing works
- [ ] Availability toggles work
- [ ] Coupons create successfully
- [ ] Analytics load without errors

## Browser support matrix

| Platform | Browsers |
|---|---|
| Desktop | Chrome · Safari · Edge · Firefox |
| Mobile | Chrome (Android) · Safari (iPhone) |

Support the latest stable versions.

## Responsive testing matrix

Test at widths: **320 · 375 · 390 · 414 · 768 · 1024 · 1280 · 1440 · 1920 px**. No layout breakage.

## Accessibility testing

Keyboard navigation · focus visibility · screen-reader labels · color contrast · semantic HTML · form
labels · error announcements. Aim for **WCAG AA**.

## Performance testing

Measure: homepage load · menu load · dashboard load · API latency · image optimization · bundle size.
**Target Lighthouse 95+** (Performance, Accessibility, Best Practices, SEO) where practical. Also:
homepage/menu/dashboard < 2 s; API typically < 300 ms.

## Security testing

Authentication · authorization · rate limiting · file upload restrictions · input validation · payment
verification · session handling · protected routes. **No sensitive data exposed.** See
[07_Security.md](./07_Security.md#security-review-checklist).

## Error-handling tests

Internet disconnected · payment declined · product unavailable · restaurant closed · invalid coupon ·
expired session · server error. App must **fail gracefully with helpful messages**.

## Regression testing

On any feature change, verify: existing functionality still works · APIs remain compatible · dashboard
unaffected · customer flow unaffected.
