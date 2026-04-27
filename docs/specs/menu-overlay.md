# Spec: Burger menu overlay

> Goal: a single navigation entry point that works the same on every screen size.

## 1. Behavior

| Trigger | Effect |
|---|---|
| Click `#menu-open` (the burger button in the header) | Overlay slides in from the top (`translateY(-100%)` → `0`), 0.7 s `cubic-bezier(.7,0,.2,1)`, body scroll-locked |
| Click `#menu-close` (the X button) | Overlay slides back out, body scroll restored |
| Click any anchor link inside the overlay | Overlay closes, then 500 ms later `scrollIntoView({behavior: 'smooth'})` scrolls to the target |
| Press `Escape` while overlay is open | Same as clicking close |

## 2. Markup contract

```html
<button class="h-menu-btn" id="menu-open" aria-controls="menu" aria-expanded="false">…</button>

<nav class="menu-overlay" id="menu" aria-hidden="true">
  <div class="menu-top">…<button id="menu-close">Cerrar</button></div>
  <div class="menu-body">
    <div class="menu-list">
      <a href="#hero" data-close>…</a>
      <a href="#intro" data-close>…</a>
      …
    </div>
    <div class="menu-aside">…</div>
  </div>
</nav>
```

Important:
- Every link has `data-close` so the JS knows to close the overlay before scrolling.
- The button toggles `aria-expanded`. The overlay toggles `aria-hidden`.
- `body.menu-open` adds `overflow: hidden` (CSS rule).

## 3. Anchor map

| Link | `href` | Targets `id=` |
|---|---|---|
| Inicio | `#hero` | `<section class="hero" id="hero">` |
| Misión | `#intro` | `<section class="intro" id="intro">` |
| Propuestas | `#programs-anchor` | `<section class="chapter flip dark" id="programs-anchor">` |
| Campus | `#gallery-anchor` | `<section class="gallery-strip" id="gallery-anchor">` |
| Beneficios | `#benefits-anchor` | `<section class="ben-chapter" id="benefits-anchor">` |
| Equipo | `#equipo-anchor` | `<section class="equipo" id="equipo-anchor">` |
| Sumate | `#closing` | `<section id="closing" class="closing">` |

If you add or rename a section, update **both** the anchor and this table.

## 4. Responsive behavior

- ≥ 1025 px: `menu-body` is 1.4fr / 1fr (link list left, aside right)
- ≤ 1024 px: aside drops below the link list
- ≤ 768 px: `menu-top` shrinks (logo 32 px, "Cerrar" label hidden, only X icon visible), link font scales down, body becomes scrollable

## 5. Accessibility

- All buttons have `aria-label` (the X button) or visible text
- `aria-expanded` on the trigger
- `aria-hidden` on the overlay
- ESC closes (keyboard equivalence)
- Focus is **not** yet trapped inside the overlay — known limitation, see "Future work"

## 6. Future work

- Trap focus inside the overlay while open
- `inert` on the rest of the page while open (replaces the manual scroll-lock)
- Animate menu items in with a stagger
