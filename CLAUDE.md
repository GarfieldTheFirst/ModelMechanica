# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this project is

ModelMechanica is a static website hosted on **GitHub Pages** at `https://garfieldthefirst.github.io/ModelMechanica/`. It showcases browser-based interactive engineering tools — each project is a self-contained HTML file with no build step or external dependencies.

## Deployment

- Deployed automatically by GitHub Pages from the `main` branch root.
- `index.html` is the landing page that links to all project pages.
- To publish a change: `git push origin main` — GitHub Pages picks it up within ~30 seconds.
- No build tools, bundlers, or package managers involved.

## Adding a new project

1. Create a self-contained `<project-name>.html` file in the repo root (inline CSS + JS, no external files except Google Fonts and similar CDNs).
2. Add a project card to `index.html` in the `.projects` grid — copy the existing `<a class="project-card">` block and update the icon, name, description, tags, and `href`.

## Code conventions

- All projects share the same design system: CSS variables defined in `:root` (colors `--bg`, `--accent`, `--accent2`, `--accent3`, `--warn`, `--text`, `--muted`; fonts `--mono` = DM Mono, `--sans` = Space Grotesk).
- Dark theme (`--bg: #0a0c10`), cyan accent (`--accent: #00e5ff`).
- Keep each project file fully self-contained — no shared CSS or JS files across pages.

## Architecture of `imu-ai-lab.html`

Three-tab mobile app:
- **Tab 0 (Live Signal):** `DeviceMotion` API → `pushSample()` ring buffer (max 2000 samples). Falls back to `startDemo()` with simulated data when the API is unavailable.
- **Tab 1 (Anomaly):** Rolling Z-score over last 100 samples (`ANOM_WIN`); threshold at 2.5σ (`ANOM_THRESH`).
- **Tab 2 (Trend):** 1-second RMS windows accumulated in `rmsBuffer` (max 120 entries = 2 min); ordinary least-squares linear regression + extrapolation.
- Canvas rendering runs at 30 fps via `requestAnimationFrame`; all four canvases are DPR-scaled and lazily resized via `resizeCanvas()`.
