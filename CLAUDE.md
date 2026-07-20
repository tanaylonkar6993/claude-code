# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Status

Personal portfolio website for github.com/tanaylonkar6993. Astro static site, served by nginx in Docker, exposed via ngrok. No backend/database (see `decisions/0001-...md` for why, and the seam left for adding one later).

See `tasks.md` for current progress and `decisions/` for the "why" behind each architectural choice — read those before making structural changes.

## Commands

```bash
npm install && npm run build   # build static site to dist/
npm run dev                    # local dev server, http://localhost:4321
npm run preview                # serve the production build locally

docker build -t portfolio .    # build the nginx+site image alone
docker compose up -d           # bring up web (nginx, :8085) + ngrok tunnel
docker compose down
```

There is no test suite or linter configured yet.

## Architecture

- `src/data/profile.json`, `src/data/projects.json` — all editable content. `projects.json` is wrapped in an envelope (`source`, `updatedAt`, `projects`) so a future `/api/projects` endpoint could return the identical shape without touching components.
- `src/layouts/BaseLayout.astro` — HTML shell; contains the no-flash light/dark theme-init script (reads `localStorage`, falls back to `prefers-color-scheme`, sets `data-theme` on `<html>` before paint).
- `src/components/` — `Header`, `Hero`, `SocialLinks`, `ThemeToggle`, `ProjectCard`, `ProjectsGrid`, `Footer`. All plain `.astro` components, no client framework.
- `src/styles/global.css` — plain CSS, no framework. Theme colors live in `:root[data-theme="dark"]` / `:root[data-theme="light"]` custom properties.
- `Dockerfile` — multi-stage: `node:20-alpine` builds the Astro static output, `nginx:1.27-alpine` serves it. Runtime image contains no `node_modules`.
- `nginx/nginx.conf` — static file serving + gzip. Has a commented-out `location /api/` block marking where a future backend would be reverse-proxied.
- `docker-compose.yml` — `web` (nginx, published on host `:8085`) and `ngrok` (tunnels to `web:80` over the internal Compose network). `NGROK_AUTHTOKEN` comes from a gitignored `.env` (see `.env.example`).

## Future phases (not built yet, tracked in `tasks.md` Phase 6)

Adding a backend + database later should only require: uncommenting the `/api/` block in `nginx/nginx.conf`, adding a `backend` service to `docker-compose.yml`, and pointing `projects.json`'s consumers at a live fetch instead of the static import — the data shape is already designed to match.
