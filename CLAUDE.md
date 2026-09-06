# safi-ai — project memory

Marketing site for Safi (صافي), "the AI cost layer". One static page, no build
system, no dependencies, no tests.

## Layout

- `index.html` — the whole site. Inline `<style>` in `<head>`, inline `<script>`
  at the end of `<body>`. Everything else is inline `style=""` attributes.
- `favicon.svg` — site icon.
- `.nojekyll` — makes GitHub Pages serve files verbatim.

## Commands

```bash
python -m http.server 8000    # serve locally at http://localhost:8000
```

No build, no lint, no test suite. Push to `main` to deploy.

## Deployment

GitHub Pages, `main` branch, root directory → https://alymo8.github.io/safi-ai/
A custom domain (ai-safi.com) is intended but not yet configured; `og:url` already
points there. Switching it on means adding a `CNAME` file plus DNS records.

## Conventions & gotchas

- **Styles are inline by design.** The page was converted from a component-based
  build to a single static file. Keep new styling inline rather than adding a
  stylesheet, so the file stays self-contained.
- **The `data-*` attributes are hooks for the responsive rules**, not decoration.
  `data-g` collapses grids to one column under 900px, `data-wrap` controls page
  gutters, `data-tbl` makes tables scroll horizontally, `data-navrow`/`data-nav`
  reflow the header. Removing one silently breaks a breakpoint.
- **Asset paths must stay relative** (`favicon.svg`, not `/favicon.svg`). The site
  is served from a subpath (`/safi-ai/`), so root-absolute paths 404.
- **The audit form posts to Formspree** (`https://formspree.io/f/mdavzqzg`). The
  endpoint appears twice — the `<form action>` (the no-JS fallback) and the
  `fetch()` call. Change both together.
- **Arabic content is RTL** and relies on `direction:rtl` plus the IBM Plex Sans
  Arabic / Noto Kufi Arabic families. Check any Arabic edit in a browser.
- **Benchmark numbers appear in several places** — the hero card, the stat band,
  the proof table, the "who saves" table, the FAQ, and `README.md`. Updating a
  figure means updating all of them.
