# openbot-deploy

Deploy OpenBot (`ghcr.io/copilotkit/openbot`) on **Prj88** via Openship, Tailscale-only.

## Access

`http://100.76.46.87:3010` — bound to Tailscale IP only, `OPENBOT_SINGLE_USER=true` (no sign-in; anyone on tailnet is admin).

## Required Openship env (project → env)

| Key | Value |
|---|---|
| `EMBEDDED_POSTGRES` | `on` |
| `PORT` / `SERVER_PORT` | `3001` |
| `KEY_ENCRYPTION_KEY` | `openssl rand -base64 32` — production refuses the example key |
| `OPENBOT_SINGLE_USER` | `true` |
| `TRUSTED_ORIGINS` | `http://100.76.46.87:3010` |
| `OPENBOT_APP_URL` | `http://100.76.46.87:3010` |
| `INTELLIGENCE_API_URL` | `https://api.intelligence.copilotkit.ai` |
| `INTELLIGENCE_GATEWAY_WS_URL` | `wss://realtime.intelligence.copilotkit.ai` |
| `INTELLIGENCE_API_KEY` | `cpk-...` from `npx copilotkit@latest login && project select` |
| `OPENAI_BASE_URL` | `https://onr.cogify.cv/v1` (OmniRoute on Prj88) |
| `OPENAI_API_KEY` | OmniRoute virtual key |
| `BOT_MODEL` | OmniRoute route name |

## Caveats

- Single shared browser for all bots (no supervisor — needs Docker socket).
- Routines/attachment-cull need external cron (`bun scripts/fire-routines.ts` from `/app/server`); wire an Openship job later if used.
- Do NOT set `OPENBOT_PUBLIC_URL`/`BETTER_AUTH_URL` to a public address under SINGLE_USER — startup refuses.
