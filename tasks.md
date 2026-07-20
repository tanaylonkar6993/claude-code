# Tasks

Tracks implementation progress for the portfolio website. See `decisions/` for the "why" behind each architectural choice, and `/home/tanay-lonkar/.claude/plans/let-s-plan-and-build-mighty-cosmos.md` for the full original plan.

## Phase 0 — Project scaffolding
- [x] Initialize git repo (`git init`, default branch `main`)
- [x] Create `.gitignore`
- [x] Create `.env.example`
- [x] Create `decisions/` folder with template + 6 initial decision records
- [x] Create this `tasks.md`

## Phase 1 — Astro site
- [x] Scaffold Astro project (hand-written `package.json` pinned to Astro 4.x — local Node is 18.19.1, `create-astro` CLI requires Node 22+; Docker build stage uses `node:20-alpine` so this is a local-tooling-only workaround, not a product decision)
- [x] Add `src/data/profile.json`
- [x] Add `src/data/projects.json`
- [x] Build `src/layouts/BaseLayout.astro` (incl. no-flash theme-init script)
- [x] Build `src/components/Header.astro`
- [x] Build `src/components/Hero.astro`
- [x] Build `src/components/SocialLinks.astro`
- [x] Build `src/components/ThemeToggle.astro`
- [x] Build `src/components/ProjectCard.astro`
- [x] Build `src/components/ProjectsGrid.astro`
- [x] Build `src/components/Footer.astro`
- [x] Build `src/pages/index.astro`
- [x] Write `src/styles/global.css` (light + dark CSS vars, responsive, no framework)
- [x] `npm run build` succeeds locally; `npm run preview` looks correct (verified via curl, all 6 project names + name render)

## Phase 2 — Docker
- [x] Write `Dockerfile` (multi-stage: node build -> nginx:alpine)
- [x] Write `nginx/nginx.conf` (incl. commented-out future `/api/` seam)
- [x] `docker build -t portfolio .` succeeds
- [x] `docker run --rm -p 8085:80 portfolio` serves the site correctly (had to start Docker Desktop via `systemctl --user start docker-desktop` first — it wasn't running)

## Phase 3 — docker-compose + ngrok
- [x] Write `docker-compose.yml` (web + ngrok services, shared network, web on host port 8085)
- [x] `docker compose config` validates; `docker compose up -d web` verified end-to-end on :8085 (ngrok service left down — no token available in this session)
- [x] Copy `.env.example` -> `.env`, fill in real `NGROK_AUTHTOKEN` (user-provided token)
- [x] `docker compose up -d` (brings up both `web` and `ngrok`)
- [x] Verify both containers healthy (`docker compose ps` — `portfolio-web` and `portfolio-ngrok` both `Up`)
- [x] Fetch public URL via ngrok API (`curl http://localhost:4040/api/tunnels`) — free-tier URL, changes each restart
- [x] Confirm public URL serves the site correctly (curl returned HTTP 200 + correct `<title>`)

## Phase 4 — Content review (user)
- [x] Fork-inclusion policy resolved: include all 6 repos, honest framing (see decision 0005)
- [x] Language fields verified via GraphQL `primaryLanguage`
- [x] Resume/contact fields: GitHub only at first cut; `linkedinUrl` added 2026-07-20 (`email`/`resumeUrl` remain null)
- [x] Final read-through of `profile.json` bio text and each project description after site is live (user confirmed all text as-is, 2026-07-18)

## Phase 5 — Polish & docs
- [x] Update root `CLAUDE.md` with build/run commands and architecture summary
- [x] Add `README.md` with local dev + Docker instructions
- [x] Verified theme-toggle markup, CSS theme vars, and all 6 project links present in served HTML (no browser tool available this session — visual/manual cross-browser + responsive check still recommended, see note below)
- [x] Record any new decisions made along the way in `decisions/` (0007, Node 18 local Astro pin)
- [ ] Manual visual QA — cross-browser + responsive check, and confirm the theme toggle actually switches (only its markup has been verified, never its behavior)

## Repo tooling (not a product phase)
- [x] `.claude/agents/code-reviewer.md` — read-only review subagent scoped to this codebase
- [x] `.claude/settings.json` — PostToolUse hook (`Write|Edit|NotebookEdit`) running that agent. Reports only when it finds something; silence means a clean pass.

## Phase 6 — Future (out of scope for v1, tracked for awareness)
- [ ] Design backend API + database (contact form / view counters / dynamic projects)
- [ ] Add nginx `/api/` reverse-proxy location block once backend exists
- [ ] Add `backend` service to `docker-compose.yml`
