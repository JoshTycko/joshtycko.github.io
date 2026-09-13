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
| Add a paper | `publications.html` — copy an existing `<li>` in the "All work" list, edit its parts, keep newest first. Add it to the "Selected" section too if it belongs there, and mirror the top few in `index.html`. |
| Selected papers / press links | `publications.html` — the "Selected" section at top; the small `Feature` / `News & Views` / `preLights` links belong only on papers that actually have coverage (link the real URL) |
| Headshot | Replace `img/portrait.jpg` with a square photo (≥600×600). The CSS renders it as a grayscale circle. |
| Colors, spacing, type | `index.html` has its own inlined `<style>`; `publications.html` and `404.html` use `css/style.css`. Both define the same custom properties in `:root`, so a palette change has to be made in both. |

The favicon (`img/favicon.svg`) is a "JT" monogram. Change the initials in the
`<text>` element if needed. The hero is deliberately plain: the role line, the
name, and the portrait, with no headline and no diagram (Josh, 2026-09-13).

## Deploy (GitHub Pages, free)

This folder is the `joshtycko.github.io` repository. GitHub Pages publishes
`main` automatically — **`git push` is the whole deploy**. The site serves at
<https://joshtycko.github.io/>.

### Moving `joshtycko.com` here (off Owlstown)

The DNS for `joshtycko.com` is at **GoDaddy** (nameservers `ns73/ns74.domaincontrol.com`).
As of 2026-09-13 the apex is a GoDaddy forward and `www` is a `CNAME` to `hosting.owlstown.com`.
**Do the DNS first and cancel Owlstown last.** Cancelling while DNS still points there leaves
`joshtycko.com` serving a dead page.

**Step 1 (needs your GoDaddy login).** In GoDaddy → My Products → `joshtycko.com` → DNS:

1. Turn **off** domain forwarding if the apex is forwarded (Domain Settings → Forwarding → delete).
2. Delete the existing apex `A` records, then add four `A` records, host `@`:
   `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   (optionally the matching `AAAA` records `2606:50c0:8000::153` through `8003::153`).
3. Edit the `www` `CNAME`: change its value from `hosting.owlstown.com` to `joshtycko.github.io`.
4. Leave the `MX` records alone. They point at `secureserver.net` and carry your mail.

Confirm it took with `dig +short joshtycko.com` (expect the four `185.199.*` addresses) and
`dig +short www.joshtycko.com` (expect `joshtycko.github.io`). Propagation is usually minutes.

**Step 2 (Claude can do this once step 1 resolves).**

1. Set the Pages custom domain: `PUT /repos/JoshTycko/joshtycko.github.io/pages` with
   `{"cname": "joshtycko.com", "https_enforced": true}`, which also writes the `CNAME` file.
2. Repoint `rel="canonical"` and the `og:url` tags in `index.html` and `publications.html`
   from `https://joshtycko.github.io/` to `https://joshtycko.com/`.
3. Wait for the certificate, then verify `https://joshtycko.com` and the
   `joshtycko.github.io` → `joshtycko.com` redirect.

Do **not** add a `CNAME` file before DNS resolves to GitHub. GitHub immediately starts
redirecting `joshtycko.github.io` to the custom domain, which would point visitors at a
domain still serving Owlstown.

**Step 3.** Once `https://joshtycko.com` serves this site, cancel the Owlstown subscription.
A snapshot of the Owlstown page and an audit of what was ported are in
`.claude/research/owlstown-archive/` (gitignored, local only).

`404.html` is picked up automatically by GitHub Pages. It uses absolute asset
paths (`/css/…`) because it is served at arbitrary URL depths — keep that if
you edit it.

## Maintenance

There is none. No packages, no framework, no build — just these files. The
fonts (Newsreader, Inter) are open-licensed (SIL OFL) and served from
`fonts/`, so the site makes no third-party requests and sets no cookies.
