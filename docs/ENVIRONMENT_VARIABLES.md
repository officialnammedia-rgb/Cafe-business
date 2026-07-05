# Environment Variables & Configuration

_Source: Master PRD Chapter 8 (Environment Variables) and Chapter 7 (Secrets Management). **Store all
configuration outside source code. Never commit secrets to Git.** Provide a `.env.example` with
placeholder values._

## Rules

- All configuration lives in **environment variables** (or a secure secrets manager) — never in source.
- **Never commit secrets to Git.** Keep a committed `.env.example` with placeholders only.
- Secrets are validated at startup (fail fast if missing) via `config/`.
- Different values per environment (Development / Staging / Production).
- **Never log** secrets (see [07_Security.md](./07_Security.md#logging)).

## Variables named in the PRD

| Variable | Purpose |
|---|---|
| `DATABASE_URL` | PostgreSQL connection string |
| `REDIS_URL` | Redis connection string |
| `JWT_SECRET` | Session/JWT signing secret |
| `RAZORPAY_KEY` | Razorpay public/key id |
| `RAZORPAY_SECRET` | Razorpay secret (server-only) |
| `GOOGLE_MAPS_KEY` | Google Maps Platform key |
| `CLOUDINARY_KEY` | Cloudinary API key |
| `RESEND_KEY` | Resend (email) API key |

## Additional variables (implied by the stack — confirm at scaffolding)

> These are commonly required by the prescribed services; add only what is actually used, and document
> each here as it is introduced.

| Variable | Purpose |
|---|---|
| `NODE_ENV` | `development` / `staging` / `production` |
| `APP_URL` / `NEXT_PUBLIC_APP_URL` | Public base URL (for links, OG, CORS) |
| `API_URL` / `NEXT_PUBLIC_API_URL` | Frontend → API base URL |
| `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_SECRET` | Cloudinary account + secret |
| `RESEND_FROM_EMAIL` | Verified sender address |
| `CORS_ALLOWED_ORIGINS` | Whitelisted origins (never `*` for authed endpoints) |
| Better Auth vars (if adopted) | Provider secrets/URLs — see [DECISIONS.md](./DECISIONS.md#i-003--authentication-better-auth-vs-jwt-env-vars) |

## Secrets inventory (keep outside the repo)

Database URL · JWT secret · Razorpay credentials · Cloudinary keys · Email provider (Resend) keys ·
Google Maps API key · any future integration credentials (delivery, ONDC, WhatsApp).

## `.env.example` template (starter)

```dotenv
# Core
NODE_ENV=development
APP_URL=http://localhost:3000
API_URL=http://localhost:4000

# Database & cache
DATABASE_URL=postgresql://user:password@localhost:5432/brewos
REDIS_URL=redis://localhost:6379

# Auth
JWT_SECRET=replace-with-strong-random-secret

# Payments (Razorpay)
RAZORPAY_KEY=rzp_test_xxxxxxxx
RAZORPAY_SECRET=replace-me

# Maps
GOOGLE_MAPS_KEY=replace-me

# Storage (Cloudinary)
CLOUDINARY_CLOUD_NAME=replace-me
CLOUDINARY_KEY=replace-me
CLOUDINARY_SECRET=replace-me

# Email (Resend)
RESEND_KEY=replace-me
RESEND_FROM_EMAIL=orders@example.com

# Security
CORS_ALLOWED_ORIGINS=http://localhost:3000
```

Update this template as variables are added; keep it in sync with the code that reads configuration.
