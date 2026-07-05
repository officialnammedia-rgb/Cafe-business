# 08 · Deployment, Infrastructure & DevOps

_Source: Master PRD Chapter 8. A focused operational runbook is in [DEPLOYMENT.md](./DEPLOYMENT.md);
configuration is in [ENVIRONMENT_VARIABLES.md](./ENVIRONMENT_VARIABLES.md)._

## Objective

The application should be deployable to production with a **single automated pipeline**. Every
deployment should be **Safe · Reproducible · Reversible · Observable**. **No manual production changes
should ever be required.** Infrastructure must be version-controlled wherever practical.

## Environments

Maintain three environments:

### Development
For local engineering work: hot reload enabled · debug logging enabled · mock/test payment keys ·
development database.

### Staging
Mirror production as closely as possible. Purpose: QA testing · UAT · client review · deployment
verification. Never use fake infrastructure here unless absolutely necessary.

### Production
Optimized for security · stability · performance. Debugging features disabled. Development tools not
exposed.

## Hosting

Preferred stack: **Next.js** (frontend) → **NestJS** (backend) → **PostgreSQL** → **Redis** →
**Cloudinary** → **Razorpay** → **Resend**. Deployment platform: **Coolify preferred for self-hosting**,
or a comparable production-ready platform (see
[DECISIONS.md](./DECISIONS.md#open-questions-for-the-product-owner)).

## Containerization

Every service must run inside **Docker**. Each service has: a **Dockerfile** · a **healthcheck** ·
**environment configuration**. The application should start using a **single orchestration command**.

## Environment variables

Store all configuration **outside source code**. Examples: `DATABASE_URL` · `REDIS_URL` · `JWT_SECRET` ·
`RAZORPAY_KEY` · `RAZORPAY_SECRET` · `GOOGLE_MAPS_KEY` · `CLOUDINARY_KEY` · `RESEND_KEY`. **Never commit
secrets to Git.** Provide a `.env.example` file with placeholder values. Full reference in
[ENVIRONMENT_VARIABLES.md](./ENVIRONMENT_VARIABLES.md).

## CI/CD pipeline

Every commit to `main` should trigger:

1. Install dependencies.
2. Run linting.
3. Run type checking.
4. Execute unit tests.
5. Execute integration tests.
6. Build frontend.
7. Build backend.
8. Validate database migrations.
9. Deploy to staging.
10. Require approval before production deployment (if desired).

**Production deployments should not occur if any required step fails.**

## Database migrations

All schema changes are managed through **version-controlled migrations**. Migrations should be:
reversible where feasible · reviewed before deployment · avoid destructive operations without explicit
approval.

## Static assets

Serve optimized assets through Cloudinary and a CDN where applicable. **Avoid storing uploaded assets on
the application server.**

## Caching

Use Redis for: frequently accessed menu data · categories · restaurant settings · rate limiting ·
temporary session data (if applicable). **Invalidate caches immediately after updates.**

## Logging

Application logs should include: timestamp · log level · request ID · user ID (if authenticated) ·
restaurant ID (if applicable) · module · action. Structure logs to support centralized log aggregation
in the future.

## Monitoring

Monitor: API latency · error rates · database health · Redis health · queue health · payment failures ·
email delivery failures · order creation success rate.

## Health endpoints

Expose health checks for: API · database connectivity · Redis connectivity · queue processing. These
should be suitable for uptime monitoring and orchestration tools.

## Deployment checklist (before every production deploy)

- Tests pass.
- Linting passes.
- Type checking passes.
- Database migrations reviewed.
- Environment variables verified.
- Build succeeds.
- Security review completed.
- Smoke tests completed.
- Rollback plan available.

## Rollback strategy

If a deployment introduces critical issues: revert the application to the previous stable release ·
restore the database only if necessary and after careful assessment · document the incident and root
cause. The system should support rapid recovery with minimal downtime.

## Performance targets

- Homepage load: **under 2 seconds**.
- Menu page: **under 2 seconds**.
- Dashboard: **under 2 seconds**.
- API: typical responses **under 300 ms**.
- Payment verification: as fast as reasonably achievable without compromising security.

## Documentation

Maintain documentation for: local setup · environment variables · deployment process · database
migrations · common operational tasks · incident recovery. **Documentation is kept current alongside
code changes.**
