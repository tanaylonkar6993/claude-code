# 0003. Two-container Docker Compose topology: nginx + ngrok

- Date: 2026-07-18
- Status: Accepted

## Decision

`docker-compose.yml` defines two services: `web` (nginx serving the Astro build output) and `ngrok` (official `ngrok/ngrok` image tunneling to `web:80` over the internal Compose network). Neither runs on the host directly.

## Context

The site must be reachable publicly via ngrok without installing ngrok on the host, and without baking tunnel logic into the same container as the web server.

## Alternatives Considered

- Single combined container running both nginx and ngrok via a process supervisor — rejected: couples an infra concern (tunneling) to the app image.
- Running ngrok directly on the host — rejected: everything should be reproducible via Compose.

## Consequences

- `ngrok` reaches `web` by Compose service name (`web:80`) over the shared bridge network — no host port needs to be published for the tunnel to work.
- `web` also publishes `8085:80` to the host purely for local debugging outside the tunnel.
- Swapping ngrok for another tunneling tool later only touches the `ngrok` service definition.
