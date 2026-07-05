# Folder Structure

_Source: Master PRD Chapter 10 (folder structure) and Chapter 4 (module encapsulation). **Organize by
feature rather than by file type wherever possible.**_

> This describes the **target** structure for the prescribed Next.js + NestJS build. The repository
> currently contains a static site (`index.html`, `styles.css`, `script.js`) — the migration approach
> is an open decision (see
> [DECISIONS.md](./DECISIONS.md#i-002--greenfield-rebuild-vs-evolving-the-existing-static-site)).

## Frontend (Next.js — App Router)

```text
app/          # routes (App Router)
components/    # shared/presentational UI components
features/      # feature modules (menu, cart, checkout, orders, dashboard, ...)
hooks/         # reusable React hooks
lib/           # framework/client setup, API client, auth helpers
services/      # data-fetching / API service layer
styles/        # global styles, Tailwind config, design tokens
types/         # shared TypeScript types
utils/         # pure utility functions
public/        # static assets
```

Design-system components (Button, Input, Card, …) live under `components/`; feature-specific composition
lives under `features/<feature>/`. See [COMPONENT_LIBRARY.md](./COMPONENT_LIBRARY.md).

## Backend (NestJS)

```text
src/
  modules/       # feature modules (auth, users, restaurants, menu, categories,
                 #   products, orders, customers, coupons, payments, analytics,
                 #   notifications, uploads, settings, staff, audit-logs)
  common/        # shared providers, decorators, constants
  config/        # configuration loading & validation
  database/      # Prisma client, migrations, seed
  middleware/    # request middleware
  guards/        # auth/role/tenant guards
  interceptors/  # response envelope, logging, transforms
  filters/       # exception filters (structured error envelopes)
  utils/         # pure utilities
```

### Module encapsulation (from Chapter 4)

Each feature module encapsulates: **Controller · Service · Repository · DTOs · Validation · Tests ·
Documentation.** Business logic lives in **services, not controllers**.

Suggested per-module layout:

```text
modules/<feature>/
  <feature>.controller.ts
  <feature>.service.ts
  <feature>.repository.ts
  dto/
  entities/            # or Prisma-derived types
  <feature>.module.ts
  <feature>.spec.ts     # unit tests
  README.md             # module documentation
```

## Cross-cutting placement

- **Guards** enforce Auth → Role → Restaurant Ownership → Permission → Resource Ownership
  ([07_Security.md](./07_Security.md#authorization)).
- **Interceptors** apply the success envelope and structured logging with correlation IDs.
- **Filters** convert errors to the structured error envelope ([API.md](./API.md#error-envelope)).
- **config/** loads secrets from environment variables only
  ([ENVIRONMENT_VARIABLES.md](./ENVIRONMENT_VARIABLES.md)).

## Repository root (target)

```text
/                 # monorepo or app root
  apps/ or web/ + api/   # frontend and backend (structure TBD at scaffolding time)
  docs/            # this documentation system
  README.md
  CLAUDE.md
  .env.example     # placeholder secrets (never commit real secrets)
  docker-compose / Dockerfiles per service
```

Final monorepo vs. split-repo layout is decided at scaffolding time; either way, keep the feature-based
organization above.
