# Deployment Runbook

_Focused operational view of Master PRD Chapter 8. The full chapter narrative is in
[08_Deployment.md](./08_Deployment.md); configuration is in
[ENVIRONMENT_VARIABLES.md](./ENVIRONMENT_VARIABLES.md)._

## Principles

Every deployment must be **Safe · Reproducible · Reversible · Observable**. **No manual production
changes.** Infrastructure is version-controlled wherever practical. Deploy via a **single automated
pipeline**.

## Environments

| Env | Purpose | Notes |
|---|---|---|
| **Development** | Local engineering | Hot reload, debug logging, mock/test payment keys, dev database |
| **Staging** | QA / UAT / client review / deploy verification | Mirror production closely; avoid fake infra |
| **Production** | Live traffic | Optimized for security/stability/performance; debug tools disabled/not exposed |

**Never use production data in development without anonymization.**

## Hosting stack

Next.js (frontend) → NestJS (backend) → PostgreSQL → Redis → Cloudinary → Razorpay → Resend.
Platform: **Coolify** (preferred self-hosting) or comparable production-ready platform.

## Containerization

Every service runs in **Docker** with a **Dockerfile**, a **healthcheck**, and environment
configuration. The whole app starts with a **single orchestration command** (e.g. `docker compose up`).

## CI/CD pipeline (on commit to `main`)

1. Install dependencies
2. Lint
3. Type-check
4. Unit tests
5. Integration tests
6. Build frontend
7. Build backend
8. Validate database migrations
9. Deploy to staging
10. Require approval before production (if desired)

**No production deploy if any required step fails.**

## Database migrations

Version-controlled. Reversible where feasible · reviewed before deployment · no destructive operations
without explicit approval.

## Pre-deploy checklist (production)

- [ ] Tests pass
- [ ] Linting passes
- [ ] Type checking passes
- [ ] Database migrations reviewed
- [ ] Environment variables verified
- [ ] Build succeeds
- [ ] Security review completed
- [ ] Smoke tests completed
- [ ] Rollback plan available

## Rollback strategy

On critical issues: revert the app to the previous stable release · restore the database only if
necessary and after careful assessment · document the incident and root cause. Support rapid recovery
with minimal downtime.

## Health endpoints

Expose checks for: API · database connectivity · Redis connectivity · queue processing. Suitable for
uptime monitoring and orchestration.

## Observability

- **Logs:** structured, including timestamp, level, request ID, user ID (if authed), restaurant ID (if
  applicable), module, action — ready for centralized aggregation.
- **Monitor:** API latency · error rates · DB/Redis/queue health · payment failures · email delivery
  failures · order creation success rate.
- **Alert on:** DB unavailable · payment verification failures · excessive error rates · repeated failed
  logins · queue failures · backup failures · critical services down (see
  [07_Security.md](./07_Security.md#alerting)).

## Static assets & caching

Serve optimized assets via Cloudinary + CDN; do not store uploads on the app server. Use Redis to cache
menu, categories, restaurant settings, rate limiting, and temporary session data; invalidate immediately
after updates.

## Performance targets

Homepage < 2 s · Menu < 2 s · Dashboard < 2 s · API typically < 300 ms · payment verification as fast as
safely possible.

## Backups & disaster recovery

Daily encrypted backups (daily/weekly/monthly retention), tested via restore drills. Recovery: restore
latest verified backup → rebuild infra from version-controlled config → validate integrity → resume with
minimal downtime → document. See [07_Security.md](./07_Security.md#backup-strategy).

## Operational documentation to maintain

Local setup · environment variables · deployment process · database migrations · common operational
tasks · incident recovery. Keep current alongside code changes.
