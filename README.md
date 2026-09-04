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

Two things differ deliberately from the supplied package, both for reasons
that only apply to a live site:

- `brand/icons/favicon.svg` carries a `prefers-color-scheme: dark` rule. The
  supplied file strokes `#0E1320`, which is invisible against a dark browser
  tab, and one `<link>` cannot carry two files. The dark value is the
  package's own `-reverse` colour, not a new one.
- `brand/icons/site.webmanifest` uses the light ground `#F2F4F8`. The supplied
  file is dark; light is the site's default, and a dark manifest flashes the
  wrong colour when the page is launched from a home screen.

The wordmark is `duetale.` — JetBrains Mono 700, always lowercase, the full
stop always present and always in `--cyan-text`. It is drawn inline in
`index.html` so the outline plate follows `currentColor` and the theme toggle
needs no second asset. Never recolour it, rotate it, soften its corners, or
set it on a photograph.

## Tokens

The two `:root` blocks in `index.html` mirror `../tokens.css` exactly, because
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
