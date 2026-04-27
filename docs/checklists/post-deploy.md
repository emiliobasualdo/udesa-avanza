# Post-deploy checklist

Run after every production deploy. Takes about 3 minutes.

## 1. Is it actually up?

```bash
curl -s -o /dev/null -w "%{http_code}\n" https://udesa-avanza.netlify.app/
```
Expect `200`.

```bash
for f in robots.txt sitemap.xml manifest.webmanifest; do
  echo -n "$f: "
  curl -s -o /dev/null -w "%{http_code}\n" "https://udesa-avanza.netlify.app/$f"
done
```
Expect three `200`s.

## 2. Are the assets reachable?

```bash
for f in favicon.ico logo-transparent.png udesa-logo.jpg og-image.jpg \
         campus-1.jpg ingresantes-1.jpg \
         udesa-volvemos.mp4 ingresantes-2026-2-hd.mp4; do
  echo -n "$f: "
  curl -s -o /dev/null -w "%{http_code}\n" "https://udesa-avanza.netlify.app/assets/$f"
done
```
Expect eight `200`s.

## 3. Headers correct?

```bash
curl -sI https://udesa-avanza.netlify.app/assets/favicon.ico | grep -i "cache-control"
```
Expect `Cache-Control: public, max-age=31536000, immutable`.

```bash
curl -sI https://udesa-avanza.netlify.app/ | grep -iE "strict-transport|x-frame|x-content"
```
Expect HSTS, `X-Frame-Options`, `X-Content-Type-Options`.

## 4. SEO endpoints

- [ ] Open https://search.google.com/test/rich-results?url=https://udesa-avanza.netlify.app/ → no errors
- [ ] Open https://www.opengraph.xyz/url/https%3A%2F%2Fudesa-avanza.netlify.app%2F → preview looks right
- [ ] Open https://validator.w3.org/nu/?doc=https%3A%2F%2Fudesa-avanza.netlify.app%2F → 0 errors

## 5. Functional smoke test

Open the live URL and click through, in this order:
- [ ] Hero video plays (autoplay) and loops
- [ ] Header "Suscribirme" pill scrolls to closing
- [ ] Burger menu opens, all 7 links scroll to the right section, X closes it
- [ ] Press ESC while menu is open → it closes
- [ ] Drag the gallery left/right → smooth, no jump
- [ ] Tap each beneficio row → no broken interaction
- [ ] Footer Instagram link opens `@udesavanza_ceu` in a new tab
- [ ] Footer UDESA logo opens `https://www.udesa.edu.ar` in a new tab

## 6. Mobile smoke test

- [ ] Open the URL on a real phone or DevTools "iPhone 14"
- [ ] Hero video plays inline (no fullscreen takeover)
- [ ] Burger menu overlay scrolls if content overflows
- [ ] Gallery scroll works with touch (momentum, no stuck state)
- [ ] No horizontal scroll anywhere
- [ ] Beneficios rows wrap correctly (3-area grid: idx | name+det | pct)

## 7. Lighthouse

The Netlify Lighthouse plugin runs on every deploy. Open the deploy log and confirm:

- Performance ≥ 0.80
- Accessibility ≥ 0.90
- Best Practices ≥ 0.90
- SEO ≥ 0.95

If any threshold fails, the deploy is marked degraded — open an issue.
