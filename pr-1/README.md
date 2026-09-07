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
