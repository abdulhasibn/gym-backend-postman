# Gym Backend — Postman

Standalone Postman collection for the [gym-backend](https://github.com/abdulhasibn/gym-backend) Express API.
Share this repo with the team — no paid Postman plan required.

## Files

| File | Purpose |
| ---- | ------- |
| `Gym-Backend-API.postman_collection.json` | Auth MVP requests + tests |
| `Gym-Backend-Local.postman_environment.json` | Local `baseUrl` + auth variables |

## Import

1. Clone this repo.
2. Open Postman → **Import** → select both `.json` files.
3. Select environment **Gym Backend — Local**.

## Smoke flow

1. Set `email`, `lane` (`CLIENT` or `STAFF`), and `name` in the environment.
2. Run **Auth → Email OTP → Request OTP**.
3. Paste the email code into `otpToken`.
4. Run **Verify OTP** (stores `accessToken` / `refreshToken` / `userId`).
5. Run **Auth → Session → Get Current User**.

`baseUrl` defaults to `http://localhost:3000`.

## Safety

Do not commit real OTP codes, access tokens, or refresh tokens. Keep secret values in your local Postman environment only.
