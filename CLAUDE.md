# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Marketing site for **Manifest** — humanoids for European defence. Plain static HTML/CSS/JS, no build step. Deployed on Vercel.

## Run locally

```
npx serve .
```

Then open http://localhost:3000. The static server reads `serve.json` for the URL rewrites listed below — without it, `/about`, `/contact`, etc. 404 locally.

The harness already runs this server as `Static Server (serve)` (see [.claude/launch.json](.claude/launch.json)) — drive it via the `preview_*` tools rather than launching `npx serve` yourself.

## Architecture

Each top-level URL maps to a single self-contained HTML file — no shared partials, no templating. CSS and JS live in `<style>`/`<script>` blocks inside the HTML.

URL → file mapping is in [vercel.json](vercel.json) (production) and [serve.json](serve.json) (local). Keep both in sync when adding routes:

- `/` → [index.html](index.html) — homepage (Meet Pierre hero, Humanoid Month campaign, About teaser, footer)
- `/about` → [pages/about.html](pages/about.html) — long-form: mission, Strategic Autonomy pillars, Inspection, Partners, Team. **Bilingual (EN/FR)** via the `TRANSLATIONS` object and `data-i18n` attributes; switcher persists to `localStorage` under `manifest-lang`.
- `/contact` → [pages/contact.html](pages/contact.html) — contact form, posts to web3forms
- `/en`, `/fr` → both rewrite to `/index.html` (legacy language URLs from when the homepage was bilingual; the EN/FR switcher now lives on `/about`)
- `/legal-notice`, `/terms`, `/privacy-policy` → standalone subpages

The nav, mobile drawer, scroll-shrink logo, and form-validation script are duplicated across pages — when changing them, change every page that has them.

### Homepage hero is full-viewport

The homepage hero ("Meet Pierre") is the single full-screen entry section: dark background, `min-height: 100vh / 100svh`, two-column grid with headline + intro on the left and Pierre's photo on the right (stacks on mobile). The hero **is** the Meet Pierre section — they were merged, so don't reintroduce a separate `#pierre` section below it. Subsequent sections (Humanoid Month, About teaser) scroll in beneath.

### Caching

[vercel.json](vercel.json) sets `Cache-Control: max-age=0, must-revalidate` for everything except `/assets/video/*` (year-long immutable). This is deliberate: previously `/(.*)` was marked immutable, which meant replacing an image at the same path (e.g. a partner logo) wouldn't reach existing visitors until they cleared their cache. Don't reintroduce blanket long caching on the assets path. If you have to break the cache mid-deploy for a specific asset, bust it with a `?v=N` query string at the call sites.

### Images

- `assets/partners/`, `assets/team/`, `assets/logo.png` — logo and small site graphics.
- `assets/campaign/` — Humanoid Month photos. Pages reference the `.jpg` versions; we ship the optimized JPEGs only and keep the source PNGs out of the repo. When you add a new campaign photo: optimize the source to a `< 300 KB` JPEG before committing (use `sips -s format jpeg -s formatOptions 82 [-Z 900]`).
- The site logo (`assets/logo.png`) is white-on-transparent — it requires a dark backdrop. Don't place it on light backgrounds without inverting.

### Forms

Contact form posts to `https://api.web3forms.com/submit` with the access key inlined. Validation runs client-side first (required fields + email regex), then a `confirm()` dialog before submit.

### Debugging language popup (on `/about`)

```js
localStorage.removeItem('manifest-lang'); location.reload();
```
