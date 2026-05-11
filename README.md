# BHG — Digital Experiences Landing

Single-page landing for **bighugegiant.com** focused on web & digital product capabilities, with a 5-case in-market results section (University of Kentucky, Hotel Tonight, iFLY, iDonate, Mission Workshop).

Originally prototyped in Claude Design; this repo is the handoff to engineering.

## What's in the box

```
web-landing/
├── Digital Experiences.html   # the page (self-contained: inline CSS + inline JS)
├── assets/
│   ├── uk/        # logo, 6-image carousel, hero video
│   ├── ht/        # logo, 6-image carousel
│   ├── ifly/      # logo, 6-image carousel
│   ├── idonate/   # logo, 6-image carousel
│   └── mw/        # logo, 2-image carousel, hero video
└── README.md
```

Total asset weight ≈ 3.7 MB (images optimized at 2200px max width). Two videos: `UK_video.mp4` (~18 MB) and `Mission_video.mp4` (~5.5 MB) — see "Optimization notes" below.

## How to integrate into bighugegiant.com

The HTML has **no header and no footer** — it was authored to drop into the existing site so the global header and footer wrap around it.

Three things to lift out of `Digital Experiences.html`:

1. **Fonts** — preserve the Google Fonts `<link>` from `<head>`:
   ```html
   <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&family=Bebas+Neue&family=Playfair+Display:ital,wght@1,400;1,500&display=swap" rel="stylesheet">
   ```
   If the site already loads DM Sans / Bebas Neue / Playfair Display, you can skip this — otherwise keep it.

2. **Inline `<style>` block** — move into the page-level stylesheet (or keep inline on this route). All custom properties are scoped under `:root` and class names are prefixed enough to coexist with existing styles, but spot-check `.btn`, `.eyebrow`, `.rule`, `.cap`, `.case-*` for any collisions.

3. **Body content** — paste everything between `<body>` and `</body>` (the five `<section>` / `<header>` / `<article>` blocks) inside your existing layout, between the global header and footer.

4. **Inline `<script>` at the bottom** — drives the scroll-reveal animation and the case-study carousels. Keep it at the end of the body, or move it into the site's bundle (it has no external deps).

### Asset paths

All asset references are relative: `assets/uk/UK_1.jpg`, `assets/mw/Mission_video.mp4`, etc. When you move the markup into the BHG site, either:

- Drop the `assets/` folder at the same level as the rendered page, **or**
- Find/replace `src="assets/` and `src='assets/` with whatever your CDN or static path is.

### Removing prototype scaffolding (optional)

A handful of `data-screen-label="…"` attributes are sprinkled across the sections — they were used by the design tool's preview pane and have no runtime effect. Safe to strip.

## Customization checklist for the dev

- [ ] Swap `mailto:hello@bighugegiant.com` in the CTA section if there's a preferred address
- [ ] Confirm the "Book an Intro Call" button at the bottom hooks into the right form / Calendly / etc.
- [ ] Check the page renders inside the site's existing header/footer chrome at all breakpoints (mobile collapse handled in the inline CSS, but verify against your nav)
- [ ] Decide whether the two `.mp4` files stay self-hosted or move to a video CDN

## Optimization notes

Images were resized to 2200px max width and re-encoded as JPEG during design — total dropped from ~40 MB to ~3.7 MB. The two videos were **not** transcoded (no ffmpeg available in the design tool). Worth a pass:

- `assets/uk/UK_video.mp4` (~18 MB → target ~4–5 MB at 1080p H.264, ~3–5 Mbps)
- `assets/mw/Mission_video.mp4` (~5.5 MB → smaller win, optional)

Both autoplay muted on loop, so quality vs. bandwidth tradeoff is straightforward.

## Browser support

Modern evergreen browsers. Uses CSS custom properties, CSS Grid, `aspect-ratio`, and `IntersectionObserver`. No build step required.
