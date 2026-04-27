# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added
- **AGENTS.md §4.4 — push-and-deploy is the default**: after committing, agents push to `origin/main` without waiting for a separate "ok to push". Pushing *is* the deploy step (Netlify auto-publishes from `main`). Pause only for risky/partial changes or when the user says to hold.
- **Custom domain configured on Netlify**: `ceu.udesa.edu.ar` registered as the primary custom domain on the Netlify site (default fallback `udesa-avanza.netlify.app` stays active). SSL via Let's Encrypt will provision automatically once UdeSA IT adds the DNS record (`ceu CNAME udesa-avanza.netlify.app.`). Full procedure in [`docs/specs/deployment.md` §10](docs/specs/deployment.md).

### Changed
- **Header**: removed the centered "UdeSA Avanza" brand mark from the navbar. Header is now flex (menu button left, "Suscribirme" pill right). Cleaner, more breathing room.
- **Capítulo 07 (Equipo)**: brand mark relocated here as a full chapter signature — CEU logo + "UdeSA Avanza" italic, "Centro de Estudiantes Universitarios" caption, separated by a hairline rule.
- **`assets/og-image.jpg`**: replaced the campus photo with a branded composition — UdeSA logo top-left, CEU mark centered, small "UdeSA Avanza" wordmark underneath. 1200×630, 23.8 KB.

### Fixed
- **Hero headline word-spacing**: "donde<br>las" smushed to "dondelas" when `<br>` was hidden on mobile. Fixed by inserting a literal space before `<br>`. Same fix applied to closing headline ("importa.<br>Sumate").
- **Menu link smooth scroll**: native `scrollIntoView({behavior:'smooth'})` is silently throttled in some embeds and headless previews. Replaced with a custom rAF + easeInOutQuad scroller — works everywhere.
- **Gallery vertical-scroll trap on mobile**: `touch-action: pan-x` + `overscroll-behavior-y: auto` on `.gallery-track` so vertical pans bubble up to the page. The carousel only captures horizontal gestures now.

## [1.0.0] — 2026-04-27

### Added
- Single-page editorial landing at `index.html` (was `v6-springs.html` during prototyping).
- Hero with looping `udesa-volvemos.mp4` background video.
- 8-chapter scrolling layout: Misión, Diálogo, Propuestas, Campus, Galería, Beneficios, Equipo, Sumate.
- Burger menu overlay with 7 anchor links + smooth scroll, ESC-to-close, body scroll-lock.
- Drag-scrollable photo gallery with 8 official UdeSA images.
- Beneficios list with 8 active partners (Tuni, AACI, Sony, Lenovo, Natura, Bowen, Avon, Patagonian Ski).
- Mesa Ejecutiva 2026 cards (Máximo Basualdo Cibils, Benjamín Steed, Tomás Massuh).
- Closing section with HD upscaled video and gradient framing.
- Preloader with progress counter.
- Full SEO suite: meta description, OG tags, Twitter Card, canonical, hreflang, GeoTags.
- JSON-LD structured data: `EducationalOrganization`, `WebSite`, `FAQPage`.
- `robots.txt` with explicit allow-list for AI crawlers (GPTBot, ClaudeBot, PerplexityBot, Google-Extended).
- `sitemap.xml` with image and video extensions.
- `manifest.webmanifest` (PWA).
- `_headers` (CSP-light, immutable cache for assets, HSTS).
- `_redirects` (`/instagram`, `/ig`, `/udesa`).
- Transparent PNG variant of the CEU logo (chroma-keyed from JPG via ffmpeg).
- HD upscale of closing video: 480×640 → 1080×1440 with denoise + sharpen + EQ boost.
- Documentation suite:
  - `AGENTS.md` (agent entry point with mandatory-doc rule)
  - `README.md`, `BRAND.md`, `CONTRIBUTING.md`
  - `docs/architecture.md`, `docs/content.md`
  - `docs/checklists/pre-commit.md`, `docs/checklists/post-deploy.md`
  - 4 ADRs in `docs/decisions/`
  - 5 feature specs in `docs/specs/`
- GitHub Actions deploy workflow (`.github/workflows/deploy.yml`) with PR preview comments and post-deploy smoke tests.

### Changed
- Restructured: `v1-cinematic`, `v4-bento`, `v5-immersive` moved to `archive/`. Only v6 ships.

### Removed
- `v2-editorial.html`, `v3-aurora.html` (deleted — see [ADR-0001](docs/decisions/0001-static-html.md)).

[Unreleased]: https://github.com/emiliobasualdo/udesa-avanza/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/emiliobasualdo/udesa-avanza/releases/tag/v1.0.0
