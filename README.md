# Gym Backend — Postman

Standalone Postman collection for the [gym-backend](https://github.com/abdulhasibn/gym-backend) Express API.
Share this repo with the team — no paid Postman plan required.

## Files

| File | Purpose |
| ---- | ------- |
| `Gym-Backend-API.postman_collection.json` | Auth + Gym Orgs + Staff Invites + Leads + Plans + Membership Invites, AI-oriented docs, saved Examples |
| `Gym-Backend-Local.postman_environment.json` | Local `baseUrl` (`http://localhost:3000`) |
| `Gym-Backend-Dev.postman_environment.json` | Dev environment variables (same keys as Local) |

## Collection layout

Every feature ships as its own **top-level folder** (same pattern as Gym Orgs, Staff
Invites, Leads, Plans, Membership Invites). Do not leave new feature requests at collection root.

When syncing to the cloud collection via MCP, prefer `putCollection` (async) with
the git export so folders stay intact — `createCollectionRequest` alone lands
items at root.

## Import

1. Clone this repo.
2. Open Postman → **Import** → select the collection and the environment(s) you need.
3. Select **Gym Backend — Local** or **Gym Backend — dev**.

## AI / mobile / web integration

Each request documents:

- **Every property** — request body, query params, and response fields
- **Enums** — full allowed value lists (e.g. `paid` \| `unpaid` \| `partial`)
- **String examples** — realistic sample values
- **Success** status + body shape
- **Errors** with stable `error.code` values

Open a request’s **Docs** panel for the property tables; open **Examples** for
concrete success/error JSON. Query params also carry per-param descriptions.

Written guides in gym-backend (same content, markdown form):

- [`docs/api.md`](https://github.com/abdulhasibn/gym-backend/blob/main/docs/api.md) — index
- [`docs/client-auth.md`](https://github.com/abdulhasibn/gym-backend/blob/main/docs/client-auth.md)
- [`docs/plans.md`](https://github.com/abdulhasibn/gym-backend/blob/main/docs/plans.md)
- [`docs/membership-invites.md`](https://github.com/abdulhasibn/gym-backend/blob/main/docs/membership-invites.md)
- [`docs/leads.md`](https://github.com/abdulhasibn/gym-backend/blob/main/docs/leads.md)

Shared error envelope:

```json
{ "error": { "code": "STRING_CODE", "message": "Human-readable message" } }
```

## Smoke flow

1. Set `email` in the environment (and `lane` / `name` for first-time users).
2. Run **Auth → Email OTP → Request OTP** — response includes `isNewUser`.
3. If `isNewUser` is true, set `lane` (`CLIENT` or `STAFF`) and optional `name`.
4. Paste the email code into `otpToken`.
5. Run **Verify OTP** (include `lane` only when `isNewUser` was true; stores tokens / `userId`).
6. Run **Auth → Session → Get Current User**.
7. Gym onboarding (STAFF lane):
   1. **Gym Orgs → Create Gym Org** (stores `gymOrgId`)
   2. **List My Gym Orgs**
   3. **Get Gym Org** / **Update Gym Org**
8. Staff invites (needs a second STAFF account’s `staff_code` in `inviteeStaffCode`):
   1. Admin token: **Create Staff Invite** (stores `staffInviteId`) → **List Gym Staff Invites**
   2. Invitee token: **Staff Invite Inbox** → **Accept Staff Invite**
   3. Or admin: **Revoke Staff Invite** while still `PENDING`
9. Mini-CRM (Admin at gym; needs `gymOrgId`):
   1. **Leads → Create Lead** (stores `leadId`; may return soft `warnings`)
   2. **Change Lead Status** / **Update Lead** (set `followUpDate`)
   3. **List Due Follow-ups** → **Soft Delete Lead**
10. Plan catalog (Admin at gym; needs `gymOrgId`):
   1. **Plans → Create BASE Plan** (stores `planId`)
   2. Optional **Create ADDON Plan** (`capability: TRAINER_COACHING`)
   3. **List Plans** / **Get Plan** / **Update Plan** → **Soft Delete Plan**
11. Membership invites + grants (needs BASE `planId`):
   1. Admin token: **Membership Invites → Create Membership Invite** (stores `membershipInviteId`; use invitee CLIENT email)
   2. Client token (same email): **Membership Invite Inbox** → **Accept Membership Invite** (stores `membershipId`)
   3. Client: **Get My Data Grants** / **Update My Data Grants** while ACTIVE
   4. Or admin: **List Membership Invites** → **Revoke Membership Invite** while still `PENDING`

`baseUrl` defaults to `http://localhost:3000` (override for hosted environments).

## Safety

Do not commit real OTP codes, access tokens, or refresh tokens. Keep secret values in your local Postman environment only.
