# Spec: SEO + GEO (Generative Engine Optimization)

> Goal: maximum discoverability on classical search engines (Google, Bing) **and** generative engines (ChatGPT, Claude, Perplexity, Gemini).

## 1. What's in `index.html` `<head>`

| Block | Purpose |
|---|---|
| `<title>` | "UdeSA Avanza · Centro de Estudiantes Universitarios — Universidad de San Andrés" (≈ 80 chars; tradeoff vs 60 char ideal — accepted because brand keywords matter) |
| `<meta name="description">` | 240-char, plain-Spanish summary that includes "Centro de Estudiantes", "Universidad de San Andrés", "CEU", and key topical terms |
| `<meta name="keywords">` | Yes, even though Google ignores it. Bing and several AI crawlers still read it |
| Open Graph tags | Facebook, LinkedIn, WhatsApp link previews |
| Twitter Card tags | X / Twitter previews; included for completeness |
| `<link rel="canonical">` | Always points at the production URL |
| `<link rel="alternate" hreflang="es-AR">` + `x-default` | Region hint for Argentina |
| GeoTags (`geo.region`, `geo.placename`, `geo.position`, `ICBM`) | Argentina · Buenos Aires · Victoria, lat/lon of UdeSA campus |
| `<link rel="manifest">` | PWA install card |
| `<link rel="icon">` (×3 sizes) | Favicon, Apple touch icon |
| JSON-LD structured data | Drives rich snippets |

## 2. JSON-LD (structured data)

Three top-level entities in a single `@graph`:

1. **`EducationalOrganization`** — represents the CEU itself.
   - Includes `parentOrganization` (the University), `member` (the 3 board members), `address`, `geo`, `slogan`.
2. **`WebSite`** — the site itself, points back to `#ceu` as publisher.
3. **`FAQPage`** — five Q&A pairs covering the most common queries:
   - "¿Qué es UdeSA Avanza?"
   - "¿Cuáles son las propuestas del CEU?"
   - "¿Dónde reservo un locker en UdeSA?"
   - "¿Qué beneficios tiene la credencial UdeSA?"
   - "¿Quiénes integran la mesa ejecutiva?"

The FAQ format is specifically chosen because it's the format AI engines (ChatGPT, Perplexity) ingest most reliably.

## 3. `robots.txt`

Standard `User-agent: *` allow, **plus** explicit `Allow:` blocks for AI crawlers that respect `robots.txt`:

- `GPTBot`, `ChatGPT-User` (OpenAI)
- `ClaudeBot`, `Claude-Web`, `anthropic-ai` (Anthropic)
- `PerplexityBot` (Perplexity)
- `Google-Extended` (Gemini training data)
- `Applebot-Extended` (Apple Intelligence)
- `Bingbot`, `facebookexternalhit`

If new bots emerge, add them to the file and to this spec.

## 4. `sitemap.xml`

Single `<url>` entry (it's a one-page site). Includes:

- 5 `<image:image>` entries (campus photos + OG card)
- 2 `<video:video>` entries (`udesa-volvemos.mp4`, `ingresantes-2026-2-hd.mp4`) with thumbnails, durations, family-friendly flag

## 5. GEO-specific tactics

Generative engines reward content that is:

- **Clearly factual** — short declarative statements, no marketing fluff
- **Self-contained** — every important fact is on the same page, not behind a click
- **Cited-able** — names, dates, addresses, prices spelled out
- **FAQ-shaped** — questions and direct answers (we have a `FAQPage` entity)
- **Source-linkable** — anchor IDs on every section so engines can deep-link

We satisfy all five.

## 6. What we deliberately do NOT do

- No analytics scripts → keep page weight down, avoid GDPR/AR-PDP complexity
- No third-party social embeds → faster TTI, fewer trackers
- No `noindex` anywhere
- No JavaScript-only content (so SEO doesn't depend on JS execution)

## 7. Validation

After every deploy, the post-deploy checklist runs:

- Google Rich Results Test (`https://search.google.com/test/rich-results`)
- W3C HTML validator
- Open Graph preview (`opengraph.xyz`)

The Netlify Lighthouse plugin enforces SEO ≥ 0.95 on every deploy.

## 8. Future work
- Add language switcher if an English version is ever shipped (`hreflang="en-US"`)
- Consider IndexNow for Bing instant indexing
