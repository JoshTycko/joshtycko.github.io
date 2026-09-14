# joshtycko.com — personal academic website

A hand-coded static site: plain HTML + CSS, self-hosted fonts, no build step,
**no dependencies to keep updated**. The only scripts are two small inline ones
on each page, for scroll reveals and for rebuilding the obfuscated email link.
If it renders today, it will render the same way in ten years.

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
| Name, role line, about text | `index.html` |
| Research interests | `index.html` (the `.interests` list) |
| Add a paper | `publications.html`, inside the section with `id="publications"`. Copy an existing `<li>`, edit its parts, keep newest first, and renumber the countdown so it runs N down to 1. Give it `data-selected` only if it belongs in the Selected five, and if you do, mirror it into `index.html`, which shows that same five. |
| Selected papers / press links | `publications.html` — the "Selected" section at top; the small `Feature` / `News & Views` / `preLights` links belong only on papers that actually have coverage (link the real URL) |
| Headshot | Replace `img/portrait.jpg` with a square photo (≥600×600). The CSS renders it as a grayscale circle. |
| Colors, spacing, type | `index.html` has its own inlined `<style>`; `publications.html` and `404.html` use `css/style.css`. Both define the same custom properties in `:root`, so a palette change has to be made in both. |

Two conventions worth knowing before editing `publications.html`:

- The page is three sections (`id="publications"`, `id="patents"`, `id="other"`)
  inside one `.pubpage` grid, with a sticky index in the left rail that tracks
  which one you are reading. Adding a section means adding a `.secnav__link` too.
- A paper whose co-first authors are not in contribution order carries a
  `.pubs__note` line under its author list. Take the wording from Josh's CV, which
  distinguishes "listed alphabetically" from "can be reported in any order". Check
  the CV **PDF**: its full numbered list carries these annotations, while the
  Selected Publications section in the .docx is abridged and omits most of them.

The favicon (`img/favicon.svg`) is a "JT" monogram. Change the initials in the
`<text>` element if needed. The hero is deliberately plain: the role line, the
name, and the portrait, with no headline and no diagram (Josh, 2026-09-13).

## Deploy (GitHub Pages, free)

This folder is the `joshtycko.github.io` repository. GitHub Pages publishes
`main` automatically — **`git push` is the whole deploy**. The site serves at
<https://joshtycko.github.io/>.

### `joshtycko.com` (migrated off Owlstown 2026-09-13)

The site serves at <https://joshtycko.com>. `www` redirects to the apex and
`joshtycko.github.io` redirects to the custom domain. DNS is at **GoDaddy**
(nameservers `ns73/ns74.domaincontrol.com`): four apex `A` records to
`185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`, and a
`www` `CNAME` to `joshtycko.github.io`. The `MX` records point at
`secureserver.net` and carry Josh's mail, so leave them alone.

The `CNAME` file in this repo is what binds the domain to Pages. Do not delete it.
If the domain ever needs re-setting, configure DNS first, then
`PUT /repos/JoshTycko/joshtycko.github.io/pages` with `{"cname": "joshtycko.com"}`,
wait for the certificate, then a second call with `{"https_enforced": true}`. The
two cannot be set in one request, since HTTPS enforcement needs the certificate to
exist. Expect one failed Pages build at the moment the domain is set, from the
auto-commit that writes `CNAME`; the next build succeeds. HTTPS enforcement can
take up to an hour to show up at the edge after the API reports it.

A snapshot of the old Owlstown page and an audit of what was ported from it are in
`.claude/research/owlstown-archive/` (gitignored, local only).

`404.html` is picked up automatically by GitHub Pages. It uses absolute asset
paths (`/css/…`) because it is served at arbitrary URL depths — keep that if
you edit it.

## Maintenance

There is none. No packages, no framework, no build — just these files. The
fonts (Newsreader, Inter) are open-licensed (SIL OFL) and served from
`fonts/`, so the site makes no third-party requests and sets no cookies.
