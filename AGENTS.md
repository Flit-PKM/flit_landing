# Flit landing (`flit_landing`)

Static marketing site for Flit PKM: single [`index.html`](index.html), styles under [`static/css/`](static/css/), images under [`static/images/`](static/images/).

## Deploy and CI

- **[`.woodpecker/pipeline.yaml`](.woodpecker/pipeline.yaml)** runs **`validate`** (`html-validate` on `index.html`) then **`deploy`** (rsync to `/var/www/flit_landing`).
- HTML rules live in **[`.htmlvalidate.json`](.htmlvalidate.json)** (SRI is off; **`input-missing-label`** is off because newsletter email fields use placeholder-only UX without visible `<label>`).
- **Titles**: use **`Flit - Note Taking & Personal Knowledge Management`** consistently in `<title>`, `meta name="title"`, `og:title`, and `twitter:title`.
- **Theme color**: `<meta name="theme-color">` uses **`oklch(0.63 0.2 240)`**, matching **`--color-blue`** in [`static/css/colors.css`](static/css/colors.css). [`site.webmanifest`](static/images/site.webmanifest) **`theme_color`** is **`#0092f4`** (same color in sRGB hex for manifest consumers).

## Behavior worth preserving

- **Two newsletter forms** share the same backend and **`POST` target**; each keeps a **unique** form `id`. There is **one** Cloudflare Turnstile widget (`data-widget-id="newsletter-turnstile"`) in the **top** newsletter block. **`onTurnstileSuccess`** enables **both** Subscribe buttons; submits read `cf-turnstile-response` from the **document**; on **201**, **`turnstile.reset(TURNSTILE_WIDGET_ID)`** runs once and **both** buttons are disabled again.
- **Thank-you dialog**: Escape closes; focus returns to the element that had focus before open.

## Assets and paths

- **Repo** currently ships SVG logos; [`static/images/site.webmanifest`](static/images/site.webmanifest) icons use paths **relative to the manifest file** (e.g. `flit_core_logo.svg`). `start_url` is absolute (`https://flit-pkm.com/`) for predictable PWA resolution.
- **Optional on deploy**: PNG favicons / Apple touch icons, `twitter-card2.png` for OG/Twitter (referenced in `index.html` meta). Add files under `static/images/` and restore `<link>` tags if you need them.

## Related URLs

- [`sitemap.xml`](sitemap.xml) includes `https://core.flit-pkm.com/...` entries (different app server); the landing host is `flit-pkm.com`.
