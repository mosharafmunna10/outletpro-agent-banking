# OutletPro Online — multi-outlet starter

This is a deployment-oriented **starter/MVP**, not a certified banking system. It includes outlet-scoped users, PostgreSQL schema, server-side sessions, basic dashboard, cash entries, expenses, targets API and audit logging. It does not connect to BRAC Bank systems, CBS, payment rails or real banking accounts.

## Outlets preloaded
1. Janata Agent Banking Outlet
2. Chowara Agent Banking Outlet
3. Paduar Agent Banking Outlet
4. Ashrafpur Agent Banking Outlet
5. Chaygram Agent Banking Outlet
6. Songkuchail Agent Banking Outlet
7. Lakshmipur Chowmuhoni Agent Banking Outlet
8. Bharasar Agent Banking Outlet

## Local run with Docker
Requirements: Docker Engine + Docker Compose.

1. In this folder create `.env` with these values (generate strong random secrets; do not share them):

```dotenv
POSTGRES_PASSWORD=replace-with-a-long-random-database-password
SESSION_SECRET=replace-with-at-least-32-random-bytes
```

2. Start: `docker compose up --build -d`
3. Create the central administrator: `docker compose exec app node scripts/create-user.js`
4. Create eight `outlet_user` accounts, one per outlet, using the same command. Choose each outlet from the numbered list. Use unique usernames and strong temporary passwords.
5. Open `http://localhost:3000` for local testing.

The database schema and outlet seed data are initialized when the app container starts. Keep `.env` private.

## Online deployment checklist
1. Provision a reputable host with HTTPS and a managed PostgreSQL instance in a private network. Keep the database inaccessible from the public internet.
2. Set `NODE_ENV=production`, `DATABASE_URL`, a random `SESSION_SECRET` (32+ characters), `PORT`, and configure the trusted proxy only according to the host's documented HTTPS proxy setup.
3. Configure TLS/HTTPS, custom domain, database backups with tested restoration, monitoring and security alerts.
4. Create one admin account and one separate account per outlet. Never use shared accounts. Deliver initial passwords through a separate secure channel.
5. Test access boundaries using two outlet accounts: an account for one outlet must never be able to read or write another outlet's data, even if a request is manually altered.
6. Use synthetic test data first. Complete a security review, dependency audit, restore drill and user acceptance test before any real operational use.

## Security included
- Password hashes using bcrypt (12 rounds); no plain-text password storage.
- Server-side sessions stored in PostgreSQL, HttpOnly/SameSite cookies, Secure cookie in production.
- Login rate limiting and temporary lock after repeated failed attempts.
- Helmet security headers, JSON request size limit, same-origin check for writes.
- Outlet scope derived from authenticated server session for outlet users; client outlet ID is not trusted for those users.
- Audit trail for login/logout and key write operations.

## Important gaps before production use
- This starter has not been independently penetration-tested or compliance-certified.
- Add a controlled first-login password reset flow, MFA for administrators, formal password reset process, user administration UI, account deactivation workflow, audit log retention/export, backup automation, alerts, and stronger schema validation.
- Review privacy/data retention rules and obtain authorization before storing customer data. Do not store PINs, OTPs, card details, account passwords, NID scans, or unnecessary customer-identifying information.
- Cash entries are internal tracking records only; they are not reconciled to bank core systems and must not be represented as official bank balances.
- HTTPS must be provided by the production host/reverse proxy. Do not expose the app over plain HTTP on the public internet.
- The app currently includes a basic web UI for login, summary, cash entries and expenses. Targets and audit log are API-backed; a full reporting and user administration UI is still future work. The `remittance` target metric is a count only: target and achievement must be whole numbers, never currency amounts.

## Main API
- `POST /api/login`, `POST /api/logout`, `GET /api/me`
- `GET /api/outlets`, `GET /api/summary`
- `GET/POST /api/cash-entries`
- `GET/PUT /api/targets`
- `GET/POST /api/expenses`
- `GET /api/audit-logs` (admin only)
- `GET /api/health`
