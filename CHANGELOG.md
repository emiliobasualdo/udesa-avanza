# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]

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
