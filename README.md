# duetale.com — landing page

One static page. No build step, no dependencies, no framework. Open
`index.html` in a browser and it works; put the directory behind any static
host and it works there too.

```
index.html      the page — markup, tokens, styles and the theme toggle, inlined
brand/          logo SVGs, favicons, app icons, web manifest, share card
sitemap.xml     one entry, because there is one public page
robots.txt      allow all, and point at the sitemap
```

## Brand

Everything under `brand/` is vendored from `duetale_brand_package/` in the
application repository, which is canonical. Do not edit these files here —
change them there and re-vendor, or the two surfaces drift.

One thing differs deliberately from the supplied package: the
`brand/icons/site.webmanifest` colours are the light ground `#FFFFFF`, because
light is the site's default and a dark manifest flashes the wrong colour when
the page is launched from a home screen. `brand/fonts/` carries the self-hosted
Host Grotesk and Fragment Mono files (SIL OFL), copied from the Go app.

The brand is v3.0, "The Signed Statement" (10 October 2026). The wordmark is
`duetale`: Host Grotesk 600, −0.035em, always lowercase, no full stop, beside the
signed-d mark on the accent plate. It is drawn inline in `index.html`, painted
from `--accent` and `--on-accent`, so it follows the theme toggle with no second
asset. Never recolour it, rotate it, crowd it, or set it on a photograph.

The wording is shared with the Go app's landing page (`duetale-go`), which
takes its copy from this page: change a sentence here and change it there.

## Tokens

The `:root` blocks in `index.html` mirror
`../duetale-go/internal/web/static/tokens.css` exactly, because
the page inlines its styles rather than importing them. They are not kept in
sync by good intentions:

```
node ../scripts/check-tokens.mjs
```

Run it after touching either file. It exits non-zero and prints every
difference.

## Before the first deploy

`og:image`, `twitter:image` and `<link rel="canonical">` are absolute and
point at `https://duetale.com/`. Every scraper requires an absolute image URL,
so this cannot be made relative — but it does mean the share card will not
resolve until the site is actually served from that domain. If you deploy
somewhere else first, update those three tags or the preview will come back
blank.

The page's own asset links are relative, so the mark and the icons work under
a project path (a GitHub Pages preview, for instance) without any change.

## Waitlist

The form posts to Google Forms with `mode: 'no-cors'`. The response is opaque
— status `0`, unreadable body, and it resolves for 200, 400 and 500 alike — so
the confirmation copy says the details were sent, never that they were
accepted. `../scripts/waitlist-endpoint.gs` holds the Apps Script side.
