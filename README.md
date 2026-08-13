# sammoran-landing-page

The router at the top of the funnel. Traffic arriving at a generic address
(sammoran.com.au and friends) lands here and gets pointed at whichever practice
the visitor actually wants:

| | |
|---|---|
| **Research** | [sammoran.phd](https://www.sammoran.phd/) — `sammoran-academic` |
| **Music** | [sammoran.music](https://www.sammoran.music) — `sammoran-music` |

## Design

A diptych. Deliberately not a third identity — it borrows from both children so
it reads as their shared parent:

- **Structure** from `sammoran-academic`: the Swiss grid, hairline ink rules,
  the inset paper frame, uppercase micro-labels, Arial + Iowan Old Style.
- **Warmth** from `sammoran-music`: the cream stock and the shared red/gold.

Both photos sit desaturated at rest. Hovering (or keyboard-focusing) a side
swells it to 62%, lifts the colour back into the portrait, floods the button
red and slides the `or` badge along the seam. The whole interaction is CSS —
`:has()` on the split, no JavaScript on the page at all.

The social links sit centred in the top bar as bordered chips carrying both an
icon and its name. Below 860px the labels drop and the chips become icon-only
squares — the labelled row needs ~517px of centre track, and under that the
wordmark starts getting squeezed.

## Caching — bump `?v=N` when an asset changes

Every mutable asset is referenced with a version query:

```
styles.css?v=3        images/sam-academic.jpg?v=2
og.jpg?v=2            images/sam-red.jpg?v=2
```

This is load-bearing, not cosmetic. `/images/*` is served
`max-age=31536000, immutable`, which is only safe *because* the URL changes when
the bytes do — without the query, a returning visitor would be pinned to the old
photos for a year. Both failure modes bit during development: a cached
stylesheet showed old layout against new markup, and cached portraits showed the
pre-alignment crops long after the files were fixed.

So: **edit an asset → bump its `?v=` in `index.html`.** For images, keep the
preload `href` byte-identical to the `<img src>` or the browser fetches twice.

On screens ≤560px the panels stack and the blurb is dropped: the portrait is
the point, and the kicker + title + button already carry the choice.

## Stack

Static HTML and one stylesheet. No build step, no dependencies, no JS.

```
index.html                 styles.css  (linked as ?v=N — bump on change)
images/                    two portraits, 900px wide, ~200KB total
og.jpg                     1200×630 share card
favicon.svg                apple-touch-icon.png
staticwebapp.config.json   cache + security headers, SPA fallback
robots.txt                 sitemap.xml
```

## Local preview

```bash
python -m http.server 4487
```

## Deploy

Azure Static Web Apps, via `.github/workflows/azure-static-web-apps-proud-pond-03f6ac300.yml`.
Pushing to `main` deploys; `app_location` is `/` with no build.

## Assets — the eye-line rule

The two portraits are re-cropped from the originals in `sammoran-academic/images/`
so that **the eye line sits at exactly 30% of image height in both**, at a shared
900×1349 aspect.

That number is load-bearing. `object-fit: cover` scales the image to the panel
width, so setting `object-position: 50% 30%` — the *same* fraction — pins the eyes
to 30% of the panel height at any panel size. The two faces therefore stay level
at rest, mid-swell, and stacked on mobile, with no per-breakpoint fiddling.
`transform-origin: 50% 30%` makes the hover zoom pivot about that line too.

The scrim is built around it as well: it must be fully transparent by 60% up,
because the chin lands near 58%. An earlier ramp hit 0.72 opacity at 34% and
washed straight over the face.

So if a portrait is ever re-shot: measure its eye line, re-crop to 30%, and
regenerate `og.jpg` (which uses the same invariant). Changing the CSS value alone
will pull the faces out of alignment.
