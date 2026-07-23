# 0004. NGROK_AUTHTOKEN supplied via gitignored .env file

- Date: 2026-07-18
- Status: Accepted

## Decision

The ngrok authtoken is stored in a local `.env` file (gitignored) and consumed by Compose as `NGROK_AUTHTOKEN`, injected into the `ngrok` service's environment. `.env.example` (committed, no real value) documents the required variable.

## Context

The token must never be committed. Compose natively reads `.env` in the project root.

## Alternatives Considered

- Hardcoding the token in `docker-compose.yml` — rejected, would leak the secret if committed.
- Docker secrets / external secret manager — overkill for a personal single-host project in v1.

## Consequences

- Anyone running this project must copy `.env.example` to `.env` and fill in their own token before `docker compose up`.
- `.gitignore` includes `.env` but not `.env.example`.
