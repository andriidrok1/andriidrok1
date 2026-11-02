# How this page is built

Self-hosted GitHub profile stats: no third-party cards, no scripts, no
external requests. The page this describes is
[github.com/andriidrok1](https://github.com/andriidrok1).

Every graphic on the profile is generated inside this repository and committed as a
plain SVG. Nothing loads from anyone else's server, so nothing can rate-limit,
go dark, or watch who views the page. This is the write-up, for anyone
who wants the same without a `github-readme-stats` style service in between.

## What GitHub lets a README do

A profile README is Markdown rendered by GitHub, and GitHub strips three
things that most "animated profile" tricks depend on:

* `<script>` is removed, so no JavaScript.
* `<style>` and inline `style=` are removed, so no CSS and no web fonts.
* SVGs are shown through `<img>`, and a browser will not fetch subresources
  for an image document: no external fonts, no external images inside the SVG.

What survives: an SVG can carry its own `<style>` block, its own
[SMIL](https://developer.mozilla.org/en-US/docs/Web/SVG/Guides/SVG_animation_with_SMIL)
`<animate>` elements, and a font inlined as a base64 data URI. That is the
whole toolbox used here.

## The pipeline

1. A [scheduled GitHub Action](.github/workflows/stats.yml) runs once a day
   (`cron: "17 5 * * *"`) and on `workflow_dispatch`, with the default
   `GITHUB_TOKEN`. No personal access token is stored anywhere.
2. [`scripts/generate_stats.py`](scripts/generate_stats.py) asks the
   [GitHub GraphQL API](https://docs.github.com/en/graphql) for
   `contributionsCollection` (the calendar and totals) and the owner's public
   repositories with their languages, then draws `stats.svg`, `streak.svg`,
   `langs.svg`, `year.svg` and the `hd-*.svg` section headings.
3. The job commits only if a file actually changed, as `github-actions[bot]`.

```yaml
on:
  schedule:
    - cron: "17 5 * * *"
  workflow_dispatch:
permissions:
  contents: write
jobs:
  refresh:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5
      - run: python3 scripts/generate_stats.py
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          GH_LOGIN: ${{ github.repository_owner }}
      - run: |
          if [ -z "$(git status --porcelain -- '*.svg')" ]; then exit 0; fi
          git config user.name  "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add '*.svg' && git commit -m "stats: refresh profile graphics" && git push
```

There is deliberately no `push` trigger. The job commits to the repository,
and a push trigger would make it re-run on its own commit.

## Making the output deterministic

The first version committed noise every night. Two things had to be pinned:

* **The contribution window, to whole UTC days.** GitHub's calendar is bucketed
  by UTC day. Asking for "the last 365 days from now" moves the window by a
  few hours each run, days slide between week buckets, and the sparkline shifts
  a fraction of a pixel. Floor `from` and `to` to midnight UTC and the SVG is
  byte-identical until the data changes.
* **`privacy: PUBLIC` on the repositories query.** A personal token sees
  private repositories, the workflow token does not, so language totals
  disagreed between a local run and the Action. Pinning the query to public
  repositories makes both agree, which is also why the language chart says
  "public repositories only".

## Animation without JavaScript

Each graphic reveals itself with a `<clipPath>` whose width is animated from
zero, and a cursor block that rides the clip edge, staggered top to bottom and
frozen at the end (`fill="freeze"`) so it prints once and stops:

```xml
<clipPath id="c1"><rect x="0" y="0" height="18" width="0">
  <animate attributeName="width" from="0" to="460" begin="0.3s" dur="0.6s" fill="freeze"/>
</rect></clipPath>
<text clip-path="url(#c1)" class="d-f">...</text>
<rect x="0" y="0" width="2" height="18" opacity="0.55">
  <animate attributeName="x" from="0" to="460" begin="0.3s" dur="0.6s" fill="freeze"/>
</rect>
```

SMIL is the only animation GitHub leaves alone, and it works in every browser
GitHub supports.

## Fonts: subset and inline, for the metrics as much as the look

The typeface is [JetBrains Mono](https://github.com/JetBrains/JetBrainsMono),
subset to just the characters each graphic draws (the 13 ramp characters for
the portrait, the heading letters, basic Latin for the stat graphics) and
inlined as a base64 `@font-face` inside each SVG. The subsets live in
[`scripts/fonts`](scripts/fonts) under the SIL Open Font License.

Looks are half the reason. The other half is geometry: the ASCII portrait's
grid assumes an advance width of exactly 0.600 em, which JetBrains Mono has
(600/1000 units). A viewer whose default monospace is narrower (Consolas is
about 0.55) would otherwise see the portrait about 7% too narrow. Inlining the
face pins the width for everyone.

## Section headings as SVG

The `about`, `stack`, `projects` and `stats` headings are images for the same
reason: Markdown headings cannot use this page's typeface, since GitHub strips
CSS. `generate_stats.py` draws them with a hairline running to the right edge,
and sizes the line to the widest plausible monospace advance so it never
overlaps the word.

## The ASCII portrait

`ascii.svg` is a photo pushed through a 13-step character ramp by
[`scripts/make_portrait.py`](scripts/make_portrait.py):
background removal with `rembg`, a crop, a darkening curve so brows and
glasses do not dissolve, then one `<text>` row per line with the same
clip-path wipe as the stat graphics. The ramp, bright and sparse to dark and
dense, with a leading space for blank:

```
 .`:-=+*cs#%@
```

It is generated once, not on a schedule. Side light at about 45° and a tight
crop matter more than any parameter. `year.svg` reuses four steps of the same
ramp, `:` `+` `#` `@`, quiet to loud, for the contribution calendar.

## Reuse

Fork the repository, rename it to your login, and run the `refresh stats`
workflow once from the Actions tab. `GH_LOGIN` is taken from the repository
owner, and the default token is enough for public data. Replace `ascii.svg`
with your own portrait via `make_portrait.py`, then
`embed_portrait_font.py` to inline the font. Language totals cover public
repositories only.
