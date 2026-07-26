# FlaVan Game Card Generator

A small, fully client-side web app that generates square **1080 × 1080** match graphics
for **FlaVancouver** — the Vancouver-based Flamengo supporters' club. Fill in the match
details on the left, watch a live 1:1 preview on the right, and download a high-quality
PNG ready for Instagram / Facebook / X.

Live: hosted on **Cloudflare Pages**, gated with **Cloudflare Access**.

## Running locally

There is **no build step** — it's plain HTML/CSS/JS. Because the app loads assets over
relative paths, serve it from a local web server rather than opening the file directly:

```bash
python3 -m http.server 8899
```

Then open <http://localhost:8899>.

## How it works

- **`index.html`** — the entire app: markup, styles, and logic in one file.
- **`assets/`** — background photo, club badge, and editor wordmark.
- **`crests/`** — team crest PNGs (transparent, trimmed to content), named `<key>.png`.

The card is authored at a fixed **1080 × 1080** canvas and CSS-scaled down for the preview.
Export renders that same element at true resolution via
[html-to-image](https://github.com/bubkoo/html-to-image), so the downloaded PNG is always
full quality regardless of the on-screen preview size.

Teams without a crest file (`remo`, `union-la-calera`) render an initials badge instead.
To add artwork later, drop `crests/<key>.png` in place and remove the key from the
`NO_CREST` set in `index.html`.

Three dependencies load from CDNs: Google Fonts (Oswald), Font Awesome 6.5.2, and
html-to-image 1.11.11. They can be vendored for offline robustness if desired.

## Deployment (Cloudflare Pages)

1. Connect this repo to Cloudflare Pages.
2. **No build command.** Output directory: the repo root (`/`).
3. Put the deployment behind a **Cloudflare Access** policy limited to the intended users.
   Authentication is handled entirely by Cloudflare — the app has no login screen and no
   user model.
4. It stays fully client-side: no backend, no database, no API keys.

## Asset licensing note

Club crests are trademarks. This is fine for a small supporters'-club tool, but the app
should not be made broadly public without considering that.

---

© FlaVancouver 2017–present
