# Hero media — drop your files here

The hero `<video>` on this page looks for two files in this folder:

| File | What it is | Notes |
|---|---|---|
| `hero.mp4`  | the looping hero video | H.264/AAC-free MP4, **no audio track** |
| `hero.webp` | the poster (first frame / still) | shown before/instead of the video; this is the LCP image |

Until both exist, the hero shows as an intentional dark brand panel (near-black
`#00031D` with white text) — nothing looks broken. Add the files and the loop appears
automatically on desktop.

## Behavior (already wired in `../index.html`)
- Plays only on screens ≥ 768px, with `prefers-reduced-motion` off and no data-saver.
- On mobile / reduced-motion / data-saver → **poster only** (protects Core Web Vitals).
- `muted loop playsinline preload="none"` — autoplays silently, no layout shift
  (height is reserved in CSS).

## Recommended source
Your own footage is best (unique + great for E-E-A-T): a PTV VISSIM/VISUM
micro-simulation screen capture, or drone footage over a Tbilisi intersection.
Free commercial-licensed alternatives: Coverr, Pexels, Pixabay, Mixkit
(search: "aerial intersection traffic", "drone roundabout", "top down traffic").

## Encoding (ffmpeg)
Aim for an 8–15s loop, ≤ ~2–3 MB, 1280–1920px wide, no audio.

```bash
# 1) Strip audio, trim to a clean loop, scale to 1600px wide, compress (MP4/H.264)
ffmpeg -i source.mov -an -t 12 -vf "scale=1600:-2,fps=30" -c:v libx264 -crf 28 -preset slow -movflags +faststart hero.mp4

# 2) Poster still from the first frame (WebP)
ffmpeg -i hero.mp4 -frames:v 1 -vf "scale=1600:-2" hero.webp
```

`-movflags +faststart` lets the video start before fully downloading.
Optional: also export `hero.webm` (VP9) and add a `<source>` for it in `index.html`.
