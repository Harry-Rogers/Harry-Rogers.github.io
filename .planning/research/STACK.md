# Stack Research

**Domain:** Academic personal website on GitHub Pages, forked from academicpages (Jekyll + Minimal Mistakes)
**Researched:** 2026-05-17
**Confidence:** HIGH for core stack (verified against pages-gem manifest); MEDIUM on a few stylistic recommendations

## TL;DR

Stay on the **default GitHub Pages builder** (no GitHub Action). Pin to the exact versions GitHub Pages ships — Jekyll **3.10.0** on Ruby **3.3.4** — via `Gemfile` + `.ruby-version`. Keep BibTeX out of the live build: use the **academicpages `_publications/*.md` collection** (one file per paper, citation string in front matter) and run the project's `markdown_generator/pubsFromBib.py` locally when importing from a `.bib`. Do **not** introduce `jekyll-scholar` — it forces a custom GitHub Action and the rest of academicpages does not need it. For dark-mode-only, set `minimal_mistakes_skin: "dark"` in `_config.yml`, override a small set of Sass variables in `assets/css/main.scss`, and **delete the skin/theme toggle markup** from `_includes` so no light-mode UI ever ships. Scholar/ORCID surface as **text links with SVG icons in `author_profile`** — never iframes (they break responsive layout and Scholar blocks embeds).

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

These are the ones academicpages already uses or you'll likely want. All run on the default GitHub Pages builder — **no Action required**.

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

```bash
# 1. Fork academicpages/academicpages.github.io via GitHub UI, then rename to
#    Harry-Rogers.github.io (the user-site repo name is load-bearing — it MUST
#    match the GitHub username, case-insensitive, to deploy at the apex URL).

# 2. Clone and pin versions
git clone https://github.com/Harry-Rogers/Harry-Rogers.github.io.git
cd Harry-Rogers.github.io
echo "3.3.4" > .ruby-version

# 3. Install Ruby 3.3.4 if not present (rbenv/asdf/chruby — your choice)
#    Then install gems
bundle install

# 4. Local dev loop
bundle exec jekyll serve --livereload
# → http://127.0.0.1:4000

# 5. Deploy: just `git push origin main`. GitHub Pages builds automatically.
```

The `Gemfile` already shipped by academicpages is fine; it pulls `github-pages` and lets pages-gem resolve transitive versions. Do **not** add Jekyll 4 or jekyll-scholar to it.

## BibTeX / Publications — Recommended Approach

**Decision: use academicpages' built-in `_publications/*.md` collection. Do not adopt jekyll-scholar.**

### Why not jekyll-scholar

- `jekyll-scholar` is **not on the GitHub Pages plugin whitelist** ([Jekyll Talk](https://talk.jekyllrb.com/t/github-pages-the-demo-site-uses-jekyll-4-and-a-third-party-plugin-both-of-which-are-currently-not-whitelisted-for-use-on-github-pages/7128), [Gemma Danks tutorial](https://open-research.gemmadanks.com/tutorials/how-to-use-jekyll-scholar-with-github-pages/)). Pages silently ignores it.
- The only way to use it is to **build via a custom GitHub Action** (`actions/jekyll-build-pages` + `actions/deploy-pages` with a Gemfile that includes the plugin). That contradicts PROJECT.md's "no CI required beyond the default Pages build."
- It also pulls in `bibtex-ruby` + `citeproc-ruby`, which slow the build and add a failure surface that doesn't pay for itself at ~8 publications.

### How to use the built-in approach

Each publication is one Markdown file in `_publications/` with this front matter (verified against the upstream template's `2024-02-17-paper-title-number-4.md`):

```yaml
---
title: "An automated precision spraying evaluation system"
collection: publications
category: conferences        # or 'manuscripts' for journals
permalink: /publication/2023-taros-precision-spraying
excerpt: 'One-sentence summary for the listing.'
date: 2023-09-13
venue: 'TAROS 2023'
paperurl: 'https://...pdf'   # optional — link to PDF
citation: 'Rogers, H., et al. (2023). "An automated precision spraying evaluation system." <i>TAROS</i>.'
---

Optional longer body shown only on the per-paper page.
```

Listings are produced by `_pages/publications.md` which iterates `site.publications` grouped by `category`. The `citation` field is rendered verbatim as the formatted reference.

### Importing from a .bib

academicpages ships **`markdown_generator/pubsFromBib.py`** ([source](https://github.com/academicpages/academicpages.github.io/blob/master/markdown_generator/pubsFromBib.py)) — a one-shot Python script. Workflow:

1. Export Harry's publications from Google Scholar / ORCID as `publications.bib`.
2. Drop it in `markdown_generator/`.
3. Run the script — it produces one `.md` per entry under `_publications/`.
4. Hand-edit the generated files to tidy citation strings (Scholar BibTeX is notoriously messy on capitalisation and venue names).

This is a **build-time-free** approach: the script runs **on Harry's laptop, not on GitHub's runner.** The site only ever sees plain Markdown. HIGH confidence.

### BibTeX on the page (for citing the work)

Render a per-paper BibTeX block by adding a `bibtex:` multi-line string field to the front matter and printing it inside a `<pre><code>` on the publication's single-page layout. Or — simpler — link out to an ORCID/Scholar export. Recommendation: **embed BibTeX in front matter** for the v1 papers; it's ~10 lines per paper and saves the visitor a click.

## Dark-Mode-Only Theming

**Decision: skin = `dark`, override Sass variables in `assets/css/main.scss`, strip the toggle UI.**

### Step 1 — set the skin

In `_config.yml`:

```yaml
minimal_mistakes_skin: "dark"
```

Minimal Mistakes ships 10 skins; `dark` and `neon` are the two dark variants ([MM docs](https://mmistakes.github.io/minimal-mistakes/docs/configuration/)). `dark` is the right base (warmer, more readable for academic prose); `neon` is more aggressive and worse for long reading passages.

### Step 2 — the actual variables in `_sass/minimal-mistakes/skins/_dark.scss`

Verified from upstream ([_dark.scss source](https://github.com/mmistakes/minimal-mistakes/blob/master/_sass/minimal-mistakes/skins/_dark.scss)):

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

Create or edit `assets/css/main.scss`. The pattern (verified from MM stylesheets docs) is **variables BEFORE imports**:

```scss
---
# Front matter required so Jekyll processes it as Sass
---

/* Override before the skin loads */
$primary-color: #c084fc;          /* swap teal for something more "Harry" */
$background-color: #1a1d24;       /* slightly deeper for less blue cast */
$link-color: mix($primary-color, $text-color, 50%);

@import "minimal-mistakes/skins/dark";
@import "minimal-mistakes";
```

The two `@import` lines are required and in that exact order — the skin must load **after** your overrides and **before** the main partials, otherwise either (a) your colors get stomped or (b) the skin doesn't propagate into component rules.

### Step 4 — kill the light-mode UI leaks (CRITICAL)

academicpages does **not** ship a runtime theme toggle, so there's no JS toggle to remove — but you must still:

1. **Set `<html data-theme="dark">` or just remove any `prefers-color-scheme` media branches** if you find them in `_includes/head/custom.html`. Minimal Mistakes' `dark` skin is unconditional, so this is mostly belt-and-braces.
2. **Check `_includes/masthead.html` and `_includes/footer.html`** for any hard-coded light backgrounds (the academicpages variant sometimes has inline-styled logos that assume light bg).
3. **Override `$code-background-color`** explicitly if you keep code blocks — Rouge's default highlight theme (`rouge.css`) is light. Either swap to a dark Rouge theme (`bundle exec rougify style monokai > assets/css/rouge.scss`) or recolour via Sass.
4. **The author-profile avatar border / social icons** inherit light-mode tints in some forks — confirm at first deploy.
5. **Search results overlay** (academicpages ships a basic JS search) renders into a div with white-ish defaults — restyle `.search-content` and `.search-input` if used.

MEDIUM confidence on items 2-5: they're empirically common leak points across academicpages forks; verify against the actual fork state at scaffold time.

## Liquid / Data File Conventions

Academicpages uses **per-item Markdown files in collections** rather than a single YAML data file. Stick with this — it's what every academicpages contributor expects, and it gives each item its own permalink for free.

| Content | Location | Layout | Listing page |
|---|---|---|---|
| Publications | `_publications/YYYY-MM-DD-slug.md` | `single` | `_pages/publications.md` (iterates `site.publications | group_by: "category"`) |
| Talks | `_talks/YYYY-MM-DD-slug.md` | `talk` | `_pages/talks.md` |
| Teaching | `_teaching/YYYY-slug.md` | `single` | `_pages/teaching.md` |
| Projects / portfolio | `_portfolio/slug.md` | `single` | `_pages/portfolio.md` |
| News | **NEW:** `_news/YYYY-MM-DD-slug.md` (you'll create this collection) | `single` | `_pages/news.md` |

For **News**, register a new collection in `_config.yml`:

```yaml
collections:
  news:
    output: true
    permalink: /news/:path/
defaults:
  - scope: { path: "", type: "news" }
    values:
      layout: single
      author_profile: true
      share: false
      comments: false
```

Each News entry is a tiny Markdown file with `title`, `date`, `category` (e.g. `paper`, `talk`, `award`) — kept short by the "milestones only" rule.

For the **landing page contact rail** (email, Oxford, ORCID, Scholar, GitHub, LinkedIn), use the MM **`author:` block in `_config.yml`** under `author.links`. Each link has `label`, `icon` (FontAwesome class string), and `url`. academicpages already wires `_includes/author-profile-custom-links.html` to render them in the sidebar.

## Google Scholar / ORCID Integration

**Decision: text links + FontAwesome SVG icons in `_config.yml > author.links`. No iframes. No live citation badges.**

```yaml
author:
  links:
    - label: "Email"
      icon: "fas fa-fw fa-envelope"
      url: "mailto:Harry.Rogers@eng.ox.ac.uk"
    - label: "ORCID"
      icon: "fab fa-fw fa-orcid"
      url: "https://orcid.org/0000-0003-3227-5677"
    - label: "Google Scholar"
      icon: "fas fa-fw fa-graduation-cap"
      url: "https://scholar.google.com/citations?user=sPwcwvMAAAAJ"
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/Harry-Rogers"
    - label: "LinkedIn"
      icon: "fab fa-fw fa-linkedin"
      url: "https://www.linkedin.com/in/harry-rogers-ox"
```

### Why not iframes / live badges

- **Google Scholar actively blocks iframe embedding** (X-Frame-Options + bot detection). Even when a citation badge service like ImpactStory works, it widens layout unpredictably on mobile.
- **ORCID does allow iframes** but the embed widget is ~600px wide and doesn't reflow under 480px — breaks the responsive promise in PROJECT.md.
- Static text + icon stays semantically clean, accessible, and printer-friendly. MEDIUM-HIGH confidence.

### Optional: a manually-maintained citation count

The "Scholar profile (37 citations, h-index 3)" data in PROJECT.md is fine to surface as plain text on the About page — Harry edits it when he updates publications. No auto-fetch (would require an Action and Scholar has no stable API).

## Mobile Responsiveness

Minimal Mistakes is already responsive — it uses a 4-breakpoint Sass system (`$small: 600px; $medium: 768px; $medium-wide: 900px; $large: 1024px; $x-large: 1280px;` in `_variables.scss`). The `author_profile` sidebar collapses below `$large`. Three things to verify in v1:

1. **Test at 360px (iPhone SE) and 768px (iPad portrait).** Use Firefox responsive mode; do not just resize the window — that misses the iOS rubber-banding font-scaling.
2. **Long publication titles wrap, not overflow.** academicpages' listing has been known to push the citation block off-screen at narrow widths; `overflow-wrap: break-word` on `.archive__item-excerpt` is the fix.
3. **The masthead nav.** With ~6 pages (Home, Publications, Talks, Teaching, Projects, News, CV-later, Contact) the desktop masthead is already crowded; below `$medium` MM auto-collapses to a hamburger — confirm the hamburger icon is visible against the dark masthead (`$masthead-link-color` defaults to `$text-color` which is `#eaeaea` against `mix(#000, $background-color, 30%)` — fine but verify).

HIGH confidence on (1) and (2); MEDIUM on (3) — depends on final color overrides.

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

---
*Stack research for: academicpages-based personal site on default GitHub Pages*
*Researched: 2026-05-17*
