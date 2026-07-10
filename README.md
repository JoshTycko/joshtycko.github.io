# Personal academic website

A hand-coded static site: plain HTML + CSS, self-hosted fonts, no JavaScript,
no build step, **no dependencies to keep updated**. If it renders today, it
will render the same way in ten years.

## Preview locally

From this folder:

```
python3 -m http.server 8000
```

Then open <http://localhost:8000>. (Opening the `.html` files directly also
works, but the local server matches how GitHub Pages serves them.)

## Where to edit what

Every placeholder in the HTML is marked with a `<!-- REPLACE: ... -->` comment.

| To change… | Edit… |
|---|---|
| Name, title, hero sentence, about text | `index.html` |
| Research interests | `index.html` (the `.interests` list) |
| Add a paper | `publications.html` — copy an existing `<li>`, edit its parts. Keep newest years first. Mirror the three to five most important ones in `index.html`. |
| Selected papers / press links | `publications.html` — the "Selected" section at top; the small `News & Views` / `Press coverage` links belong only on papers that actually have some |
| Headshot | Save a square photo (≥600×600) as `img/portrait.jpg`, then point the `<img>` in `index.html` at it (a comment there marks the spot) |
| Colors, spacing, type | `css/style.css` — everything is a custom property in `:root` |

The favicon (`img/favicon.svg`) is an "AR" monogram — change the initials in
the `<text>` element if the name changes.

## Deploy (GitHub Pages, free)

This folder is the `joshtycko.github.io` repository. GitHub Pages publishes
`main` automatically — **`git push` is the whole deploy**. The site serves at
<https://joshtycko.github.io/>.

To move it to a custom domain (e.g. `tyckolab.org`) later:

1. Repository **Settings → Pages → Custom domain** — GitHub adds a `CNAME`
   file to the repo.
2. At the DNS host, add the records GitHub shows (a `CNAME` from the domain to
   `joshtycko.github.io`, or the four apex `A` records). HTTPS is provisioned
   automatically.
3. Update the `rel="canonical"` links in each page's `<head>`.

`404.html` is picked up automatically by GitHub Pages. It uses absolute asset
paths (`/css/…`) because it is served at arbitrary URL depths — keep that if
you edit it.

## Maintenance

There is none. No packages, no framework, no build — just these files. The
fonts (Newsreader, Inter) are open-licensed (SIL OFL) and served from
`fonts/`, so the site makes no third-party requests and sets no cookies.
