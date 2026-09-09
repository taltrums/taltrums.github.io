# taltrums.github.io

Link-in-bio for **Mohd Talha** — `cpucrusher` (gaming) and `taltrums4real` (fitness, lifestyle, memes).

**Live:** https://taltrums.github.io

## Stack

None. One `index.html` — inline CSS, ~20 lines of JS, no build step, no dependencies.
Film grain and vignette are generated SVG, so the page ships without a single background image.

## Editing

| What | Where |
|---|---|
| The pinned card at top | `index.html` → search `ACTIVE`. Two alt versions sit commented beside it (going live / other account) |
| Links | `index.html` → the `LIFE` / `GAMING` / `WORK` sections |
| Bio | `index.html` → `ABOUT` section |
| Photos & video | drop into `img/` — see `img/README.txt` for filenames |
| Share preview | `og.html` is the 1200x630 source for `img/og.jpg` |
| Contact form | `index.html` → `CONTACT` section. Endpoint: the `action` on the `<form>` |

Swap the pinned card weekly. Paste the **specific** post URL, not the profile URL.

> **Filenames are case-sensitive in production.** GitHub Pages serves from Linux;
> your Mac does not care about case, so `lift.MP4` works locally and 404s live.
> Keep every asset extension lowercase.

## Making a change

`main` is protected — it only moves through a pull request.

```bash
git switch -c card/muscle-up-reel   # branch
# edit, then
git commit -am "swap pinned card to the muscle-up reel"
git push -u origin HEAD
gh pr create --fill
```

CI deploys the branch and comments a link on the PR:

| | URL |
|---|---|
| Production (`main`) | https://taltrums.github.io |
| Any open PR | `https://taltrums.github.io/pr-<number>/` |

Open the preview **on your phone** before merging — that is where this page
actually gets used. Merging publishes to production; closing the PR deletes
its preview directory.

Both live in `.github/workflows/`. They publish to the `gh-pages` branch with
plain `git` and no third-party actions, so nothing outside GitHub holds write
access to the site.

## Contact form

Mail goes to Proton via [FormSubmit](https://formsubmit.co). No backend, no
account, nothing to keep running.

**Already activated — no setup needed.** The `action` holds FormSubmit's
random token rather than the address, so the inbox can't be scraped out of the
page source. The token is a one-way pointer: it maps to the address on
FormSubmit's side and the address can't be read back from it.

Two things that will bite if you forget them:

- Activation is bound to **`taltrums.github.io`**. Serving the page from any
  other host means re-confirming there.
- FormSubmit refuses submissions from `file://` pages — opening `index.html`
  by double-clicking it will show *"Make sure you open this page through a web
  server"*. Use `python3 -m http.server` to test locally.

To send mail somewhere else you need a **new** token: submit once from a real
`https` page on the domain with the plain address in the `action`, confirm it,
and swap the new token in.

The form is a plain `POST`, so it works with JavaScript off — FormSubmit just
redirects back to `?sent=1`. With JS on, it submits through
`formsubmit.co/ajax/…` and never leaves the page, which matters because most
visitors are inside an Instagram webview. That path is verified working with the
token (their docs only document `/ajax/` for a bare address). If it ever fails
it falls back to the plain POST, so a message is never silently lost.

reCAPTCHA is on (FormSubmit's default). Tuning knobs are the `_subject`,
`_template`, and `_next` hidden inputs; `_next` is rewritten by the script to
the current origin so a PR preview returns to the preview.

## SEO

The useful work here isn't keywords, it's **entity resolution**. `index.html`
carries a JSON-LD `Person` whose `sameAs` lists all eight profiles, so a search
engine treats *Mohd Talha*, *taltrums4real*, *cpucrusher* and those accounts as
one identity instead of unrelated strings.

**If you add or remove a link, update `sameAs` in the JSON-LD to match.** The
`rel="me"` attributes in the body and `sameAs` in the head say the same thing
in two places, and they only help while they agree.

| File | Purpose |
|---|---|
| `robots.txt` | Allows everything except `/pr-` previews, which would otherwise be duplicate copies of the homepage. Points to the sitemap. |
| `sitemap.xml` | One URL. Mostly there so Search Console has something to fetch. |

The three stills are real `<img>` tags with `alt`, `width`/`height` and
`loading="lazy"` — a CSS `background-image` can't be indexed by image search or
carry alt text. Keep the `alt` text descriptive when you swap photos.

Not done, and deliberately: `<meta name="keywords">` is ignored by Google, and
`taltrums.github.io` can't be changed without buying a domain.

To verify after a change, paste the URL into Google's Rich Results Test and
Search Console → URL Inspection.

## Regenerating the share image

```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless --window-size=1200,630 --virtual-time-budget=4000 \
  --screenshot=/tmp/og.png "file://$PWD/og.html"
sips -s format jpeg -s formatOptions 82 /tmp/og.png --out img/og.jpg
```

## Notes

- Video is `preload="none"` and tap-to-play on phones — nothing downloads on cellular unasked
- No `target="_blank"`: same-tab navigation is more reliable inside Instagram/Snapchat webviews
- Respects `prefers-reduced-motion`

Built by [OddlyBuilt](https://github.com/OddlyBuilt).
