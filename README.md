# Gym Backend — Postman

Standalone Postman collection for the [gym-backend](https://github.com/abdulhasibn/gym-backend) Express API.
Share this repo with the team — no paid Postman plan required.

## Files

| File | Purpose |
| ---- | ------- |
| `Gym-Backend-API.postman_collection.json` | Auth MVP requests + tests |
| `Gym-Backend-Local.postman_environment.json` | Local `baseUrl` (`http://localhost:3000`) |
| `Gym-Backend-Dev.postman_environment.json` | Dev environment variables (same keys as Local) |

## Import

1. Clone this repo.
2. Open Postman → **Import** → select the collection and the environment(s) you need.
3. Select **Gym Backend — Local** or **Gym Backend — dev**.

## Smoke flow

1. Set `email`, `lane` (`CLIENT` or `STAFF`), and `name` in the environment.
2. Run **Auth → Email OTP → Request OTP**.
3. Paste the email code into `otpToken`.
4. Run **Verify OTP** (stores `accessToken` / `refreshToken` / `userId`).
5. Run **Auth → Session → Get Current User**.

`baseUrl` defaults to `http://localhost:3000` (override for hosted environments).

## Safety

Do not commit real OTP codes, access tokens, or refresh tokens. Keep secret values in your local Postman environment only.
