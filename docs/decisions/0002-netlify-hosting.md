# ADR-0002: Netlify as host + CDN

- Status: Accepted
- Date: 2026-04-27

## Context

We need a host that:
1. Serves static files behind a global CDN.
2. Provides automatic HTTPS.
3. Has a free tier wide enough for ~10–50 GB media + low traffic.
4. Plays well with GitHub-Actions CI.
5. Generates preview URLs per PR.

Alternatives considered: Cloudflare Pages, GitHub Pages, Vercel.

## Decision

**Netlify**, free tier, deployed via the official `netlify-cli` from a GitHub Actions job.

## Consequences

### Positive
- Free tier ships with: 100 GB bandwidth/month, 300 build min, 125k function invocations.
- `_headers` and `_redirects` are first-class, no boilerplate.
- Preview deploys per PR are zero-config.
- Lighthouse plugin runs on every deploy and posts results.
- Netlify CDN serves video files with proper `Accept-Ranges: bytes` (validated in `_headers`).

### Negative
- 100 MB max single-file size. Our largest file (`ingresantes-2026-2-hd.mp4`) is ~10 MB — fine.
- Free tier has no analytics — we accept this.
- Vendor lock-in for `_headers` / `_redirects` syntax (mitigated: simple, readable, easy to port).

## Alternatives rejected
- **Cloudflare Pages**: similar feature set, but Lighthouse plugin is Netlify-native.
- **GitHub Pages**: no PR previews, no edge headers config.
- **Vercel**: equally good, but its "every commit becomes a preview" model is heavier than we need.
