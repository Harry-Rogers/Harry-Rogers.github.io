<!-- GSD:project-start source:PROJECT.md -->
## Project

**Harry Rogers — Academic Website**

A personal academic website for **Harry Rogers**, Postdoctoral Research Associate at the Institute of Biomedical Engineering, University of Oxford. The site is a single canonical place for anyone (peers, students, recruiters, journalists) who lands on it to learn who he is, see his published work and talks, find his teaching, download his CV, and contact him.

The site is built by forking the [academicpages](https://github.com/academicpages/academicpages.github.io) Jekyll template (fork-then-customise rather than build-from-scratch), deployed via **GitHub Pages** at `https://harry-rogers.github.io` from the repo `Harry-Rogers/Harry-Rogers.github.io`.

**Core Value:** > When someone Googles "Harry Rogers Oxford" and lands on this site, within ten seconds they can tell **who he is, what he works on, and how to reach him** — and within thirty seconds they can find his papers, talks, and CV.

Everything else is secondary.

### Constraints

- **Host:** GitHub Pages (free tier, Jekyll builds supported natively). Repo must be named exactly `Harry-Rogers.github.io` to deploy at `https://harry-rogers.github.io/`.
- **Template:** Forked from `academicpages/academicpages.github.io` (Jekyll + Minimal Mistakes). Stay close to the template to keep maintenance low; only deviate where it adds personal character.
- **Stack:** Jekyll (Ruby), Liquid templating, Sass, vanilla JS. No JS framework, no build tooling beyond what GitHub Pages runs.
- **Dark mode only.** No light-mode theme. The default academicpages light theme must be replaced.
- **No blog.** Strictly static pages. A *News* feed is allowed but limited to academic milestones (papers accepted, talks given, awards, joining/leaving institutions) — no opinion posts, no "thoughts" posts ("blogs feel cringy").
- **CV file:** Skipped for v1. The "Download CV" link is deferred — site should not include a broken link.
- **Maintenance model:** Owner edits Markdown files directly and commits to `main`; GitHub Pages auto-rebuilds. No CI required beyond the default Pages build.
<!-- GSD:project-end -->

<!-- GSD:stack-start source:research/STACK.md -->
## Technology Stack

## TL;DR
## Recommended Stack
### Core Technologies
| Technology | Version | Purpose | Why Recommended |
|---|---|---|---|
| **Jekyll** | **3.10.0** | Static site generator | Exact version GitHub Pages' default builder runs ([pages-gem versions](https://pages.github.com/versions/)). Pinning anything else (e.g. Jekyll 4.x) forces a custom GitHub Action — explicit non-goal in PROJECT.md ("no CI beyond default Pages build"). HIGH. |
| **Ruby** | **3.3.4** | Jekyll runtime | The Ruby that pages-gem is built against on GitHub's runners as of 2026. Pin with `.ruby-version` so `bundle exec jekyll serve` locally matches prod. HIGH. |
| **github-pages gem** | **latest from rubygems** (currently tracks Jekyll 3.10.0) | Meta-gem that pins Jekyll + all whitelisted plugins to the same versions Pages runs | Lets you reproduce the production build locally. Always `bundle update github-pages` rather than hand-pinning individual plugin versions. HIGH. |
| **Minimal Mistakes (vendored)** | as shipped with academicpages fork (currently 4.x lineage, vendored into `_sass/`, `_includes/`, `_layouts/`) | UI theme underneath academicpages | Already vendored into the academicpages fork — do **not** swap to remote_theme or upgrade independently. HIGH. |
| **academicpages template** | fork from `academicpages/academicpages.github.io` at fork date | Provides `_publications`, `_talks`, `_teaching`, `_portfolio` collections, `author_profile` sidebar, archive layouts | Decision is locked in PROJECT.md. HIGH. |
| **Kramdown + Rouge** | 2.4.0 / 3.30.0 | Markdown + syntax highlighting | Bundled in pages-gem; no action needed. HIGH. |
| **Liquid** | 4.0.4 | Template language | Bundled. HIGH. |
### Whitelisted Plugins (safe to enable in `_config.yml > plugins:`)
| Plugin | Version (pages-gem) | Purpose | Keep? |
|---|---|---|---|
| `jekyll-feed` | 0.17.0 | Generates `/feed.xml` (RSS for News) | **YES** — News section justifies it |
| `jekyll-sitemap` | 1.4.0 | Generates `/sitemap.xml` | **YES** — SEO for academic discoverability |
| `jekyll-seo-tag` | 2.8.0 | Emits OpenGraph, Twitter Card, canonical URLs from front matter | **YES** — add it; academicpages does not enable it by default but it's a free win |
| `jekyll-redirect-from` | 0.16.0 | Per-page `redirect_from:` aliases | **YES** — useful if you rename pages later |
| `jekyll-paginate` | 1.1.0 | Paginates News if it grows | **OPTIONAL** — News will be small (milestones only); only enable if it ever exceeds ~15 entries |
| `jemoji` | (bundled) | GitHub-style emoji in Markdown | **OPTIONAL** — keep if academicpages enables it; harmless |
| `jekyll-gist` | 1.5.0 | Embed GitHub gists | **NO** — not needed; remove from `_config.yml` if present |
| `jekyll-mentions` | 1.6.0 | @user → GitHub profile link | **NO** — not needed |
| `jekyll-avatar` | 0.8.0 | GitHub avatar URLs from username | **NO** — using a local photo |
### Development Tools
| Tool | Purpose | Notes |
|---|---|---|
| **Bundler** | Resolve Gemfile, lock versions | `bundle install` then `bundle exec jekyll serve` for local preview at http://127.0.0.1:4000 |
| **`.ruby-version` file** | Pin Ruby for local dev (and any rbenv/asdf user) | Single line: `3.3.4` |
| **`Gemfile.lock`** | Reproducible local builds | **Commit it.** Pages itself ignores it (it uses its own pinned manifest), but local dev parity matters. |
| **VS Code + "Liquid" + "Jekyll Snippets" extensions** | Editing | Optional but useful |
| **`html-proofer` (local only)** | Catches broken links / missing alt text before commit | Not on Pages — run manually pre-push: `bundle exec htmlproofer ./_site --disable-external` |
## Installation
# 1. Fork academicpages/academicpages.github.io via GitHub UI, then rename to
#    Harry-Rogers.github.io (the user-site repo name is load-bearing — it MUST
#    match the GitHub username, case-insensitive, to deploy at the apex URL).
# 2. Clone and pin versions
# 3. Install Ruby 3.3.4 if not present (rbenv/asdf/chruby — your choice)
#    Then install gems
# 4. Local dev loop
# → http://127.0.0.1:4000
# 5. Deploy: just `git push origin main`. GitHub Pages builds automatically.
## BibTeX / Publications — Recommended Approach
### Why not jekyll-scholar
- `jekyll-scholar` is **not on the GitHub Pages plugin whitelist** ([Jekyll Talk](https://talk.jekyllrb.com/t/github-pages-the-demo-site-uses-jekyll-4-and-a-third-party-plugin-both-of-which-are-currently-not-whitelisted-for-use-on-github-pages/7128), [Gemma Danks tutorial](https://open-research.gemmadanks.com/tutorials/how-to-use-jekyll-scholar-with-github-pages/)). Pages silently ignores it.
- The only way to use it is to **build via a custom GitHub Action** (`actions/jekyll-build-pages` + `actions/deploy-pages` with a Gemfile that includes the plugin). That contradicts PROJECT.md's "no CI required beyond the default Pages build."
- It also pulls in `bibtex-ruby` + `citeproc-ruby`, which slow the build and add a failure surface that doesn't pay for itself at ~8 publications.
### How to use the built-in approach
### Importing from a .bib
### BibTeX on the page (for citing the work)
## Dark-Mode-Only Theming
### Step 1 — set the skin
### Step 2 — the actual variables in `_sass/minimal-mistakes/skins/_dark.scss`
| Variable | Value |
|---|---|
| `$background-color` | `#252a34` |
| `$text-color` | `#eaeaea` |
| `$primary-color` | `#00adb5` (teal accent) |
| `$border-color` | `mix(#fff, $background-color, 20%)` |
| `$code-background-color` | `mix(#000, $background-color, 15%)` |
| `$code-background-color-dark` | `mix(#000, $background-color, 20%)` |
| `$form-background-color` | `mix(#000, $background-color, 15%)` |
| `$footer-background-color` | `mix(#000, $background-color, 30%)` |
| `$link-color` | `mix($primary-color, $text-color, 40%)` |
| `$link-color-hover` | `mix(#fff, $link-color, 25%)` |
| `$link-color-visited` | `mix(#000, $link-color, 25%)` |
| `$masthead-link-color` | `$text-color` |
| `$masthead-link-color-hover` | `mix(#000, $text-color, 20%)` |
| `$navicon-link-color-hover` | `mix(#000, $background-color, 30%)` |
### Step 3 — override the variables you don't like
# Front matter required so Jekyll processes it as Sass
### Step 4 — kill the light-mode UI leaks (CRITICAL)
## Liquid / Data File Conventions
| Content | Location | Layout | Listing page |
|---|---|---|---|
| Publications | `_publications/YYYY-MM-DD-slug.md` | `single` | `_pages/publications.md` (iterates `site.publications | group_by: "category"`) |
| Talks | `_talks/YYYY-MM-DD-slug.md` | `talk` | `_pages/talks.md` |
| Teaching | `_teaching/YYYY-slug.md` | `single` | `_pages/teaching.md` |
| Projects / portfolio | `_portfolio/slug.md` | `single` | `_pages/portfolio.md` |
| News | **NEW:** `_news/YYYY-MM-DD-slug.md` (you'll create this collection) | `single` | `_pages/news.md` |
## Google Scholar / ORCID Integration
### Why not iframes / live badges
- **Google Scholar actively blocks iframe embedding** (X-Frame-Options + bot detection). Even when a citation badge service like ImpactStory works, it widens layout unpredictably on mobile.
- **ORCID does allow iframes** but the embed widget is ~600px wide and doesn't reflow under 480px — breaks the responsive promise in PROJECT.md.
- Static text + icon stays semantically clean, accessible, and printer-friendly. MEDIUM-HIGH confidence.
### Optional: a manually-maintained citation count
## Mobile Responsiveness
## Alternatives Considered
| Recommended | Alternative | When the alternative is better |
|---|---|---|
| Default Pages builder, Jekyll 3.10 | `actions/jekyll-build-pages` workflow with Jekyll 4 | Only if you need a plugin that's not whitelisted (e.g. jekyll-scholar). Locked out by PROJECT.md. |
| `_publications/*.md` (academicpages built-in) | `jekyll-scholar` + `_bibliography/papers.bib` | If you wanted CSL-styled rendering across multiple citation styles or had 100+ papers. Overkill for 8. |
| `minimal_mistakes_skin: dark` + overrides | Build a custom skin file from scratch in `_sass/minimal-mistakes/skins/_harry.scss` | If you want a very distinct palette (e.g. solarized). Easy upgrade later if needed. |
| Static links to Scholar/ORCID | Embedded iframe widgets / serverless citation-count fetcher | Never — see above |
| `_news` collection | `_posts` (Jekyll blog) | Only if you wanted RSS+date archive UI for free. PROJECT.md rules out a blog. A custom collection sidesteps that framing. |
## What NOT to Use
| Avoid | Why | Use Instead |
|---|---|---|
| **Jekyll 4.x or 5.x** | Not on Pages' whitelist; forces a custom Action — explicit non-goal | Jekyll 3.10.0 (pages-gem) |
| **jekyll-scholar** | Not whitelisted; needs Action; over-engineered for 8 papers | `_publications/*.md` + `pubsFromBib.py` (local) |
| **`remote_theme: mmistakes/minimal-mistakes`** | academicpages has *vendored* the theme and customised `_includes/`, `_layouts/`, and the `_sass` partials. Switching to `remote_theme` silently overrides those customisations and breaks the `author_profile` sidebar, publication layouts, etc. | Keep MM vendored as the fork ships it |
| **`jekyll-admin`, `jekyll-compose`** | Not whitelisted | Edit Markdown directly (PROJECT.md says owner does this anyway) |
| **A light-mode toggle (e.g. `darkmode-js`, `theme-toggles`)** | PROJECT.md says dark-only; a toggle is dead UI weight and a maintenance surface | Hard-code `dark` skin |
| **Google Analytics / Plausible / Fathom** | Out of scope per PROJECT.md | Nothing |
| **Disqus, utterances** | Comments are out of scope | Nothing |
| **Algolia / Lunr replacement search** | academicpages' built-in search is sufficient and a v1 non-goal | Built-in search |
| **CSS-in-JS, Tailwind, PostCSS** | No build tooling allowed | Sass (already in Jekyll) |
| **MathJax/KaTeX globally** | Adds ~100KB to every page; not needed since there's no blog | Only enable per-page (`mathjax: true` in front matter) if a paper writeup needs equations |
## Version Compatibility
| Package | Pinned at | Notes |
|---|---|---|
| jekyll@3.10.0 | pages-gem | Will NOT work with Jekyll 4 plugins; do not add any |
| github-pages (latest) | rubygems | Use `bundle update github-pages` monthly to track Pages-side bumps |
| ruby@3.3.4 | `.ruby-version` | Locks local dev to the runner; bump only when [pages-gem versions](https://pages.github.com/versions/) updates |
| Minimal Mistakes (vendored in academicpages) | as forked | Do not `bundle update` it; do not switch to `remote_theme` |
| Rouge@3.30.0 | pages-gem | Default light highlight theme — override CSS for dark mode |
| Liquid@4.0.4 | pages-gem | Standard `{% raw %}`, `where_exp`, `group_by` all available |
## Sources
- [GitHub Pages dependency versions (pages-gem manifest)](https://pages.github.com/versions/) — Jekyll 3.10.0, Ruby 3.3.4, full whitelist verified — HIGH
- [academicpages.github.io upstream repo](https://github.com/academicpages/academicpages.github.io) — Gemfile and `_publications` structure verified — HIGH
- [academicpages `pubsFromBib.py`](https://github.com/academicpages/academicpages.github.io/blob/master/markdown_generator/pubsFromBib.py) — local-only BibTeX-to-Markdown generator — HIGH
- [Minimal Mistakes `_dark.scss`](https://github.com/mmistakes/minimal-mistakes/blob/master/_sass/minimal-mistakes/skins/_dark.scss) — exact variable names and defaults — HIGH
- [Minimal Mistakes stylesheets docs](https://mmistakes.github.io/minimal-mistakes/docs/stylesheets/) — override pattern (variables before `@import`) — HIGH
- [Minimal Mistakes configuration docs](https://mmistakes.github.io/minimal-mistakes/docs/configuration/) — `minimal_mistakes_skin` key — HIGH
- [Jekyll Talk: jekyll-scholar not whitelisted](https://talk.jekyllrb.com/t/github-pages-the-demo-site-uses-jekyll-4-and-a-third-party-plugin-both-of-which-are-currently-not-whitelisted-for-use-on-github-pages/7128) — MEDIUM (community thread, but consistent with pages-gem manifest)
- [Gemma Danks: jekyll-scholar via Actions tutorial](https://open-research.gemmadanks.com/tutorials/how-to-use-jekyll-scholar-with-github-pages/) — confirms the workaround we're rejecting — MEDIUM
- [inukshuk/jekyll-scholar](https://github.com/inukshuk/jekyll-scholar) — primary source on the plugin we're declining — HIGH
- [actions/jekyll-build-pages](https://github.com/actions/jekyll-build-pages) — the Action we're NOT using — HIGH
<!-- GSD:stack-end -->

<!-- GSD:conventions-start source:CONVENTIONS.md -->
## Conventions

Conventions not yet established. Will populate as patterns emerge during development.
<!-- GSD:conventions-end -->

<!-- GSD:architecture-start source:ARCHITECTURE.md -->
## Architecture

Architecture not yet mapped. Follow existing patterns found in the codebase.
<!-- GSD:architecture-end -->

<!-- GSD:skills-start source:skills/ -->
## Project Skills

No project skills found. Add skills to any of: `.claude/skills/`, `.agents/skills/`, `.cursor/skills/`, `.github/skills/`, or `.codex/skills/` with a `SKILL.md` index file.
<!-- GSD:skills-end -->

<!-- GSD:workflow-start source:GSD defaults -->
## GSD Workflow Enforcement

Before using Edit, Write, or other file-changing tools, start work through a GSD command so planning artifacts and execution context stay in sync.

Use these entry points:
- `/gsd-quick` for small fixes, doc updates, and ad-hoc tasks
- `/gsd-debug` for investigation and bug fixing
- `/gsd-execute-phase` for planned phase work

Do not make direct repo edits outside a GSD workflow unless the user explicitly asks to bypass it.
<!-- GSD:workflow-end -->



<!-- GSD:profile-start -->
## Developer Profile

> Profile not yet configured. Run `/gsd-profile-user` to generate your developer profile.
> This section is managed by `generate-claude-profile` -- do not edit manually.
<!-- GSD:profile-end -->
