# JKADB Implementation Update

This source tree has been upgraded from the supplied archive while preserving the existing Next.js + PostgreSQL + Drizzle architecture.

## Included

- Green/white public-service visual system and open-hand startup identity.
- Settings page with Admin Login moved out of the citizen homepage.
- Real Grok/xAI backend assistant at `/api/ai/chat`; the API key is server-side only.
- Temporary AI conversation state in the browser; only operational AI metrics are stored.
- PostgreSQL/Drizzle schema additions for RBAC permissions, admin sessions, post offices, public contacts, quick alerts, response templates, AI metrics, notification delivery and system jobs.
- Username + email administrator login.
- Server-side permission helpers and audit helper.
- Secure complaint tracking verification.
- Required citizen phone number.
- Post Office location support.
- Idempotency-key support for complaint submissions.
- Private evidence storage with JPG/JPEG/PNG/PDF validation and protected download.
- Admin pages/APIs for Public Contact, Quick Alerts, Response Templates, FAQ, Roles & Permissions, AI Monitoring, System Health, Reports and Assignments.
- Announcement popup field and update/archive endpoints.
- Initial Drizzle migration and schema snapshot under `drizzle/`.
- Environment template without secrets.

## Required local setup

1. Copy `.env.example` to `.env.local`.
2. Set a real PostgreSQL `DATABASE_URL`.
3. Set a long random `JWT_SECRET`.
4. Set `ADMIN_INITIAL_EMAIL`, `ADMIN_INITIAL_USERNAME`, and `ADMIN_INITIAL_PASSWORD`.
5. Set `GROK_API_KEY` and, if desired, `GROK_MODEL`.
6. Install dependencies with `npm install`.
7. Apply migrations with `npm run db:migrate`.
8. Seed with `npm run db:seed`.
9. Run `npm run typecheck`.
10. Run `npm run dev`.

Never commit `.env.local`, Grok keys, JWT secrets or real administrator passwords.

## Important

The supplied archive did not contain a production database, external SMS/email credentials, malware scanner, or backup infrastructure. Those integrations are represented through real server-side boundaries/configuration rather than fake success responses. External delivery should be configured before production use.
