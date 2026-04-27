# Pre-commit checklist

Run through this before opening a PR. It takes 2 minutes.

## 1. Did you update the docs?

- [ ] If a feature changed → relevant `docs/specs/<feature>.md` updated
- [ ] If a decision was made → new `docs/decisions/NNNN-*.md`
- [ ] If text or images changed → `docs/content.md` updated
- [ ] If anything ships → row added to `CHANGELOG.md` under `## [Unreleased]`
- [ ] If file structure changed → `docs/architecture.md` file map updated

> **No documentation = no merge.** ([AGENTS.md §1](../../AGENTS.md))

## 2. Sanity checks

- [ ] Page loads with no console errors at `http://localhost:8765`
- [ ] Burger menu opens, closes (link, X, ESC), scrolls to anchors
- [ ] All anchor links in the burger menu resolve to existing IDs:
  ```bash
  python3 -c "
  import re
  h = open('index.html').read()
  ids = set(re.findall(r'\bid=\"([^\"]+)\"', h))
  for a in re.findall(r'href=\"(#[^\"]+)\"', h):
      print(a, '✓' if a[1:] in ids else '✗ BROKEN')
  "
  ```
- [ ] Every `<img>` has an `alt` attribute (even if empty for decorative)
- [ ] Every `<a href="...">` to an external site has `rel="noopener"` if `target="_blank"`
- [ ] No `console.log` left in JS

## 3. Mobile check

- [ ] DevTools → device toolbar → iPhone 14 (375 px). Scroll the entire page.
- [ ] No horizontal scrollbar appears.
- [ ] All text is readable without zooming.
- [ ] All tap targets are ≥ 44×44 px.

## 4. SEO check

- [ ] `<title>` is < 60 chars and includes "UdeSA Avanza"
- [ ] `<meta name="description">` is 140–160 chars
- [ ] All new images on the page are referenced in `sitemap.xml` `<image:image>`
- [ ] If a new section was added, JSON-LD updated (FAQ entry if relevant)

## 5. Performance

- [ ] Total page weight (HTML + CSS + JS, no media) under 100 KB
- [ ] Any new image > 200 KB has a justification in the PR description
- [ ] Any new video re-encoded per `docs/specs/media-pipeline.md`

## 6. Commit message

- [ ] Follows [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/): `<type>(<scope>)?: <summary>`
- [ ] Body explains *why*, not *what* (the diff shows the *what*)
