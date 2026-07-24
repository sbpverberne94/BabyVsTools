# AGENTS.md

This is a plain static site — no build step, no Jekyll, no Ruby. GitHub Pages
serves these files directly. The custom domain is set in `CNAME`.

## Files

- `index.html` — the entire page (markup + inline CSS-`<link>` + inline JS). No templating.
- `style.css` — all styling.
- `tools.json` — the data: one entry per pregnancy week, each with a comparison
  "tool" object and the image shown for that week. `index.html` fetches this
  file at load time.
- `images/` — the photos referenced by `tools.json`'s `img` field.

## Editing the week data

Edit `tools.json` directly — it's a plain JSON array, no code changes needed
for content updates:

```json
{ "week": 20, "lengte": "25.5", "gereedschap": "een KNIPEX Cobra 87 01 250", "img": "images/week20.jpg" }
```

- `week`: integer 1–40, must be unique and ideally contiguous (the slider
  covers 1–40; `findWeek()` in `index.html` falls back to the nearest lower
  week if one is missing).
- `lengte`: baby length in cm, as a string.
- `gereedschap`: Dutch description of the comparison object. The tool image
  link (see below) is derived from this text, so keep the actual product name
  near the front — extra explanatory text can go after a comma/period/paren
  and will be stripped from the search query.
- `img`: path to the image, relative to the repo root.

## Bol.com links

Each tool image links to a bol.com search for that product
(`bolSearchUrl()` in `index.html`). It strips a leading "de"/"een" article and
truncates at the first `.`, `,`, or `(` before URL-encoding, so
`"een KNIPEX Cobra 87 01 250"` becomes a search for `"KNIPEX Cobra 87 01 250"`.
This is a live search link (not a hardcoded product URL), so it stays valid
even if bol.com's catalog/URLs change — no per-item link to maintain.

## Local preview

No dependencies to install. Serve the directory over HTTP (fetch of
`tools.json` won't work from `file://`), e.g.:

```
python -m http.server 8000
```

then open `http://localhost:8000/`.
