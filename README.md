# joshtycko.com — personal academic website

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

| To change… | Edit… |
|---|---|
| Name, title, hero line, about text | `index.html` |
| Research interests | `index.html` (the `.interests` list) |
| Add a paper | `publications.html` — copy an existing `<li>` in the "All work" list, edit its parts, keep newest first. Add it to the "Selected" section too if it belongs there, and mirror the top few in `index.html`. |
| Selected papers / press links | `publications.html` — the "Selected" section at top; the small `Feature` / `News & Views` / `preLights` links belong only on papers that actually have coverage (link the real URL) |
| Headshot | Replace `img/portrait.jpg` with a square photo (≥600×600). The CSS renders it as a grayscale circle. |
| Colors, spacing, type | `css/style.css` — everything is a custom property in `:root` |

The favicon (`img/favicon.svg`) is a "JT" monogram — change the initials in
the `<text>` element if needed. The hero's dot-matrix is a decorative
`.hero__grid` in `index.html` (styled in `css/style.css`); delete that block to
remove it.

## Deploy (GitHub Pages, free)

This folder is the `joshtycko.github.io` repository. GitHub Pages publishes
`main` automatically — **`git push` is the whole deploy**. The site serves at
<https://joshtycko.github.io/>.

### Moving `joshtycko.com` here (off Owlstown), when ready

1. Repository **Settings → Pages → Custom domain** → enter `joshtycko.com` and
   save. GitHub adds a `CNAME` file to the repo.
2. At the domain's DNS host, point it at GitHub Pages:
   - Apex `joshtycko.com` — four `A` records to `185.199.108.153`,
     `185.199.109.153`, `185.199.110.153`, `185.199.111.153` (and, optionally,
     the matching `AAAA` records `2606:50c0:8000::153` … `8003::153`).
   - `www.joshtycko.com` — one `CNAME` to `joshtycko.github.io`.
3. Wait for DNS to propagate, then tick **Enforce HTTPS** in Settings → Pages.
4. Change each page's `rel="canonical"` (and `og:` URLs) from
   `https://joshtycko.github.io/` to `https://joshtycko.com/`.
5. Once it resolves, cancel the old Owlstown subscription.

`404.html` is picked up automatically by GitHub Pages. It uses absolute asset
paths (`/css/…`) because it is served at arbitrary URL depths — keep that if
you edit it.

## Maintenance

There is none. No packages, no framework, no build — just these files. The
fonts (Newsreader, Inter) are open-licensed (SIL OFL) and served from
`fonts/`, so the site makes no third-party requests and sets no cookies.
