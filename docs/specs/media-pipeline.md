# Spec: Media pipeline

> Goal: every image and video on the site is in its smallest, sharpest, mobile-friendly form before deploy.

## 1. Image rules

| Rule | Why |
|---|---|
| Save as JPEG (`-q 75`) for photographs | best size/quality ratio for content photos |
| Save as PNG only when transparency matters | (e.g. `logo-transparent.png`) |
| Max 1500 px on the long side | The page never displays larger than ~1200 px. Bigger is wasted bytes. |
| Strip EXIF on commit | Privacy + 5–20 KB savings |
| Add to `sitemap.xml` `<image:image>` if it represents content | crawlable image SEO |

## 2. Video rules

| Rule | Why |
|---|---|
| Container: MP4 (H.264 + AAC or no audio) | Universal mobile support |
| `+faststart` | Allows progressive playback before full download |
| `pix_fmt yuv420p` | Required for iOS Safari |
| Strip audio when looping muted | Saves bandwidth |
| Loop videos < 12 s | Smaller initial payload |

## 3. The CEU logo (special case)

Source: `assets/logo.jpg` — flat-color CEU mark with hard white background.

To make it usable on dark backgrounds we run:

```bash
ffmpeg -y -i assets/logo.jpg \
  -vf "colorkey=0xFFFFFF:0.30:0.10,format=rgba" \
  -frames:v 1 \
  assets/logo-transparent.png
```

- `colorkey=0xFFFFFF:0.30:0.10` — chroma-key out white, with 0.30 similarity (handles JPEG-compressed near-whites) and 0.10 blend (soft edge)
- `format=rgba` — adds the alpha channel
- `-frames:v 1` — single-frame output

Result: `logo-transparent.png` (RGBA, color_type=6). Verified by reading the IHDR chunk.

If the source `logo.jpg` is ever replaced, re-run the command and commit the new PNG.

## 4. The closing video upscale

Source: `assets/ingresantes-2026-2.mp4` — 480×640 @ 877 kbps. Visibly low-quality on a desktop hero.

We render an HD version once, at deploy time:

```bash
ffmpeg -y -i ingresantes-2026-2.mp4 \
  -vf "hqdn3d=4:3:6:4.5,\
       scale=1080:1440:flags=lanczos,\
       unsharp=5:5:1.2:3:3:0.4,\
       eq=contrast=1.12:saturation=1.20:brightness=0.02" \
  -c:v libx264 -preset medium -crf 18 \
  -pix_fmt yuv420p -movflags +faststart \
  -an \
  ingresantes-2026-2-hd.mp4
```

- `hqdn3d=4:3:6:4.5` — high-quality 3D denoise (cleans compression artifacts before upscaling)
- `scale=1080:1440:flags=lanczos` — 2.25× Lanczos upscale
- `unsharp=5:5:1.2:3:3:0.4` — luma + light chroma sharpening
- `eq=contrast=1.12:saturation=1.20:brightness=0.02` — punchier image
- `crf 18` — visually lossless
- `-an` — audio-stripped (autoplay loops are muted anyway)

Result: 1080×1440 at ~7.2 Mbps, ~10 MB. ~8× the source bitrate.

The original source MP4 is kept (`ingresantes-2026-2.mp4`) for reproducibility.

## 5. The OG card

Source: `assets/campus-1.jpg`.

```bash
ffmpeg -y -i campus-1.jpg \
  -vf "scale=1200:630:force_original_aspect_ratio=increase,\
       crop=1200:630,\
       eq=brightness=-0.06:contrast=1.05:saturation=1.05" \
  -q:v 3 og-image.jpg
```

- `scale + crop` — produces an exact 1200×630 (Open Graph spec)
- mild EQ — slightly more dramatic than the raw

## 6. Adding new media

1. Drop the source into `/assets`.
2. Re-encode per the rules above.
3. Reference it in `index.html`.
4. If it's a hero/content image, add it to `sitemap.xml`.
5. Update `docs/content.md` → "Image inventory".
6. Add the encoding command to this file if it differs from the recipes above.

## 7. Why we keep this in-repo

- The CEU is a student org — no asset CDN budget.
- ffmpeg is installed once per CI runner; encoding is fast (~30 s).
- Keeping originals + recipes in version control means a new agent can re-derive every asset cleanly.
