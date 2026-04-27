# Content Map

> Every text block on the page, traced back to its source. Update this file whenever copy changes.

## 1. Source of truth

The factual content of this site mirrors **the previous CEU site** at `https://udesavanzaa.netlify.app/`. Visual identity follows **`https://www.udesa.edu.ar/`** (UDESA institutional). When the two diverge, the **CEU site wins for content**, and **UDESA wins for visual brand**.

## 2. Hero

| Element | Copy | Source |
|---|---|---|
| Eyebrow | "Universidad de San Andrés · Centro de Estudiantes" | new |
| Headline | "El espacio donde las ideas se vuelven realidad." | new (paraphrasing CEU mission) |
| Sub | "Estudiantes que trabajan para mejorar la experiencia universitaria. Diálogo abierto, propuestas concretas, comunidad activa." | paraphrased from CEU mission |
| Bottom-left | "VICTORIA · BUENOS AIRES · 1988" | UdeSA founding year |
| Bottom-right | "CICLO 2026 / Nº 01" | calendar |

## 3. Intro / Misión

> "Construir un Centro de Estudiantes que esté **verdaderamente al servicio** de la comunidad universitaria."

**Source:** literal quote from `udesavanzaa.netlify.app` mission statement.

Stat blocks:
| Number | Label | Source |
|---|---|---|
| 8 | Propuestas activas | count of items on CEU site |
| 8 | Beneficios vigentes | count of partners on CEU site |
| '88 | Fundación UDESA | UdeSA founding year |
| CEU | Mesa Ejecutiva 2026 | current term |

## 4. Capítulos (sections)

### Capítulo 02 — Diálogo
Body paraphrased from the "Escuchar y actuar con compromiso" block on `udesavanzaa.netlify.app`.

### Capítulo 03 — Propuestas (8)
Each item is taken **verbatim** from the CEU site:
1. Escuchar y actuar — Diálogo
2. Mejora del WiFi — Conectividad
3. Participación estudiantil — Voz
4. Estacionamiento — Campus
5. Vida deportiva — Deporte
6. Convenios y beneficios — Comunidad
7. Espacios de estudio — Académico
8. Oferta gastronómica — Campus

### Capítulo 04 — Locker
| Field | Value | Source |
|---|---|---|
| Ubicación | Edificio Hirsh | CEU site |
| Precio | $25.000 / semestre | CEU site |
| Cómo | Reserva por mail | CEU site |

## 5. Beneficios (8)

| Marca | Categoría | Descuento | Condiciones | Source |
|---|---|---|---|---|
| Tuni | Académicos | −10% en la primera clase particular | IRM · IPC · Economía I · Análisis Mat. I · Programación I | CEU site |
| AACI – Inglés | Académicos | −25% en cursos de inglés | Para alumnos, docentes, staff y graduados. Arenales 1913, CABA | CEU site |
| Sony | Tecnología | 5–10% | 5%: cámaras, lentes Alpha, video, consolas, VR. 10%: TV, audio, gaming | CEU site |
| Lenovo | Tecnología | EXCL. | Registrarse con mail @udesa.edu.ar | CEU site |
| Natura | Compras | −35% | 30% web, 35% + envío gratis app. Solo @udesa.edu.ar, un uso/mes | CEU site |
| Bowen | Compras | −20% | + 6 cuotas sin interés. Credencial digital + DNI | CEU site |
| Avon | Compras | −30% | 25% web (>$25.000), 30% + envío gratis app. Solo @udesa.edu.ar | CEU site |
| Patagonian Ski | Deportes | −15% | Para toda la comunidad UDESA | CEU site |

## 6. Mesa Ejecutiva 2026

| Rol | Nombre | Source |
|---|---|---|
| Presidente | Máximo Basualdo Cibils | `udesavanzaa.netlify.app` |
| Vicepresidente | Benjamín Steed | `udesavanzaa.netlify.app` |
| Tesorero | Tomás Massuh | `udesavanzaa.netlify.app` |

## 7. Closing / CTA

| Element | Copy |
|---|---|
| Eyebrow | "Capítulo 08 / Comunidad" |
| Headline | "Tu voz importa. Sumate al CEU." |
| Description | "Recibí novedades, beneficios y anuncios importantes directamente en tu mail universitario." |
| Primary CTA | "Seguinos @udesavanza_ceu" → https://instagram.com/udesavanza_ceu |
| Secondary CTA | "Ir a UDESA" → https://www.udesa.edu.ar |

## 8. Footer

| Field | Value |
|---|---|
| Brand line | "UdeSA Avanza" + "Centro de Estudiantes" |
| Address | Vito Dumas 284, Victoria, Buenos Aires |
| Instagram | @udesavanza_ceu |
| University | https://www.udesa.edu.ar |
| Motto | Quaerere Verum |
| Founded | 1988 |
| Issue | "Issue Nº 01 / 2026" |

## 9. Image inventory

All images live in `/assets`. See `docs/specs/media-pipeline.md` for the encoding pipeline.

| File | Subject | Source |
|---|---|---|
| `campus-1..5.jpg` | Campus Victoria, "5 razones que lo hacen único" | UdeSA Instagram (provided by user) |
| `ingresantes-1..7.jpg` | Ingresantes 2026, primer día | UdeSA Instagram (provided by user) |
| `udesa-volvemos.mp4` | "UdeSA ya está lista para recibirte" video | UdeSA Instagram (provided by user) |
| `ingresantes-2026.mp4` | Ingresantes 2026 video | UdeSA Instagram (provided by user) |
| `ingresantes-2026-2-hd.mp4` | Same video, upscaled 480→1080 | derived in repo |
| `udesa-logo.jpg` | Universidad de San Andrés crest | https://www.udesa.edu.ar/logo |
| `logo-transparent.png` | CEU mark, transparent | derived from `logo.jpg` via ffmpeg colorkey |
| `og-image.jpg` | Social share card 1200×630 | derived from `campus-1.jpg` |
| `favicon.ico` | UdeSA favicon | https://www.udesa.edu.ar/favicon.ico |
| `brand-tuni.png`, `brand-aaci.png`, `brand-sony.png`, `brand-lenovo.png`, `brand-bowen.png` | Partner logos | extracted from CEU site (base64) |
