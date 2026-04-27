# Spec: Responsive design

> Goal: the page is comfortable to read and use on any screen from 320 px wide to 4K, on any input device (mouse, touch, keyboard).

## 1. Breakpoints

| Range | Audience | Layout shifts |
|---|---|---|
| ≤ 480 | Small phones (iPhone SE, older Androids) | Tight padding (18 px sides), single-column footer, smaller logo (32 px), gallery cards 84 vw |
| ≤ 768 | Phones / large phones | Header collapses (hamburger icon only, no label), hero `<br>` removed, intro stats 2×2, beneficios as 3-area grid (idx | name+det | pct), closing buttons full-width stacked |
| ≤ 1024 | Tablets, small laptops | Chapter sections stack 1-col, equipo to 2-col, footer to 3-col, menu overlay aside stacks below the link list |
| > 1024 | Desktop | Full editorial 2-col chapters, 4-col footer, generous padding |

## 2. Viewport handling

- `<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">`
- Hero uses `100vh` with a `100svh` override (small-viewport-height units) to prevent the iOS Safari URL-bar collapse jump.
- `min-height: 600px` on hero so landscape phones still get a real hero rather than a sliver.

## 3. Touch-device polish

A dedicated `@media (hover: none) and (pointer: coarse)` block:
- Disables hover-only transforms on `.gallery-card` and `.ben-row` (so a tap doesn't leave a stuck state)
- Sets `cursor: auto` on the gallery (was `grab`)
- Adds `-webkit-overflow-scrolling: touch` for momentum scroll

## 4. Tap targets

All interactive elements are ≥ 44×44 px on mobile (Apple HIG minimum). Specifically:
- Burger icon: 44×44 hit area (icon is 22×14, padded)
- Beneficios row: 24+ px vertical padding
- Closing CTA buttons: 16 px vertical padding × full-width

## 5. Typography scale

| Element | Desktop | Mobile (≤768) | Small (≤480) |
|---|---|---|---|
| Hero h1 | clamp(60px, 9vw, 168px) | clamp(44px, 11vw, 64px) | clamp(38px, 11vw, 52px) |
| Section h2 | clamp(40px, 5vw, 84px) | clamp(32px, 7vw, 48px) | clamp(28px, 8vw, 40px) |
| Chapter title | clamp(48px, 5.5vw, 96px) | clamp(36px, 8vw, 56px) | clamp(30px, 9vw, 44px) |
| Body | 18 px | 15 px | 15 px |

## 6. Image strategy

All `<img>` tags have `alt`. Background images (set via `style="background-image: url(...)"`) are decorative — the surrounding markup carries the semantic content.

Photos are served as-is (JPEGs from Universidad de San Andrés). They are pre-sized to 1080–1500 px wide; we do not resize on the fly because:
- The site is single-screen, so `<picture>`/`srcset` would add complexity for marginal payoff
- All images are immutable-cached for 1 year

## 7. `<br>` + display:none gotcha

When a `<br>` is hidden on mobile via `display: none`, browsers do **not** insert whitespace where it was. This caused "donde<br>las" to render as "dondelas" on phones. Fix is to put a literal space *before* the `<br>` in source markup:

```html
<h1>El espacio donde <br>las ideas...</h1>
```

When the `<br>` is hidden, the space remains and the text reads correctly. Apply this to every `<br>` that may be hidden on mobile.

## 8. Touch-action on horizontal scrollers

Any `overflow-x: auto` element that doesn't span the full viewport must declare `touch-action: pan-x` so vertical scrolls pass through to the page. Otherwise users get a "frozen" page when their finger is over the scroller.

Spec'd in `.gallery-track`:

```css
.gallery-track {
  overflow-x: auto;
  touch-action: pan-x;
  overscroll-behavior-x: contain;
  overscroll-behavior-y: auto;
}
```

## 9. Known issues

- iOS Safari < 15.4 doesn't support `100svh` and falls back to `100vh` (the URL-bar jump returns). Acceptable.
- On very narrow viewports (< 320 px) the hero CTA may overlap the scroll cue. Out of scope.
