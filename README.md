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

The social links live as bordered icon chips in the centre of the top bar.

On screens ≤560px the panels stack and the blurb is dropped: the portrait is
the point, and the kicker + title + button already carry the choice.

## Stack

Static HTML and one stylesheet. No build step, no dependencies, no JS.

```
index.html                 styles.css
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
