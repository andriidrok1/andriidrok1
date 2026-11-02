<div align="center">

<img src="./ascii.svg" width="460" alt="Andrii Drok"/>

<img src="./stats.svg" width="620" alt="Contributions in the last year"/>

[andriidrok.com](https://andriidrok.com) &nbsp;·&nbsp;
[instagram](https://www.instagram.com/andrii_drok/) &nbsp;·&nbsp;
[linkedin](https://www.linkedin.com/in/andriidrok/) &nbsp;·&nbsp;
[email](mailto:clb@mirasvit.com)

</div>

<img src="./hd-about.svg" width="620" alt="about"/>

> Andrii Drok. CS student at San Francisco State University, San Francisco Bay Area.<br>
> Small, sharp tools over big vague ideas.

I build fast, test on real users, and kill what doesn't work. Right now that's<br>
[autobroll](https://github.com/andriidrok1/autobroll) — an AI short-form video editor that runs in the browser. Also<br>
deep into markets: Pine Script indicators, backtesting, on-chain data.

<img src="./hd-stack.svg" width="620" alt="stack"/>

<samp>python &nbsp; typescript &nbsp; javascript &nbsp; react &nbsp; node &nbsp; three.js &nbsp; fastapi &nbsp; postgres &nbsp; docker &nbsp; git &nbsp; linux</samp>

<img src="./hd-projects.svg" width="620" alt="projects"/>

**[autobroll](https://github.com/andriidrok1/autobroll)** &nbsp;·&nbsp; <samp>typescript, remotion</samp><br>
AI short-form video editor in the browser. Auto captions with accents,<br>
drag-and-retime editing, b-roll placement: transcript in, rendered video out.

**[strategy-checker](https://github.com/andriidrok1/strategy-checker)** &nbsp;·&nbsp; <samp>python</samp><br>
Describe a trading strategy in plain English, get a real backtest with<br>
statistical validation. Exposes curve-fitting, not alpha.

**[compound](https://github.com/andriidrok1/compound)** &nbsp;·&nbsp; <samp>typescript, convex</samp><br>
Autonomous research agent for your second brain. Built solo at Nozomio<br>
Hackathon, EF SF.

**[andriidrok.com](https://andriidrok.com)** &nbsp;·&nbsp; <samp>three.js, webgl</samp><br>
Particle-morph portfolio: thousands of particles reshaping between scenes.

<img src="./hd-stats.svg" width="620" alt="stats"/>

<div align="center">

<img src="./streak.svg" width="620" alt="Current and longest streak"/>

<img src="./langs.svg" width="620" alt="Top languages by bytes and by repo"/>

<img src="./year.svg" width="620" alt="The last year, one character per day"/>

</div>

<img src="./hd-about-this-page.svg" width="620" alt="about this page"/>

Every graphic here is generated, not embedded from anyone else's server.<br>
`ascii.svg` is a photo pushed through a character ramp by<br>
[`scripts/make_portrait.py`](scripts/make_portrait.py); the stat graphics and<br>
these section headings are drawn by [a scheduled action](.github/workflows/stats.yml)<br>
straight from the GitHub GraphQL API, once a day, committing only what changed.

They animate with SMIL inside the SVG, because GitHub strips scripts from<br>
READMEs — and since nothing loads from a third party, nothing here can<br>
rate-limit or go dark. The headings are SVGs for the same reason: GitHub also<br>
strips CSS, so an image is the only way to put this page's own typeface on them.

The typeface is [JetBrains Mono](scripts/fonts), subset to just the characters<br>
each graphic draws and inlined as base64. That isn't only for looks: the<br>
portrait's grid assumes an advance width of exactly 0.600 em, and a viewer whose<br>
default monospace is narrower would otherwise see it squeezed.

Language totals cover public repositories only. `year.svg` uses the portrait's<br>
character ramp: `:` `+` `#` `@`, quiet to loud.

The full write-up, with the gotchas and how to reuse it, is in<br>
[how-this-page-is-built.md](how-this-page-is-built.md).
