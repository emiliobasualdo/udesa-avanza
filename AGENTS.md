# AGENTS.md

> **This file is the entry point for any AI agent (Claude Code, Cursor, GitHub Copilot Workspace, etc.) that touches this repository.**
> It follows the [agents.md](https://agents.md) emerging convention.
> Read this file **first**, in full, before making any change.

---

## 0. Who this repo is for

This repository is the **public landing site** of **UdeSA Avanza — Centro de Estudiantes Universitarios** (CEU) of the **Universidad de San Andrés** (Victoria, Buenos Aires, Argentina).

It is maintained primarily by **AI agents** receiving prompts from **non-technical student volunteers** (the rotating CEU board). They cannot read code; they will tell you what they want in Spanish, in plain words.

Your job, as an agent, is to:
1. Translate their request into a plan.
2. Execute it with minimum surprise.
3. **Document everything** so the next agent (or the next iteration of yourself, with no memory of this conversation) can pick up cleanly.

---

## 1. The rule that overrides everything else

> **DOCUMENTING EVERY SINGLE CHANGE, SPEC, AND FEATURE IS MANDATORY.**

No exceptions. If your change does not include a documentation update, **the change is not done.**

What "documenting" means in this repo (concretely):

| When you... | You MUST also update... |
|---|---|
| Add or modify a feature | `docs/specs/<feature>.md` (create if missing) |
| Make a non-obvious technical decision | `docs/decisions/NNNN-<slug>.md` (next ADR number) |
| Ship anything to production | `CHANGELOG.md` (top of file, today's date) |
| Touch the deploy pipeline | `docs/specs/deployment.md` |
| Rename, move, or delete a file | `docs/architecture.md` (file map section) |
| Change copy / content | `docs/content.md` |
| Change brand assets, colors, fonts | `BRAND.md` |
| Add a new agent-facing convention | This file (`AGENTS.md`) |

**No documentation = the PR is not mergeable. Period.**

A pre-commit checklist sits at [`docs/checklists/pre-commit.md`](docs/checklists/pre-commit.md).

---

## 2. Documentation standard we follow

This repo combines four well-known industry standards. Read each link once.

| Concern | Standard | Where it lives |
|---|---|---|
| Agent guidance | [**agents.md**](https://agents.md) convention | `AGENTS.md` (this file) |
| User docs structure | [**Diátaxis**](https://diataxis.fr/) — tutorials, how-to, reference, explanation | `docs/` |
| Architecture decisions | [**ADR**](https://adr.github.io/) — Architecture Decision Records | `docs/decisions/` |
| Changelog format | [**Keep a Changelog**](https://keepachangelog.com/en/1.1.0/) + [**SemVer**](https://semver.org/) | `CHANGELOG.md` |
| Commit messages | [**Conventional Commits**](https://www.conventionalcommits.org/en/v1.0.0/) | every commit |

If you are about to do something this standard does not cover, write a new ADR proposing the extension. Don't invent silently.

---

## 3. Repository map (authoritative)

```
udesa-avanza/
├── index.html                  ← the landing page (single source of truth)
├── manifest.webmanifest        ← PWA manifest
├── robots.txt                  ← crawler rules + GEO-friendly bots
├── sitemap.xml                 ← Google/Bing sitemap with image+video tags
├── netlify.toml                ← build & plugin config
├── _headers                    ← Netlify HTTP header rules
├── _redirects                  ← Netlify redirect rules
├── BRAND.md                    ← color palette, typography, identity
├── README.md                   ← project overview for humans
├── AGENTS.md                   ← THIS FILE — entry point for AI agents
├── CHANGELOG.md                ← release log (Keep a Changelog)
├── CONTRIBUTING.md             ← human contributor guide
├── LICENSE                     ← repo license
├── .gitignore
├── .github/
│   └── workflows/
│       └── deploy.yml          ← CI: deploys to Netlify on push to main
├── assets/                     ← all static media (images, videos, logos)
├── archive/                    ← previous landing iterations (do NOT deploy)
└── docs/
    ├── architecture.md         ← system overview + file map
    ├── content.md              ← editorial map: every text block & source
    ├── checklists/
    │   ├── pre-commit.md       ← run before committing
    │   └── post-deploy.md      ← run after each deploy
    ├── decisions/              ← ADRs (one file per decision, numbered)
    │   ├── 0001-static-html.md
    │   ├── 0002-netlify-hosting.md
    │   ├── 0003-documentation-standard.md
    │   └── 0004-mandatory-docs-rule.md
    └── specs/                  ← feature specs (one file per feature)
        ├── seo.md
        ├── menu-overlay.md
        ├── responsive.md
        ├── deployment.md
        └── media-pipeline.md
```

---

## 4. How to work in this repo (agent runbook)

### 4.1 Before you start
1. Read `AGENTS.md` (this file) — done if you're here.
2. Read the spec for the area you'll touch (`docs/specs/*.md`).
3. Read the latest 3 ADRs in `docs/decisions/`.
4. Skim `CHANGELOG.md` to see what shipped recently.
5. If anything is unclear, **stop and ask the user a single, focused question.**

### 4.2 While you work
- One conceptual change per commit.
- Use [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/): `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `perf:`, `style:`, `test:`, `ci:`.
- If you discover a hidden constraint, write it down in the relevant spec **as you discover it**, not later.
- Never delete an ADR. Mark it `Superseded by ADR-NNNN`.

### 4.3 Before you commit
Run through [`docs/checklists/pre-commit.md`](docs/checklists/pre-commit.md). It's short.

### 4.4 After you commit — push and deploy by default
**Always `git push` to `origin/main` after committing, unless the user explicitly says otherwise** (e.g. "don't push", "wait", "draft only"). Pushing to `main` triggers the Netlify production deploy automatically — that *is* the deploy step; there is nothing else to do. Do not wait for a separate "ok to push" prompt before pushing routine changes the user already approved.

Exceptions where you should pause and confirm:
- The change is risky, partial, or breaks the build locally.
- You're force-pushing or rewriting history on `main`.
- The user said to keep the work local / on a branch / behind a PR.

### 4.5 After deploy
Run through [`docs/checklists/post-deploy.md`](docs/checklists/post-deploy.md). It's also short.

---

## 5. Tech stack (intentionally boring)

| Layer | Choice | Why |
|---|---|---|
| Markup | **Plain HTML5** (single `index.html`) | No build step, no framework lock-in, fastest possible TTFB, easiest for next agent to read |
| Styling | **Inline `<style>`** | Same file = single source of truth, no FOUC, perfect Lighthouse |
| Scripts | **Plain ES6 modules** in `<script>` | No bundler, no polyfills, ships exactly what's written |
| Hosting | **Netlify** (free tier) | CDN included, automatic HTTPS, deploy previews, image-optim plugin |
| CI | **GitHub Actions** | Triggers on push, deploys via `netlify-cli` |
| Media | Hosted in `/assets` on Netlify CDN | Free up to 100 GB bandwidth/mo |

If you want to introduce a new dependency or a build step, **write an ADR first** (`docs/decisions/`). The bar is high.

---

## 6. Non-negotiables

- **Spanish copy only** (`lang="es-AR"`). Never insert English in user-visible text.
- **Mobile first.** Test at 375 px before you ship.
- **No tracking scripts** without a new ADR.
- **No copyrighted media** unless we have an explicit license. All current media comes from Universidad de San Andrés official sources or was provided by the CEU (see `docs/content.md`).
- **No PII** in commits, screenshots, or docs.
- **Keep the page weight under 12 MB** on first paint (excluding loop videos which lazy-play).

---

## 7. Common tasks — quick recipes

### "Update the list of beneficios"
1. Find the section in `index.html` (search for `class="ben-list-grid"`).
2. Edit text only. Keep markup structure identical.
3. Update `docs/content.md` → "Beneficios" table.
4. Add a row to `CHANGELOG.md` under `### Changed`.
5. Commit: `docs: update beneficios — <new partner>`.

### "Replace a video"
1. Drop the new file into `assets/`.
2. Edit `index.html` to point at the new filename.
3. Update `docs/specs/media-pipeline.md` if the encoding pipeline changed.
4. Update `sitemap.xml` (`<video:content_loc>`).
5. `CHANGELOG.md` → `### Changed`.

### "Add a new section"
1. Write the spec first: `docs/specs/<section>.md` (use the template at the bottom of `docs/specs/seo.md`).
2. Add the markup + styles in `index.html`.
3. Add an anchor ID and a link in the burger-menu overlay.
4. Update `sitemap.xml` priorities if needed.
5. `CHANGELOG.md` → `### Added`.

### "Deploy a one-off fix"
1. Branch off `main`.
2. Open a PR — Netlify auto-publishes a preview.
3. Test the preview URL.
4. Merge → main → auto-deploys to production.

---

## 8. Who to surface questions to

| Topic | Whom to ask (in the PR description) |
|---|---|
| Brand / tone of voice | CEU communications lead |
| Beneficios / partners | CEU treasurer (currently Tomás Massuh) |
| Site outage | CEU president (currently Máximo Basualdo Cibils) |
| Anything else | Open a GitHub issue tagged `@udesavanza/maintainers` |

---

## 9. License & attribution

All UdeSA institutional imagery is © Universidad de San Andrés and used with permission.
The CEU logo is © UdeSA Avanza.
Code is MIT — see `LICENSE`.

---

*Last reviewed: 2026-04-27 — when you read or edit this file, update this date.*

<!-- 2026-04-27: added §4.4 — push-and-deploy is the default after committing -->

