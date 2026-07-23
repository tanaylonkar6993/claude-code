# 0006. Light/dark theme toggle, vanilla JS, localStorage-persisted

- Date: 2026-07-18
- Status: Accepted

## Decision

The site ships both a light and dark CSS variable set and a user-facing toggle button to switch between them. The choice is persisted in `localStorage` and defaults to the visitor's `prefers-color-scheme` on first visit. Implemented with plain CSS custom properties and a small inline vanilla-JS snippet — no theming library.

## Context

The user wanted both palettes rather than committing to just one, with the visitor able to switch. A no-framework approach keeps the "keep dependencies light" constraint from decision 0002.

## Alternatives Considered

- Dark-only or light-only — rejected, user wanted both available.
- CSS-only toggle (checkbox hack, no persistence) — rejected, choice wouldn't survive a page reload.

## Consequences

- `BaseLayout.astro` includes a small inline `<script>` that runs before paint to read `localStorage`/`prefers-color-scheme` and set `data-theme` on `<html>`, avoiding a flash of the wrong theme.
- `ThemeToggle.astro` owns the click handler that flips `data-theme` and writes to `localStorage`.
