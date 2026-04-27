# Spec: Deployment

> Goal: a non-technical CEU member never has to "deploy" anything by hand. Pushing to `main` is the entire procedure.

## 1. Pipeline overview

```
git push origin main
        │
        ▼
GitHub repo (emiliobasualdo/udesa-avanza)
        │
        ▼
.github/workflows/deploy.yml triggered
        │
        ├── checkout
        ├── setup Node 20
        ├── install netlify-cli
        ├── validate static files exist
        └── netlify deploy --prod   ──►  Netlify production (udesa-avanza.netlify.app)
                  │
                  ▼
            smoke-test job ──► curl checks (homepage, robots, sitemap, 5 critical assets)
```

## 2. Environment variables (GitHub Actions secrets)

| Secret | Source | Purpose |
|---|---|---|
| `NETLIFY_AUTH_TOKEN` | Netlify "Personal access tokens" page | Authenticates the CLI |
| `NETLIFY_SITE_ID` | Netlify site settings → "Site information" → API ID | Targets the right project |

To rotate either:
1. Generate a new value in Netlify.
2. `gh secret set NETLIFY_AUTH_TOKEN -R emiliobasualdo/udesa-avanza` (or via the GitHub web UI).
3. Re-run the latest deploy workflow to confirm.

## 3. Triggers

| Event | Result |
|---|---|
| Push to `main` | Production deploy → `https://udesa-avanza.netlify.app/` |
| Open / update PR | Preview deploy → unique `https://deploy-preview-N--udesa-avanza.netlify.app` URL, comment posted on PR |
| `workflow_dispatch` | Manual production deploy (Actions tab → "Run workflow") |

## 4. Concurrency

The job uses `concurrency: deploy-${{ github.ref }}` with `cancel-in-progress: true`. If you push twice quickly, only the latter runs.

## 5. Manual deploy (escape hatch)

If CI is broken and we must ship:

```bash
export NETLIFY_AUTH_TOKEN=$(cat ~/Library/Preferences/netlify/config.json | jq -r '.users | to_entries[0].value.auth.token')
export NETLIFY_SITE_ID=<the site id, also in that file>
npx netlify-cli deploy --dir=. --prod --auth $NETLIFY_AUTH_TOKEN --site $NETLIFY_SITE_ID
```

Document the use of this escape hatch in `CHANGELOG.md` so it's clear why a deploy didn't go through CI.

## 6. Rollback

Netlify keeps every deploy. To roll back:
1. Netlify dashboard → Deploys → find the last good one → "Publish deploy"
2. Add a row in `CHANGELOG.md` under `### Reverted`

## 7. Build settings (in `netlify.toml`)

```toml
[build]
  publish = "."
  command = "echo 'Static site — nothing to build.'"
```

Plus three plugins:
- `netlify-plugin-image-optim` — lossless image compression at deploy time
- `@netlify/plugin-lighthouse` — fails the build if scores drop below thresholds
- `@netlify/plugin-sitemap` — auto-generates a sitemap from links (kept in addition to our hand-written one as a backup)

## 8. Lighthouse thresholds (enforced)

| Category | Min |
|---|---|
| Performance | 0.80 |
| Accessibility | 0.90 |
| Best Practices | 0.90 |
| SEO | 0.95 |

Fail = build status "failed-with-warnings" (Netlify still publishes, but the deploy is flagged).

## 9. Post-deploy

The smoke-test job in the workflow runs:
- `GET /` → expect 200
- `GET /robots.txt` → expect 200
- `GET /sitemap.xml` → expect 200
- `GET /assets/{favicon, logo, udesa-logo, campus-1, hd-video}` → all expect 200

If any fails, the workflow marks the deploy red. The smoke test is **not** the same as the manual checklist in `docs/checklists/post-deploy.md` — both should be run.

## 10. Future work

- Atomic deploys with feature flags (Netlify supports this natively, not used yet).
- `IndexNow` ping on every prod deploy.
- Slack notification to `@udesavanza_ceu` chat on deploy success/failure.
