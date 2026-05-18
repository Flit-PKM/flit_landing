# Flit landing (`flit_landing`)

Static marketing site for Flit PKM: [`index.html`](index.html), [`features.html`](features.html), [`about.html`](about.html), [`apps.html`](apps.html); styles under [`static/css/`](static/css/), images under [`static/images/`](static/images/).

## Deploy and CI

- **[`.woodpecker/pipeline.yaml`](.woodpecker/pipeline.yaml)** runs **`validate`** (`html-validate` on all four HTML pages) then **`deploy`** (rsync to `/var/www/flit_landing`). Rsync uses **`--exclude=blog/`** so the server-managed blog at `/blog` is not deleted or overwritten by landing deploys.
- HTML rules live in **[`.htmlvalidate.json`](.htmlvalidate.json)** (SRI is off; **`input-missing-label`** is off because newsletter email fields use placeholder-only UX without visible `<label>`).
- **Titles**: use **`Flit - Note Taking & Personal Knowledge Management`** consistently in `<title>`, `meta name="title"`, `og:title`, and `twitter:title`.
- **Theme color**: `<meta name="theme-color">` uses **`oklch(0.63 0.2 240)`**, matching **`--color-blue`** in [`static/css/colors.css`](static/css/colors.css). [`site.webmanifest`](static/images/site.webmanifest) **`theme_color`** is **`#0092f4`** (same color in sRGB hex for manifest consumers).

## Behavior worth preserving

- **Newsletter signup** uses the same backend as before: form `id` **`newsletter-form-secondary`**, **`POST`** to the subscriptions API from script. There is **one** Cloudflare Turnstile widget (`data-widget-id="newsletter-turnstile"`) in the bottom newsletter block. **`onTurnstileSuccess`** enables the Subscribe button; submits read `cf-turnstile-response` from the **document**; on **201**, **`turnstile.reset(TURNSTILE_WIDGET_ID)`** runs and the Subscribe button is disabled again.
- **Thank-you dialog**: Escape closes; focus returns to the element that had focus before open.

## Assets and paths

- **Repo** currently ships SVG logos; [`static/images/site.webmanifest`](static/images/site.webmanifest) icons use paths **relative to the manifest file** (e.g. `flit_core_logo.svg`). `start_url` is absolute (`https://flit-pkm.com/`) for predictable PWA resolution.
- **Optional on deploy**: PNG favicons / Apple touch icons, `twitter-card2.png` for OG/Twitter (referenced in `index.html` meta). Add files under `static/images/` and restore `<link>` tags if you need them.

## Structured data (JSON-LD)

- Canonical `@graph` snippets live in [`static/schema/graph-reference.json`](static/schema/graph-reference.json); each page embeds its block in `<head>`.
- Stable entity `@id`s on `https://flit-pkm.com/`: `#organization`, `#brand`, `#website`, `#software-flit-web`, `#software-flit-android`; sub-pages use `#webpage` on their canonical URL.
- Full Organization / Brand / WebSite / SoftwareApplication graph is on **index only**; sub-pages reference those `@id`s plus page-local WebPage and BreadcrumbList.
- After edits, spot-check with [Google Rich Results Test](https://search.google.com/test/rich-results) and [Schema Markup Validator](https://validator.schema.org/) for `https://flit-pkm.com/`, `/features.html`, `/about.html`, and `/apps.html`.

## Related URLs

- [`sitemap.xml`](sitemap.xml) includes `https://core.flit-pkm.com/...` entries (different app server); the landing host is `flit-pkm.com`.
