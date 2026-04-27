# Contributing

Thanks for helping! This repo runs primarily on AI-agent contributions, but humans are very welcome.

## Before you change anything

1. Read [`AGENTS.md`](AGENTS.md) — yes, even if you are a human.
2. Read the spec for the area you'll touch in `docs/specs/`.
3. Look at the latest ADRs in `docs/decisions/`.

## The workflow

1. **Branch:** `git switch -c <type>/<short-slug>` (e.g. `feat/new-beneficio-microsoft`).
2. **Edit + document.** A change without a docs update is incomplete — see [`AGENTS.md` §1](AGENTS.md).
3. **Run the [pre-commit checklist](docs/checklists/pre-commit.md).**
4. **Commit** with a [Conventional Commit](https://www.conventionalcommits.org/en/v1.0.0/) message:
   - `feat:` — new feature
   - `fix:` — bug fix
   - `docs:` — doc-only change
   - `chore:` — tooling, deps
   - `refactor:` — no behavior change
   - `perf:` — perf improvement
   - `style:` — formatting
   - `ci:` — CI/CD config
5. **Open a PR** against `main`. Netlify will comment a preview URL.
6. **Test the preview** on a phone (real device or DevTools "iPhone 14").
7. **Merge** when checks are green.

## Code style

- 2-space indent in HTML and JS.
- Spanish for user-visible text. English (concise) for comments.
- Class names in `kebab-case`.
- Keep new dependencies out unless an ADR justifies them.

## Reporting issues

Open a GitHub issue. Use the templates if available; otherwise include:
- What you expected
- What happened
- Browser + viewport size (if visual)
- Screenshot or screen recording

## Code of conduct

Be kind. Assume good faith. We are a student organization first; this repo is a means.
