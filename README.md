# Portfolio Website

Personal portfolio for [tanaylonkar6993](https://github.com/tanaylonkar6993), built with Astro, served by nginx in Docker, exposed publicly via ngrok.

See `decisions/` for why things are built this way, and `tasks.md` for what's done and what's left.

## Local development (no Docker)

```bash
npm install
npm run dev       # http://localhost:4321
```

```bash
npm run build     # outputs static site to dist/
npm run preview   # serve the production build locally
```

## Docker (single container)

```bash
docker build -t portfolio .
docker run --rm -p 8085:80 portfolio
# open http://localhost:8085
```

## Full stack: nginx + ngrok tunnel

1. Copy the env template and add your ngrok authtoken (get one at https://dashboard.ngrok.com/get-started/your-authtoken):
   ```bash
   cp .env.example .env
   # edit .env and set NGROK_AUTHTOKEN=<your token>
   ```
2. Bring up both containers:
   ```bash
   docker compose up -d
   ```
3. Site is reachable locally at http://localhost:8085 and publicly via the ngrok tunnel. Get the public URL:
   ```bash
   curl -s http://localhost:4040/api/tunnels | jq '.tunnels[0].public_url'
   ```
   (or open the ngrok inspection dashboard at http://localhost:4040)
4. Tear down:
   ```bash
   docker compose down
   ```

Note: on ngrok's free plan, the public URL changes every time the `ngrok` container restarts.

## Editing content

- `src/data/profile.json` — name, title, bio, social links.
- `src/data/projects.json` — the project cards. Add/remove/reorder entries here; no code changes needed.

## Project structure

```
src/
├── data/            profile.json, projects.json — all editable content
├── layouts/         BaseLayout.astro
├── components/      Header, Hero, SocialLinks, ThemeToggle, ProjectCard, ProjectsGrid, Footer
├── pages/           index.astro
└── styles/          global.css (light/dark theme variables, no CSS framework)
nginx/nginx.conf     nginx config used inside the Docker image
Dockerfile           multi-stage build: node -> astro build -> nginx:alpine
docker-compose.yml   web (nginx) + ngrok services
decisions/           one file per architectural decision
tasks.md             implementation task tracker
```
