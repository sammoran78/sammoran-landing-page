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

## Assets

The two portraits are copied from `sammoran-academic/images/` (`sam-academic.jpg`
and `sam-red.jpg`) and downscaled to 900px. If those are ever re-shot, re-copy
and re-run the downscale, then regenerate `og.jpg`.
