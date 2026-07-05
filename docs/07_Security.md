# 07 · Security, DevSecOps & Production Readiness

_Source: Master PRD Chapter 7 (with role permissions consolidated from Chapters 1, 3, and 4).
Configuration & secrets reference is in [ENVIRONMENT_VARIABLES.md](./ENVIRONMENT_VARIABLES.md)._

## Philosophy

**Security is not a feature — it is part of every feature.** Every API, page, upload, database query,
and deployment. Every engineer working on BrewOS should assume:

> **Everything coming from the client can be malicious until proven otherwise.**

## Security goals

The platform should serve production traffic without exposing customer information. Goals: zero known
critical vulnerabilities · zero sensitive information leaked · secure authentication · secure payments ·
secure deployments · complete auditability.

## Security principles

Least Privilege · Secure by Default · Defense in Depth · Zero Trust · Fail Securely · Explicit
Validation · Never Trust User Input. Everything should be validated, authenticated, and authorized.

## Authentication

**V1:** Email + Password. **Future:** Google Login · Apple Login · OTP Login · Magic Link.

**Passwords:** never store plaintext; always hash with modern secure algorithms; never log; never
return.

**Sessions:** use secure HTTP-only cookies where appropriate. Cookies must be **Secure · SameSite ·
HttpOnly**. Rotate session identifiers after login where appropriate. Support **logout from all
devices**.

> Preferred implementation is Better Auth (or equivalent). The primary mechanism (Better Auth vs.
> custom JWT) is an open decision — see
> [DECISIONS.md](./DECISIONS.md#i-003--authentication-better-auth-vs-jwt-env-vars).

## Authorization

Every API request must verify, in order:

```
Authentication → Role → Restaurant Ownership → Permission → Resource Ownership
```

**Never trust frontend permissions.**

### Role permissions

| Role | Can | Cannot |
|---|---|---|
| **Customer** | Access own profile, orders, addresses, favorites | Access anyone else's data |
| **Staff** | Accept orders, update status, view products | Delete restaurant, access billing, modify platform settings |
| **Manager** | Edit products, create coupons, manage orders, manage staff | Access platform admin, delete restaurant |
| **Platform Admin** | Manage restaurants, view platform analytics, manage subscriptions, view logs | — |

(Staff sub-roles Owner/Manager/Kitchen/Cashier/Support have configurable permissions — see
[03_Admin_Dashboard.md](./03_Admin_Dashboard.md#staff-management).)

## Input validation

Every request must be validated. **Reject:** unexpected fields · malformed JSON · oversized payloads ·
invalid IDs · negative quantities · invalid prices · invalid coupon codes.

## SQL injection

Prevent via **parameterized queries**, ORM protections, and input validation. **Never build raw SQL
with string concatenation.**

## Cross-site scripting (XSS)

Escape user-generated content · sanitize HTML where applicable · use a **Content Security Policy
(CSP)** · never trust user input displayed in the UI.

## CSRF

Protect state-changing requests · use CSRF protection where applicable · verify request origins.

## Rate limiting

Apply to: Login · Registration · Checkout · Password Reset · Admin APIs · Search endpoints (to prevent
abuse).

## Brute-force protection

After repeated failed logins: temporary rate limiting · optional CAPTCHA after a threshold · log
suspicious activity.

## CORS

**Whitelist allowed origins.** Never use `Access-Control-Allow-Origin: *` for authenticated endpoints.

## Secure headers

Use: Content Security Policy (CSP) · Strict-Transport-Security (HSTS) · X-Frame-Options ·
X-Content-Type-Options · Referrer-Policy · Permissions-Policy. Review and tailor to application needs.

## File upload security

**Allowed: images only.** Validate MIME type · extension · size · dimensions (if needed). Reject
executable files and unexpected formats. Store uploads in **Cloudinary**. **Never execute uploaded
files.**

## Secrets management

**Never commit secrets to Git.** Store via environment variables or a secure secrets manager. Examples:
Database URL · JWT secret · Razorpay credentials · Cloudinary keys · Email provider keys · Maps API
keys. Full list in [ENVIRONMENT_VARIABLES.md](./ENVIRONMENT_VARIABLES.md).

## Logging

**Never log:** passwords · card information · authentication secrets · session tokens · private keys ·
sensitive personal data beyond what is necessary for operations and debugging.

## Payment security

All payment verification happens on the **backend**. The frontend **never** decides whether a payment
succeeded. Verify signatures before confirming prepaid orders. Store payment references securely. See
[04_Backend.md](./04_Backend.md#payments).

## HTTPS

Production must use HTTPS. Redirect HTTP → HTTPS. Reject insecure production traffic where appropriate.

## API security

Validate every request · authorize every request · log important requests · return **generic errors**
for unauthorized access · never leak stack traces.

## Audit logging

Record: who performed the action · what action occurred · which resource changed · timestamp · relevant
metadata. Critical actions include: price changes · menu edits · order cancellations · coupon creation
· staff management · permission changes.

## Backup strategy

Daily automated database backups. Retention: daily (short-term) · weekly (medium-term) · monthly
(long-term). Backups should be **encrypted and tested periodically through restore drills**.

## Disaster recovery

If production fails: restore the latest verified backup · rebuild infrastructure using version-
controlled configuration · validate application integrity · resume service with minimal downtime ·
document the recovery process.

## Monitoring

Track: API latency · CPU usage · memory usage · database health · Redis health · queue health · email
failures · payment failures · order failures · authentication failures.

## Alerting

Alert on: database unavailable · payment verification failures · excessive error rates · repeated
failed logins · queue failures · backup failures · critical services down.

## Dependency management

Keep dependencies updated · monitor for known vulnerabilities · remove unused packages · review
third-party libraries before adoption.

## Release process

Every production release includes: automated tests passing · manual smoke testing · security review ·
database migration validation · rollback plan.

## Environment separation

Maintain separate **Development · Staging · Production** environments. **Never use production data in
development without proper anonymization.** See [08_Deployment.md](./08_Deployment.md#environments).

## Security review checklist (before every release)

- Authentication works correctly.
- Authorization is enforced.
- Input validation passes.
- File uploads are restricted.
- Payments are verified server-side.
- Secrets are not exposed.
- Logs do not contain sensitive information.
- Rate limits are functioning.
- Backups completed successfully.
- Monitoring is healthy.

## Secure coding rules for Claude

Prefer safe language/framework defaults · avoid hardcoded credentials · keep dependencies current ·
validate all external input · avoid unnecessary privileges · write readable, maintainable code ·
document security-sensitive logic · flag assumptions that could impact security. **If a requested
implementation would reduce security, recommend a safer alternative and explain the trade-offs.**

## Definition of production ready

Production ready only if: critical security checks are in place · core functionality is fully tested ·
logging and monitoring are operational · backups are configured and verified · deployment is
reproducible · performance targets are met · documentation is up to date.

## Final product direction

BrewOS should be engineered assuming it will eventually support many cafés, multiple developers, and
continuous feature development. **Every security decision should favor long-term reliability and
maintainability over short-term convenience.**
