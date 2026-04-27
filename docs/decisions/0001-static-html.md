# ADR-0001: Single static HTML, no framework

- Status: Accepted
- Date: 2026-04-27
- Authors: Initial agent build (Claude)

## Context

The CEU site is a low-frequency-of-change marketing landing. Editorial updates happen a few times per term. Traffic is small but spiky (during admissions / campaigns). Maintainers will mostly be AI agents driven by non-technical CEU board members.

We considered: Next.js + Vercel, Astro, Eleventy, plain HTML.

## Decision

A single hand-written `index.html` with inline CSS and a small inline `<script>` block.

## Consequences

### Positive
- **Zero build step.** Anyone (or any agent) can edit and deploy in seconds.
- **Maximum readability** — one file to grep through.
- **Best-possible Lighthouse Performance** — no hydration, no JS framework, server pushes a single document.
- **No dependency surface** — no `npm audit` notifications, no Dependabot churn.
- **Cheapest possible hosting** — fits any free static host.

### Negative
- Re-using markup blocks requires copy-paste (we accept this — the page is small).
- Cannot easily add a CMS later without restructuring.
- Inline CSS means cache hit ratio is the same as the HTML itself (acceptable for a 55 KB file).

## Mitigations
- If the site grows past ~3 pages, revisit this decision in a follow-up ADR.

## Alternatives rejected
- **Next.js**: overkill, build step adds friction for non-technical edits.
- **Astro**: same as above.
- **Eleventy**: would help with multi-page, but we are single-page.
