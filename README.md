# UdeSA Avanza — Centro de Estudiantes Universitarios

> Public landing site for **UdeSA Avanza**, the student government (CEU) of the **Universidad de San Andrés** (Victoria, Buenos Aires, Argentina).

**Live:** https://udesa-avanza.netlify.app

---

## What this is

A single-page, static HTML site that:
- Presents the 8 propuestas (proposals) of the CEU
- Lists active beneficios (student discounts) — Tuni, AACI, Sony, Lenovo, Natura, Bowen, Avon, Patagonian Ski
- Shows the Mesa Ejecutiva 2026 (executive board)
- Documents the locker service in Edificio Hirsh
- Drives traffic to `@udesavanza_ceu` on Instagram

It is intentionally framework-free: one `index.html`, one `assets/` folder, one Netlify deploy.

---

## For agents

> **Stop and read [`AGENTS.md`](AGENTS.md) before doing anything.**
> It is the operating manual for any AI agent (Claude, Cursor, Copilot, etc.) that touches this repo.

---

## For humans

### Local preview

```bash
cd udesa-avanza
python3 -m http.server 8765
# then open http://localhost:8765
```

That is the entire dev environment. There is no build step, no `npm install`, no compile.

### Deploy

Pushing to `main` triggers `.github/workflows/deploy.yml`, which deploys to Netlify automatically. PRs get preview URLs commented on the PR.

If you must deploy manually:

```bash
npx netlify-cli deploy --dir=. --prod
```

(Requires `NETLIFY_AUTH_TOKEN` and `NETLIFY_SITE_ID` env vars — see `docs/specs/deployment.md`.)

### Edit content

Almost every editorial change is a Find-and-Replace in `index.html`. After editing:

1. Run the [pre-commit checklist](docs/checklists/pre-commit.md).
2. Update [CHANGELOG.md](CHANGELOG.md).
3. Open a PR.

---

## Documentation

| File | What's in it |
|---|---|
| [`AGENTS.md`](AGENTS.md) | Mandatory entry point for AI agents |
| [`BRAND.md`](BRAND.md) | Color palette, fonts, logo usage |
| [`CHANGELOG.md`](CHANGELOG.md) | What shipped, when |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | How to contribute |
| [`docs/architecture.md`](docs/architecture.md) | File map + how the site is wired |
| [`docs/content.md`](docs/content.md) | Source of every text block on the page |
| [`docs/decisions/`](docs/decisions/) | Architecture Decision Records (ADRs) |
| [`docs/specs/`](docs/specs/) | One spec per feature |
| [`docs/checklists/`](docs/checklists/) | Run-before-shipping lists |

We follow:

- **[Diátaxis](https://diataxis.fr/)** for the structure of `docs/`
- **[ADR](https://adr.github.io/)** for `docs/decisions/`
- **[Keep a Changelog](https://keepachangelog.com/en/1.1.0/)** + **[SemVer](https://semver.org/)** for `CHANGELOG.md`
- **[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)** for every commit
- **[agents.md](https://agents.md)** for the agent entry point

---

## Tech stack

| Layer | Tool |
|---|---|
| Markup | Plain HTML5 |
| Styling | CSS in `<style>` (no preprocessor) |
| Scripts | Vanilla ES6 (no bundler) |
| Hosting | Netlify (free tier, CDN-fronted) |
| CI/CD | GitHub Actions → `netlify-cli deploy` |
| Repo | GitHub (public) |

---

## License

Code: MIT — see [`LICENSE`](LICENSE).
Images & video are © Universidad de San Andrés / CEU UdeSA Avanza.

---

*Maintained by UdeSA Avanza — Centro de Estudiantes Universitarios. Quaerere Verum.*
