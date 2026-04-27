# Architecture

> Single source of truth for **how the site is wired**. Update this file the moment any structural fact below stops being true.

## 1. One-paragraph overview

A single `index.html` (≈55 KB) containing inline CSS and a small JS module. All assets sit in `/assets`. Netlify serves the static files from its CDN, terminates HTTPS, applies the headers from `_headers`, and runs the redirects in `_redirects`. Deploys are triggered by GitHub Actions on every push to `main`.

## 2. File map

```
udesa-avanza/
├── index.html              ← THE page. Inline <style>, inline <script>.
├── manifest.webmanifest    ← PWA metadata
├── robots.txt              ← crawler directives + AI bot allow-list
├── sitemap.xml             ← image+video sitemap, single <url> for the SPA
├── netlify.toml            ← Netlify build/plugins config
├── _headers                ← CSP-light, immutable cache for /assets/*
├── _redirects              ← /instagram, /ig, /udesa shortcuts; SPA fallback
│
├── BRAND.md
├── README.md
├── AGENTS.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── LICENSE
├── .gitignore
├── .github/workflows/deploy.yml
│
├── assets/
│   ├── favicon.ico
│   ├── logo-transparent.png       ← CEU mark, white removed via ffmpeg colorkey
│   ├── udesa-logo.jpg             ← Universidad de San Andrés institutional crest
│   ├── og-image.jpg               ← 1200×630 social-share card
│   ├── campus-1.jpg … campus-5.jpg  ← official Campus Victoria photos
│   ├── ingresantes-1.jpg … ingresantes-7.jpg  ← Ingresantes 2026 photos
│   ├── udesa-volvemos.mp4         ← hero loop video (720×1280)
│   ├── ingresantes-2026.mp4       ← gallery video (720×960)
│   ├── ingresantes-2026-2.mp4     ← original closing video (480×640) — kept for reference
│   ├── ingresantes-2026-2-hd.mp4  ← upscaled closing video (1080×1440) — used in production
│   └── brand-*.png                ← partner logos for Beneficios section
│
├── archive/
│   ├── v1-cinematic.html
│   ├── v4-bento.html
│   ├── v5-immersive.html
│   └── index-versions-page.html   ← old version selector
│
└── docs/
    ├── architecture.md            ← THIS FILE
    ├── content.md                 ← editorial source map
    ├── checklists/
    │   ├── pre-commit.md
    │   └── post-deploy.md
    ├── decisions/
    │   ├── 0001-static-html.md
    │   ├── 0002-netlify-hosting.md
    │   ├── 0003-documentation-standard.md
    │   └── 0004-mandatory-docs-rule.md
    └── specs/
        ├── seo.md
        ├── menu-overlay.md
        ├── responsive.md
        ├── deployment.md
        └── media-pipeline.md
```

## 3. Page anatomy (`index.html`)

The page reads top-to-bottom as a print magazine. Every section has an `id` so the burger menu can scroll to it.

| Order | `id` | Visual | Content |
|---|---|---|---|
| 1 | `hero` | Full-bleed video + cream headline | "El espacio donde las ideas se vuelven realidad." |
| 2 | `intro` | Centered serif statement | Misión + 4 stat blocks |
| 3 | (no id; `.chapter`) | Image left / text right | Capítulo 02 — Diálogo |
| 4 | `programs-anchor` | Text left / image right (flipped, dark) | Capítulo 03 — Propuestas (8 items) |
| 5 | (no id; `.chapter`) | Image left / text right | Capítulo 04 — Locker (Edificio Hirsh) |
| 6 | `gallery-anchor` | Drag-scrollable horizontal gallery | Capítulo 05 — 8 campus images |
| 7 | `benefits-anchor` | Two-column grid list | Capítulo 06 — 8 partner cards |
| 8 | `equipo-anchor` | Dark band, 3 columns | Capítulo 07 — Mesa Ejecutiva |
| 9 | `closing` | Video + dark gradient | Capítulo 08 — Sumate (CTA) |
| 10 | `footer` | 4-column footer | Brand, navigation, contacto |

Plus, layered at z-index 200: the `.menu-overlay` (the burger menu).

## 4. JavaScript surface

Roughly 80 lines. Responsibilities:
1. **Burger menu** — open/close overlay, smooth-scroll to anchor, ESC handler, body scroll lock.
2. **Header scroll state** — toggles `.scrolled` on `<header>` after 80 px.
3. **Intersection-observer reveal** — fades sections in on scroll.
4. **Parallax on chapter images** — small `background-position-y` shift.
5. **Drag-scroll on gallery** — mouse-down → grabbing.
6. **Preloader fake progress** — counts to 100, then fades.

Each block is commented with a `// ====== Name ======` banner. If you grep for one, you find the other.

## 5. CSS strategy

- All styles inline, in source order.
- Sections are commented with `/* ====== Section ====== */` banners.
- Three breakpoints: `≤1024` (tablet), `≤768` (mobile), `≤480` (small mobile).
- One `(hover: none) and (pointer: coarse)` block to disable hover-only effects on touch.

## 6. Hosting & delivery

```
Browser
  │
  ▼
Netlify Edge CDN (HTTPS, HTTP/2)
  │  └── _headers / _redirects applied at the edge
  │
  ▼
Static origin (Netlify Blob storage, generated by deploy)
```

Deploys originate from the GitHub Actions workflow, which calls `netlify deploy --prod` with credentials read from repo secrets.

## 7. Where to look when something breaks

| Symptom | Look at |
|---|---|
| 404 on a path | `_redirects`, then `index.html` markup |
| Wrong cache headers | `_headers` |
| CI deploy fails | `.github/workflows/deploy.yml` + Actions log |
| Lighthouse regression | `netlify.toml` Lighthouse plugin output |
| OG card preview wrong | `<meta property="og:*">` block at top of `index.html` |
| Sitemap missing entry | `sitemap.xml` (it's hand-written) |
