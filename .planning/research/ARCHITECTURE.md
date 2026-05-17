# Architecture Research

**Domain:** Jekyll academic personal site (fork of `academicpages/academicpages.github.io`, served by GitHub Pages)
**Researched:** 2026-05-17
**Confidence:** HIGH (verified directly against the upstream repo on GitHub)

## TL;DR

academicpages is a Jekyll site built on the **Minimal Mistakes** theme, customised for academic content. It defines four content collections (`publications`, `talks`, `teaching`, `portfolio`), a folder of standalone pages (`_pages/`), site-wide data (`_data/`), and a layered Sass tree that already ships **six themes in light/dark pairs** — including `default_dark`. Forking, editing on `main`, and letting GitHub Pages auto-build is the canonical workflow; no Actions, no Ruby, no local server required for the owner's stated edit-and-commit model.

## Standard Architecture

### System Overview

```
┌────────────────────────────────────────────────────────────────────┐
│                          Source (git repo)                          │
│                                                                     │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────────┐  │
│  │   CONFIG     │  │   CONTENT    │  │         THEME             │  │
│  │ _config.yml  │  │ _publications│  │ _layouts/  _includes/    │  │
│  │ Gemfile      │  │ _talks/      │  │ _sass/     assets/       │  │
│  │ _data/*.yml  │  │ _teaching/   │  │   theme/_default_dark    │  │
│  │  authors     │  │ _portfolio/  │  │   layout/, include/      │  │
│  │  navigation  │  │ _posts/      │  │ images/                  │  │
│  │  ui-text     │  │ _pages/      │  │                          │  │
│  └──────┬───────┘  └──────┬───────┘  └─────────────┬────────────┘  │
│         │                  │                        │                │
│         └──────────────────┼────────────────────────┘                │
│                            ▼                                         │
│             git push origin main                                     │
└────────────────────────────┬───────────────────────────────────────┘
                             ▼
┌────────────────────────────────────────────────────────────────────┐
│            GitHub Pages auto-build (pages-build-deployment)         │
│                                                                     │
│   Jekyll 3.x + safe-mode plugins ──▶  _site/  ──▶  CDN              │
│   (jekyll-feed, sitemap, paginate, redirect-from, gist, jemoji)     │
└────────────────────────────┬───────────────────────────────────────┘
                             ▼
                  https://harry-rogers.github.io/
```

### Component Responsibilities

| Component | Responsibility | Where it lives |
|-----------|---------------|----------------|
| **Site config** | URL, title, owner identity, collection definitions, plugin list, theme choice | `_config.yml` |
| **Site-wide data** | Nav menu, author profile, UI text strings, CV (JSON variant) | `_data/navigation.yml`, `_data/authors.yml`, `_data/ui-text.yml`, `_data/cv.json` |
| **Content (collections)** | One Markdown file per publication / talk / teaching item / portfolio project | `_publications/`, `_talks/`, `_teaching/`, `_portfolio/` |
| **Content (pages)** | Landing page, papers index, CV page, archives | `_pages/about.md`, `publications.html`, `talks.html`, `teaching.html`, `cv.md`, `404.md`, etc. |
| **Layouts** | Page-shape templates: `default`, `single`, `archive`, `talk`, `cv-layout`, `splash`, `archive-taxonomy`, `compress` | `_layouts/*.html` |
| **Includes** | Reusable HTML partials: `head.html`, `masthead.html`, `footer.html`, `author-profile.html`, `seo.html`, `archive-single.html`, `archive-single-talk.html`, `scripts.html`, `paginator.html`, plus provider sub-folders for analytics/comments/footer/head | `_includes/*.html` |
| **Sass / theming** | Layered SCSS: `_themes.scss` selects a theme; `theme/_default_dark.scss` + light/dark pairs for six built-in themes; `layout/`, `include/`, `vendor/` partials handle structure | `_sass/`, `assets/css/main.scss` |
| **Static assets** | Profile photo, paper PDFs, slide decks, BibTeX | `images/`, `files/`, `assets/` |
| **Build** | Jekyll, executed by GitHub Pages servers | (no source — GitHub runs it) |

## Directory Tree (top 2 levels)

```
academicpages.github.io/
├── _config.yml              ← CONFIG: site title, url, collections, defaults, plugins, theme
├── _config_docker.yml       ← (ignore — only for local Docker dev)
├── Gemfile                  ← Ruby deps (only needed if running Jekyll locally)
├── README.md
├── _data/                   ← CONFIG (data)
│   ├── authors.yml          ← author profile shown in sidebar
│   ├── navigation.yml       ← top-bar menu items
│   ├── ui-text.yml          ← all user-facing strings (i18n)
│   └── cv.json              ← optional JSON-driven CV
├── _pages/                  ← CONTENT (standalone pages)
│   ├── about.md             ← landing page
│   ├── publications.html    ← iterates over site.publications
│   ├── talks.html
│   ├── teaching.html
│   ├── portfolio.html
│   ├── cv.md
│   ├── 404.md
│   ├── year-archive.html, category-archive.html, tag-archive.html, ...
├── _publications/           ← CONTENT (collection)
│   └── YYYY-MM-DD-slug.md   ← one file per paper
├── _talks/                  ← CONTENT (collection)
│   └── YYYY-MM-DD-slug.md   ← one file per talk
├── _teaching/               ← CONTENT (collection)
│   └── YYYY-term-slug.md
├── _portfolio/              ← CONTENT (collection)
│   └── portfolio-N.md
├── _posts/                  ← CONTENT (blog — NOT USED in this project; can be emptied)
├── _drafts/                 ← (unpublished posts — ignorable)
├── _layouts/                ← THEME (page shells)
│   ├── default.html, single.html, archive.html, talk.html, cv-layout.html,
│   │   splash.html, archive-taxonomy.html, compress.html
├── _includes/               ← THEME (partials)
│   ├── head.html, masthead.html, footer.html, sidebar.html,
│   ├── author-profile.html, archive-single.html, archive-single-talk.html,
│   ├── seo.html, scripts.html, analytics.html, comments.html, paginator.html,
│   └── (subdirs: analytics/, comments/, footer/, head/)
├── _sass/                   ← THEME (Sass tree)
│   ├── _themes.scss         ← @imports the chosen theme variables
│   ├── _syntax.scss         ← code highlighting
│   ├── theme/               ← LIGHT/DARK COLOR PALETTES
│   │   ├── _default_dark.scss   ← ★ what we want (or fork into _harry_dark)
│   │   ├── _default_light.scss
│   │   ├── _air_dark.scss / _air_light.scss
│   │   ├── _contrast_dark.scss / _contrast_light.scss
│   │   ├── _dirt_dark.scss / _dirt_light.scss
│   │   ├── _mint_dark.scss / _mint_light.scss
│   │   └── _sunrise_dark.scss / _sunrise_light.scss
│   ├── layout/, include/, vendor/   ← structural partials, never theme colors
├── assets/                  ← compiled CSS, JS, fonts
├── images/                  ← profile photo, paper figures
├── files/                   ← PDFs, slide decks, .bib files
├── markdown_generator/      ← (optional) Python/Jupyter helpers to bulk-generate _publications/*.md from a CSV or .bib
├── talkmap/, talkmap.py     ← (optional) map of talk locations
├── scripts/                 ← misc utility scripts
├── .github/                 ← workflows (auto-build badge only; nothing to edit)
└── Dockerfile, docker-compose.yaml, .devcontainer/   ← (optional) local dev container
```

## Data Flow: How a Publication Becomes a Rendered Page

End-to-end trace for the canonical case.

```
1.  Author writes  _publications/2024-precision-agriculture.md
    ───────────────────────────────────────────────────────────
    ---
    title:    "Advancing precision agriculture..."
    collection: publications
    category: manuscripts
    permalink: /publication/2024-precision-agriculture
    excerpt:  'Domain-specific augmentations and robustness testing.'
    date:     2024-03-15
    venue:    'Neural Computing and Applications'
    paperurl: 'https://...'
    bibtexurl: '/files/rogers-2024.bib'
    citation: 'Rogers, H., et al. (2024). "..." <i>NCA</i>.'
    ---
    Optional markdown body for the per-publication page.

2.  git push origin main

3.  GitHub Pages runs `jekyll build`.
    _config.yml defines:
        collections:
          publications:
            output: true
            permalink: /:collection/:path/
        defaults:
          - scope: { path: "", type: publications }
            values: { layout: single, author_profile: true, ... }

4.  Jekyll:
      • Reads every file under _publications/ → exposes site.publications
      • For each item: renders _layouts/single.html
          which inherits _layouts/default.html
          which pulls in _includes/head.html, masthead.html,
                          author-profile.html, footer.html, scripts.html
      • Output: _site/publication/2024-precision-agriculture/index.html

5.  Meanwhile _pages/publications.html runs at build time:
      {% for post in site.publications reversed %}
        {% include archive-single.html %}
      {% endfor %}
    → produces _site/publications/index.html (the index list)
    archive-single.html reads front-matter (title, venue, date, citation,
    paperurl, bibtexurl, slidesurl) and renders a card with links.

6.  Both pages are served from the CDN at:
      https://harry-rogers.github.io/publications/                   (list)
      https://harry-rogers.github.io/publication/2024-precision-agriculture/  (detail)
```

The same shape applies to `_talks/` (rendered by `_layouts/talk.html` via `_includes/archive-single-talk.html`), `_teaching/`, and `_portfolio/`.

## Where Dark Mode Slots In

academicpages **already ships dark themes**. There is no custom-CSS task here — only a config switch and (optional) palette tweaks.

```
_config.yml
└── teaser:        (irrelevant)
└── minimal_mistakes_skin: "default"   ← change this OR
                                          point _sass/_themes.scss at _default_dark
_sass/_themes.scss
└── @import "theme/default";  →  @import "theme/default_dark";
```

Two viable approaches:

| Approach | Effort | Risk |
|----------|--------|------|
| **A. Swap import in `_themes.scss`** to one of the six bundled `*_dark` palettes. | 1 line. | None — uses an officially supported palette. |
| **B. Fork `_default_dark.scss` → `_harry_dark.scss`**, tweak accent colour for personal differentiator (PROJECT.md requirement), import that instead. | ~30 min. | Low — just SCSS variable overrides. |

Recommended: **B**, because PROJECT.md explicitly wants "at least one small visual/structural touch". Variable overrides in a forked palette are the smallest such touch.

The structural Sass (`_sass/layout/`, `_sass/include/`) does **not** need editing for dark mode — palettes alone carry the colour tokens.

## GitHub Pages Deployment Path

Vanilla academicpages uses **GitHub Pages' built-in Jekyll build** — no custom Action, no `Gemfile.lock` you have to maintain.

```
Repo settings → Pages → Source: "Deploy from a branch" → main / (root)
```

Steps in practice:
1. Create the repo as `Harry-Rogers/Harry-Rogers.github.io` (user-site naming rule).
2. Fork / "Use this template" from `academicpages/academicpages.github.io` into that repo.
3. Edit `_config.yml` — set `url:`, `repository:`, `title:`, `name:`, `description:`.
4. Push to `main`. GitHub fires the `pages-build-deployment` workflow automatically.
5. First build: 1–5 minutes. Subsequent builds on each commit: ~30–60 s.

**No GitHub Actions YAML to write.** The badge in academicpages' README links to a workflow GitHub auto-provisions when "Deploy from a branch" is selected. The Jekyll plugins academicpages uses (`jekyll-feed`, `jekyll-sitemap`, `jekyll-paginate`, `jekyll-redirect-from`, `jekyll-gist`, `jemoji`) are all on the GitHub Pages plugin whitelist, so safe-mode builds work without modification.

If a future need arises that GitHub Pages safe-mode can't satisfy (e.g. a non-whitelisted plugin, Jekyll 4), the escape hatch is switching the Pages source to "GitHub Actions" and adding an `actions/jekyll-build-pages` workflow. Flag in PITFALLS, not needed for v1.

## Recommended Build Order (CONFIG → CONTENT → THEME)

The owner edits files directly and commits to `main`. Order matters because each phase produces something deployable and verifiable.

| Order | Phase | What changes | Files touched | Verified by |
|-------|-------|-------------|---------------|-------------|
| **1. Bootstrap** | Fork + deploy | Repo exists, builds, serves something at the URL | (none — just the fork) | Visiting `https://harry-rogers.github.io/` and seeing the upstream demo content. |
| **2. Config** | Site identity | `url`, `title`, `name`, owner bio, Scholar/ORCID/GitHub/LinkedIn links, nav menu trimmed (remove "Blog Posts", "Guide") | `_config.yml`, `_data/authors.yml`, `_data/navigation.yml` | Landing page shows Harry's name, photo placeholder, correct links; menu shows only the pages we want. |
| **3. Content — collections** | Real academic data replaces demo | One file per publication, talk, teaching item, project. Delete every demo file in `_publications/`, `_talks/`, `_teaching/`, `_portfolio/` first. Empty `_posts/` and `_drafts/`. | `_publications/*.md`, `_talks/*.md`, `_teaching/*.md`, `_portfolio/*.md`, `files/*.bib`, `files/*.pdf` | `/publications/`, `/talks/`, `/teaching/`, `/portfolio/` index pages list real items. |
| **4. Content — pages** | Landing & static pages | About page (intro, research framing), tidy 404, drop `cv.md` link until v2 (CV deferred per PROJECT.md), kill `markdown.md` ("Guide"), strip blog archives if unused. | `_pages/about.md`, `_pages/cv.md` (delete or stub), `_pages/markdown.md` (delete), `_pages/*-archive.html` (delete unused) | Landing page reads as Harry's intro; no broken links from nav. |
| **5. News widget** | Milestone-only news section | Either repurpose `_posts/` with a strict "milestone-only" editorial rule, OR build a small `_data/news.yml` rendered by a sidebar include. (Recommend the latter — keeps "no blog" promise structural, not just editorial.) | `_data/news.yml` (new), `_includes/sidebar.html` (small edit) **or** `_pages/about.md` (inline) | News items appear; no blog archive page exposed. |
| **6. Theme — dark mode** | Visual personality | Swap palette in `_sass/_themes.scss`; optionally fork `_default_dark.scss → _harry_dark.scss` with one accent-colour tweak. | `_sass/_themes.scss`, `_sass/theme/_harry_dark.scss` (new) | Whole site renders dark on every page; accent colour matches preference. |
| **7. Polish** | Small differentiator + mobile pass | Personal touch (custom masthead text, favicon, social-share image, footer line). Check mobile breakpoints. | `_includes/masthead.html`, `_includes/footer.html`, `images/favicon*`, `images/og.png` | Site feels like Harry's, not the template's. |

### Why this order, not the reverse

- **Config first** because every subsequent build needs `url:` right or links break.
- **Content before theme** because theming an empty site means you're styling Lorem Ipsum — you only know if the palette works once real publication titles, venues, and dates are on the page. Most "looks wrong" feedback emerges only with real text.
- **Theme last** because dark-mode swap is a one-line config change that doesn't depend on anything above it — cheap to defer, cheap to retry, and if you do it first you'll keep re-checking the styling every time you touch content.
- **News widget mid-pipeline** because it depends on decisions about `_posts/` (which need to be made when emptying demo content in step 3), but its rendering touches the theme (sidebar include), so it sits between content and theme.

## Local Jekyll Dev Environment: Discussion

PROJECT.md states the owner will edit Markdown files and commit directly to `main`. The question is whether to flag this as a pitfall.

| Criterion | Edit-and-commit (no local Jekyll) | Local Jekyll (`bundle exec jekyll serve`) |
|-----------|-----------------------------------|-------------------------------------------|
| Setup cost | Zero. | Ruby 3.x + Bundler + `bundle install` (5–30 min, OS-dependent). |
| Iteration loop for content (`.md` edits) | 30–60 s (push → GH Pages rebuild). | ~1 s (live-reload). |
| Iteration loop for **Sass / layout** edits | 30–60 s per change, **and you can't see the diff before pushing.** | ~1 s. |
| Risk of broken `main` | Real — a typo in `_config.yml` breaks the whole site for 30–60 s. | Caught locally before push. |
| Risk of "works on Pages, broke locally" version skew | None (one source of truth). | Real — local Bundler can drift from GH Pages' pinned versions; mitigated by the `github-pages` gem. |

**Recommendation:** Don't require local Jekyll for the content phases (1–5). They're text-editor-only work and the feedback loop is fine. **Do recommend local Jekyll for phase 6 (theme/dark mode)** — iterating on SCSS variables via push-and-wait is genuinely painful (10–20 cycles to dial in colours × 60 s each = a wasted afternoon).

If the owner refuses to install Ruby, two workable alternatives for phase 6:
- **GitHub Codespaces** — academicpages ships `.devcontainer/`, so a Codespace runs `jekyll serve` in-browser with live preview. Free tier covers this use case easily.
- **Branch + preview deploy** — create a `theme-wip` branch, configure GitHub Pages to deploy from it temporarily, iterate there, merge to `main` when happy. Slower than Codespaces but zero local tooling.

**Pitfall flag for PITFALLS.md:** Editing `_config.yml` directly on `main` without a preview will at some point produce a 30-minute outage when a YAML typo breaks the build. Either accept it, use a `staging` branch, or use Codespaces.

## Key Risks & Anti-Patterns (forward to PITFALLS.md)

- **Renaming the repo wrong.** Must be `Harry-Rogers.github.io` (case-sensitive matching the username) for the user-site `/` URL. Anything else gets a `/<reponame>/` subpath, which breaks every absolute permalink in `_publications/*.md`.
- **Leaving demo content in.** Forgetting to delete `_publications/2009-10-01-paper-title-number-1.md` and friends is the single most common academicpages embarrassment.
- **Editing `_config.yml` and pushing without checking the build.** A single bad indent silently 500s the whole site for one build cycle.
- **Forking the Sass too aggressively.** academicpages' Sass tree is layered (`vendor/` ← `include/` ← `layout/` ← `theme/`). Edit only `theme/*` for colours. Editing `layout/` or `include/` makes upstream merges painful.
- **Using a non-whitelisted plugin.** If a "cool" Jekyll plugin requires turning off safe mode, you've left the GH-Pages-auto-build path and must switch to Actions. Avoid in v1.

## Sources

- [academicpages/academicpages.github.io (master)](https://github.com/academicpages/academicpages.github.io) — top-level structure verified directly.
- [`_data/` listing](https://github.com/academicpages/academicpages.github.io/tree/master/_data) — `authors.yml`, `navigation.yml`, `ui-text.yml`, `cv.json`.
- [`_layouts/` listing](https://github.com/academicpages/academicpages.github.io/tree/master/_layouts) — `default`, `single`, `archive`, `talk`, `cv-layout`, `splash`, `archive-taxonomy`, `compress`.
- [`_includes/` listing](https://github.com/academicpages/academicpages.github.io/tree/master/_includes) — `head`, `masthead`, `footer`, `author-profile`, `archive-single`, `archive-single-talk`, etc.
- [`_sass/theme/` listing](https://github.com/academicpages/academicpages.github.io/tree/master/_sass/theme) — six built-in palettes in light/dark pairs.
- [Sample publication front matter](https://raw.githubusercontent.com/academicpages/academicpages.github.io/master/_publications/2009-10-01-paper-title-number-1.md) — fields: `title`, `collection`, `category`, `permalink`, `excerpt`, `date`, `venue`, `paperurl`, `slidesurl`, `bibtexurl`, `citation`.
- [Sample talk front matter](https://raw.githubusercontent.com/academicpages/academicpages.github.io/master/_talks/2012-03-01-talk-1.md) — fields: `title`, `collection`, `type`, `permalink`, `venue`, `date`, `location`.
- [`_config.yml`](https://raw.githubusercontent.com/academicpages/academicpages.github.io/master/_config.yml) — confirms collections, defaults, plugins, GH-Pages whitelist.
- [`_pages/publications.html`](https://raw.githubusercontent.com/academicpages/academicpages.github.io/master/_pages/publications.html) — confirms `site.publications` iteration via `archive-single.html`.
- [`_data/navigation.yml`](https://raw.githubusercontent.com/academicpages/academicpages.github.io/master/_data/navigation.yml) — confirms default nav items.

Confidence on every claim in this document: **HIGH** (each is sourced from the live upstream repo, not inferred from training data).
