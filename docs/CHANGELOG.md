# Changelog

All notable changes to BrewOS (product and documentation) are recorded here. Format follows
[Keep a Changelog](https://keepachangelog.com/); the project uses semantic-ish versioning
(`MAJOR.MINOR.PATCH`).

## [Unreleased]

### Added
- **Documentation system** generated from the Master PRD v1.0 (`masterprd.pdf`). Created the `docs/`
  library (vision, website, dashboard, backend, UI/UX, database, security, deployment, testing,
  API, architecture, design system, roadmap, user flows, component library, coding standards, folder
  structure, environment variables, test plan, acceptance criteria, decisions) plus root
  `README.md` and `CLAUDE.md`. See [01_PRD.md](./01_PRD.md) for the document map.
- **DECISIONS.md** capturing structural choices and 10 flagged PRD inconsistencies/ambiguities
  (brand-name mismatch, greenfield vs. migration, auth mechanism, delivery model, and more).

### Notes
- No application code has been written yet. Awaiting product-owner approval of the documentation and
  resolution of the open questions in [DECISIONS.md](./DECISIONS.md) before implementation begins.

---

## Product release history (planned)

### [1.0.0] — Not yet released
Target scope: marketing website, customer ordering, authentication, restaurant dashboard, Razorpay
payments, email notifications, basic analytics, menu & order management, responsive UI, SEO.
Release gate: see [ACCEPTANCE_CRITERIA.md](./ACCEPTANCE_CRITERIA.md#release-criteria) and
[09_Testing.md](./09_Testing.md#release-criteria).
