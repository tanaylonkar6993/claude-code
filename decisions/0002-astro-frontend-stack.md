# 0002. Use Astro as the frontend framework

- Date: 2026-07-18
- Status: Accepted

## Decision

Build the site with Astro, outputting static HTML/CSS with minimal JS. No SPA framework (React/Vue/Svelte) for v1.

## Context

This is a content-driven, mostly-static portfolio with no client-side interactivity beyond a theme toggle. Astro produces minimal-JS static output well suited to nginx serving.

## Alternatives Considered

- Next.js / React SPA — unnecessary client JS and hydration overhead for a static content site.
- Plain hand-written HTML/CSS — no component reuse, harder to keep `projects.json` in sync with markup.
- Hugo/Jekyll — viable, but Astro keeps the project in the JS/TS ecosystem for easier future extension (API routes, islands).

## Consequences

- Build output is a static `dist/` folder, trivially served by nginx.
- If a future phase needs server-rendered/dynamic routes, Astro supports SSR adapters and API routes without a framework rewrite.
