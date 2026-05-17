# Project Research Summary

**Project:** Harry Rogers — Academic Website (harry-rogers.github.io)
**Domain:** Personal academic site, Jekyll/GitHub Pages, academicpages fork
**Researched:** 2026-05-17
**Confidence:** HIGH

## Executive Summary

This is a personal academic website for a CS/ML postdoc (human-AI collaboration in medical AI, University of Oxford). The canonical approach is to fork the `academicpages/academicpages.github.io` Jekyll template and deploy it via GitHub Pages' default, no-Actions builder. The entire stack (Jekyll 3.10.0, Ruby 3.3.4, Minimal Mistakes theme, a whitelist of bundled plugins) is pinned by the `github-pages` meta-gem and runs on GitHub's runners without any custom CI. The owner edits Markdown files and commits to `main`; Pages auto-rebuilds in 30-60 seconds. No CI YAML, no Ruby Actions, no custom build step.

The recommended build order — confirmed independently by all four researchers — is: Bootstrap (fork + verify deployment) → Config (site identity, URL, nav) → Content (publications, talks, teaching, projects) → News (milestone-only collection) → Theme/Dark mode (Sass variable overrides) → Polish (SEO, favicon, OG, mobile, link check). This ordering is non-trivial: `url`/`baseurl` must be correct before any content links work, and real content must be on the page before dark-mode palette tweaks can be evaluated accurately.

The primary risks are front-loaded in the Bootstrap and Config phases: wrong repo name, wrong `baseurl`, wrong Pages source branch, and non-whitelisted plugins each silently break the entire site. Once those four are cleared, subsequent phases are low-risk text authoring and Sass variable overrides. The publications page is the highest-effort content item (8 papers × structured front matter + BibTeX block each), but it is well-structured work, not technically complex. Dark-mode theming requires local Jekyll for the Sass iteration loop — otherwise the 60-second push-and-rebuild cycle makes colour tuning impractical.

## Key Findings

### Recommended Stack

**Stack one-liner:** Jekyll 3.10.0 on Ruby 3.3.4, deployed via GitHub Pages default builder (no Actions), academicpages fork with Minimal Mistakes vendored, publications from `_publications/*.md` (no jekyll-scholar), dark skin via `minimal_mistakes_skin: "dark"` + Sass overrides in `assets/css/main.scss`.

**Core technologies:**

- **Jekyll 3.10.0** — static site generator; the exact version GitHub Pages runs; pinned via `github-pages` meta-gem. Any other version (4.x, 5.x) forces a custom GitHub Action — explicit non-goal.
- **Ruby 3.3.4** — the runtime Pages uses; pin via `.ruby-version` so local dev matches production.
- **`github-pages` gem (latest)** — meta-gem that pins Jekyll + all whitelisted plugins to Pages' own versions; use `bundle update github-pages` monthly; never replace with bare `gem "jekyll"`.
- **Minimal Mistakes (vendored in academicpages)** — UI theme; already vendored into `_sass/`, `_includes/`, `_layouts/`; do NOT switch to `remote_theme` (silently overrides the customised includes).
- **Plugins (whitelisted, safe):** `jekyll-feed` (RSS for News), `jekyll-sitemap`, `jekyll-seo-tag`, `jekyll-redirect-from`. All others either already present in academicpages or explicitly disabled.
- **jekyll-scholar: EXCLUDED** — not on the GitHub Pages plugin allowlist; forces a custom Action; overkill for 8 papers. Use `_publications/*.md` + `markdown_generator/pubsFromBib.py` (runs locally, never on the Pages runner).
- **Dark mode:** `minimal_mistakes_skin: "dark"` in `_config.yml`, Sass variable overrides in `assets/css/main.scss` (variables BEFORE `@import "minimal-mistakes/skins/dark"; @import "minimal-mistakes";`). Rouge syntax highlighter needs a separate dark theme (`rougify style monokai`).

**Critical version constraint:** Committing `Gemfile.lock` is mandatory. Pages ignores it but local dev parity depends on it. Never run bare `jekyll serve` — always `bundle exec jekyll serve`.

### Expected Features

**Must have (table stakes — v1):**

- Landing page: real headshot, name, current role (Oxford IBME / Noble group), one-paragraph research framing
- Contact block on landing page: email, ORCID (0000-0003-3227-5677), Google Scholar (sPwcwvMAAAAJ), GitHub, LinkedIn — rendered via `_config.yml` author links with FontAwesome icons; no iframes
- Publications page: all 8 known papers, each with title, authors, venue, year, PDF/DOI/arXiv link, BibTeX block (rendered inline in front matter, copy-button via ~20 lines vanilla JS)
- Talks page (reverse-chronological)
- Teaching page (reverse-chronological, includes AgriFoRwArdS workshops, Ormiston Victory career talk, Lego Mindstorms outreach, mentoring)
- Projects/portfolio page: precision spraying, clock-drawing test, home spirometry, PiCar
- Dark-mode-only theme (no toggle, no light-mode fallback)
- News collection (milestone-only: papers accepted, talks, awards, role changes) — blog disabled
- Mobile-responsive layout
- SEO basics: `title`, `description`, OG image, `sitemap.xml`, favicon
- No broken links — specifically NO "Download CV" link (deferred per PROJECT.md)

**Differentiators to land in v1 (low cost, high return):**

- **Three-block research framing** on landing page: "Now: Human-AI collaboration in medical education / Recently: Medical imaging (clock-drawing, spirometry) / Previously: Precision agriculture (PhD)" — resolves the imaging-vs-education tension, tells a career story; no implementation cost beyond authoring `about.md`
- **Custom dark accent colour** (something other than the default teal `#00adb5`) — cheapest possible personality differentiator; satisfies PROJECT.md's "at least one visual touch" requirement; tune during Theme phase
- **Award badges on papers** — Best Paper @ KDIR 2023 and Best Application nomination @ TAROS 2023; a front-matter `award:` field + small badge component
- **Per-paper BibTeX copy block** — embed BibTeX as `bibtex:` field in front matter, render inside `<pre><code>` on the publication page; ~20 lines of vanilla JS for the copy button

**Should have — add soon after launch (v1.x, not blocking v1):**

- Per-paper TL;DR one-liner (`tldr:` front-matter field)
- Reproducibility badges per paper (`code:`, `data:`, `preprint:` fields rendered as pill badges)
- Paper grouping by research theme (Medical AI / Precision Agriculture / Explainability) using Liquid `group_by`
- Brief ethics / responsible-AI statement on landing page or `/ethics/`
- Embedded talk videos (YouTube iframe on `_talks/*.md`) where recordings exist
- Manually-curated citation counts with "as of YYYY-MM" caveat
- Funding/disclosure footer (UKRI, AgriFoRwArdS CDT, Syngenta, Oxford IBME)

**Defer to v2+:**

- CV PDF download — blocked until a current PDF is authored; no broken link in v1
- "Research at a glance" SVG diagram — MEDIUM complexity, design risk; better after site is live
- Datasets page / dataset cards — may need ethical clearance
- Privacy-respecting analytics (server-side only, never user-facing)

**Anti-features (explicitly not building):**

Blog, light-mode toggle, live Scholar citation scraper, AI chatbot, custom domain, comments, newsletter, auto-generated CV, X/Twitter embed.

### Architecture Approach

The site follows academicpages' canonical three-layer structure: **Config** (`_config.yml`, `_data/*.yml`) → **Content** (one Markdown file per item in `_publications/`, `_talks/`, `_teaching/`, `_portfolio/`, new `_news/` collection) → **Theme** (`_sass/`, `_includes/`, `_layouts/`, `assets/css/main.scss`). GitHub Pages runs Jekyll as the build step; `_site/` is the output served by the CDN. No custom Actions, no Ruby scripts on the runner.

**Major components:**

1. **Config layer** (`_config.yml`, `_data/navigation.yml`, `_data/authors.yml`) — site URL, title, author identity, social links, nav menu, plugin list; errors here break every page
2. **Content collections** (`_publications/`, `_talks/`, `_teaching/`, `_portfolio/`, `_news/`) — one `.md` file per item; each has structured front matter consumed by layouts; delete ALL demo content before launch
3. **Page layer** (`_pages/about.md`, `publications.html`, `talks.html`, `teaching.html`, `portfolio.html`, `news.html`) — index pages that iterate over collections via Liquid; `about.md` is the landing page
4. **Theme layer** (`_sass/theme/_default_dark.scss` forked to `_harry_dark.scss`, `assets/css/main.scss` for overrides) — edit ONLY via `assets/css/main.scss` overrides and a forked skin file; never touch `_sass/minimal-mistakes/*` directly
5. **Static assets** (`images/`, `files/`) — profile photo, paper PDFs, BibTeX files; always use site-absolute paths (`/images/foo.jpg`, not relative)

**News collection:** Register a `_news/` collection in `_config.yml` (do NOT repurpose `_posts/`); empty `_posts/` entirely to structurally prevent blog content. Each news entry: `title`, `date`, `category` (paper/talk/award/role), one-sentence body.

**Dark mode wiring:** Fork `_default_dark.scss` → `_harry_dark.scss` and tweak one accent colour; override remaining variables in `assets/css/main.scss`; generate a dark Rouge syntax CSS via `rougify`. Set `<meta name="color-scheme" content="dark">` in `_includes/head/custom.html` to prevent form/scrollbar flash.

### Critical Pitfalls

1. **Repo name mismatch (CRITICAL, gates Bootstrap)** — The repo MUST be named `Harry-Rogers.github.io` (matching the GitHub username) to deploy at the root URL `https://harry-rogers.github.io/`. Forking from academicpages creates a repo named `academicpages.github.io` which deploys as a project page at a subpath, breaking every absolute link. Verify: `curl -I https://harry-rogers.github.io/` returns 200 with no path component.

2. **`baseurl`/`url` left at academicpages defaults (CRITICAL, gates Config)** — The forked `_config.yml` ships with `url: "https://academicpages.github.io"`. Set `url: "https://harry-rogers.github.io"` and `baseurl: ""` (empty string, not `"/"`) in the very first config commit. Every sitemap entry, OG tag, and canonical link is wrong until this is fixed.

3. **Pages build source branch mismatch (CRITICAL, gates Bootstrap)** — academicpages was authored when the default branch was `master`; new forks default to `main`. Settings → Pages may show branch `master` (non-existent). Fix: Settings → Pages → Source → Branch: `main` / `/ (root)`.

4. **Non-whitelisted plugins break the build silently (CRITICAL, gates Config)** — Adding `jekyll-scholar` causes the Pages build to fail with a cryptic Liquid error. The correct approach for 8 papers is `_publications/*.md` with a `bibtex:` front-matter field.

5. **Demo/placeholder content shipped in production (HIGH, gates Content launch)** — academicpages ships fake publications, sample talks, placeholder teaching entries, and a Jekyll welcome blog post. Run `grep -r "paper title number\|lorem ipsum\|john doe\|teaching experience" .` before sharing the URL; result must be zero.

6. **Dark-mode light bleed (HIGH, gates Theme)** — Setting `minimal_mistakes_skin: "dark"` alone is insufficient. Light bleed shows in: `<table>` backgrounds, inline `<code>`, Rouge syntax highlighting, form controls. Must also override Sass variables, replace the Rouge theme with a dark variant, set `<meta name="color-scheme" content="dark">`.

7. **Sass overrides in the wrong layer (MEDIUM, ongoing)** — Editing `_sass/minimal-mistakes/*.scss` directly makes upstream merges painful. All custom Sass goes in `assets/css/main.scss` and a forked skin file only.

## Implications for Roadmap

All four researchers converged on the same five-phase ordering. This is the recommended phase sequence.

### Phase 1: Bootstrap

**Rationale:** The deployment pipeline must be verified before any other work has value. A misconfigured repo name or Pages source branch means all subsequent commits silently do nothing.

**Delivers:** `https://harry-rogers.github.io/` resolves to a live (demo-content) site with no 404, no subpath in the URL, and Pages auto-building on push to `main`.

**Addresses:** "Site builds and deploys at canonical URL" (table stakes baseline)

**Bootstrap phase MUST end with all of these verified:**
- [ ] `curl -I https://harry-rogers.github.io/` returns 200, no path component in URL
- [ ] Settings → Pages shows `main` branch as source, last build succeeded
- [ ] `_config.yml`: `url: "https://harry-rogers.github.io"`, `baseurl: ""`, `repository: "Harry-Rogers/Harry-Rogers.github.io"`
- [ ] `Gemfile` uses `github-pages` meta-gem; `Gemfile.lock` committed
- [ ] `_config.yml` plugins list is a subset of the Pages allowlist (pages.github.com/versions/)

**Research flag:** Standard patterns. No deeper research needed.

### Phase 2: Config

**Rationale:** All content links, sitemap entries, OG tags, and social icons depend on correct config values. Configuring identity before authoring content means every collection item generates correct permalinks on first push.

**Delivers:** Landing page shows Harry's name, current role, correct sidebar links (email, ORCID, Scholar, GitHub, LinkedIn); nav shows only the intended pages (Home, Publications, Talks, Teaching, Projects, News — no Blog, no CV link yet); demo content still present but identity is correct.

**Files touched:** `_config.yml`, `_data/navigation.yml`, `_data/authors.yml`

**Must avoid:** Leaving the Blog / Guide nav items in; adding a "Download CV" nav link (deferred — broken link risk).

**Research flag:** Standard patterns. No deeper research needed.

### Phase 3: Content

**Rationale:** Real content must exist before the dark-mode palette can be evaluated. Content authoring is the largest phase by time but low technical risk. Delete all demo content first.

**Delivers:** All five collection types populated with real data; all demo/placeholder files deleted; `_posts/` emptied; profile photo is Harry's actual photo; `about.md` has the three-block research framing (Now / Recently / Previously).

**Key content decisions:**
- Publications: one `_publications/YYYY-MM-DD-slug.md` per paper; front matter includes `title`, `collection: publications`, `permalink`, `date`, `venue`, `paperurl` (DOI preferred), `citation`, `bibtex` (raw block), `award` (where applicable)
- BibTeX workflow: run `markdown_generator/pubsFromBib.py` locally on a Scholar/ORCID export; hand-clean generated files; never run on the Pages runner
- Research framing on `about.md`: three-block structure (Now / Recently / Previously)
- `_posts/` emptied entirely — blog structurally disabled, not just editorially suppressed

**Must avoid:** Use site-absolute image paths (`/images/...`); validate BibTeX, use DOI permalinks, bold Harry's name in citation strings, cross-check against ORCID; pre-launch grep for placeholder strings.

**Exit gate:** `grep -r "paper title number\|lorem ipsum\|john doe\|teaching experience [0-9]" .` returns zero results.

**Research flag:** Standard patterns. The BibTeX-via-pubsFromBib.py workflow is documented in the academicpages repo. No deeper research needed.

### Phase 4: News

**Rationale:** News sits between Content and Theme because the `_posts/` decision (made in Content) affects how News is scaffolded, and News sidebar rendering touches the theme layer.

**Delivers:** A `_news/` collection registered in `_config.yml`; `_pages/news.md` listing milestone entries; at least one real entry; `_posts/` confirmed empty; no blog archive page exposed in nav.

**Architecture:** Register `_news/` as a distinct collection in `_config.yml` — not `_posts/`. This makes "no blog" a structural guarantee, not just an editorial rule.

**Research flag:** Well-understood Jekyll custom collection pattern. No deeper research needed.

### Phase 5: Theme / Dark Mode

**Rationale:** Dark mode is the last functional step before polish. Doing it here means real content is visible while tuning colours, which is the only way to know if the palette actually works.

**Delivers:** Site renders dark on every page with no light bleed; a custom accent colour is set (the key personality differentiator); Rouge syntax highlighting is dark-themed; `<meta name="color-scheme" content="dark">` and `<meta name="theme-color">` are set; no theme-toggle UI exists anywhere.

**Recommended approach:**
1. `minimal_mistakes_skin: "dark"` in `_config.yml`
2. Fork `_sass/theme/_default_dark.scss` → `_sass/theme/_harry_dark.scss`; change accent colour
3. Update `_sass/_themes.scss` to import `_harry_dark`
4. Put all remaining overrides in `assets/css/main.scss` (variables before `@import` lines)
5. Generate dark Rouge CSS: `bundle exec rougify style monokai > assets/css/rouge.css`

**Must avoid:** Do not use `@media (prefers-color-scheme: dark)` — force dark unconditionally. Never edit `_sass/minimal-mistakes/*` directly.

**Local dev strongly recommended for this phase:** Iterating Sass variables via 60-second push-and-rebuild cycles is impractical. Options: local Ruby + `bundle exec jekyll serve --livereload`, or GitHub Codespaces (academicpages ships `.devcontainer/`).

**Research flag:** Well-documented Minimal Mistakes patterns. Variable names verified against upstream source. No deeper research needed.

### Phase 6: Polish

**Rationale:** SEO, OG metadata, favicon, mobile verification, link checking, and email obfuscation are cosmetic or defensive — best done last so they can be verified against the final state of all pages.

**Delivers:** Favicon present; OG image (1200×630 headshot card) configured; `jekyll-seo-tag` producing correct `<title>` and meta description on every page; `sitemap.xml` listing `harry-rogers.github.io` URLs; email address obfuscated (not plain `mailto:`); Lighthouse mobile score ≥ 90; `bundle exec htmlproofer ./_site --disable-external false` returns zero broken links; custom dark-themed 404 page; HTTPS enforced in Pages settings.

**Research flag:** Standard patterns. No deeper research needed.

### Phase Ordering Rationale

- **Bootstrap before Config:** Cannot verify `baseurl` is correct until the site is live at the URL.
- **Config before Content:** Every collection permalink is generated from `_config.yml` values; wrong URL = wrong links in all 8 publication files.
- **Content before Theme:** Real text is required to evaluate whether a colour palette works.
- **News between Content and Theme:** The `_posts/` decision (made in Content) affects News scaffolding; News sidebar rendering touches the theme layer.
- **Theme before Polish:** SEO metadata and OG images reference the final visual identity; generate them once styling is locked.
- **Dark mode at Theme, not earlier:** It is a one-line change with no dependencies; deferring it saves debugging sessions where you cannot tell if a dark-mode glitch is a Sass issue or a content issue.

### Research Flags

No phase requires a `/gsd-research-phase` call. All phases have well-documented, standard patterns. The research done here is sufficient.

## Open Questions to Surface Before Locking the Roadmap

1. **Ruby / local-dev preference (affects Phase 5 planning):** Is Harry willing/able to install Ruby 3.3.4 locally (`rbenv`/`asdf`) for the Sass/dark-mode iteration phase? If no, the plan should default to GitHub Codespaces (`.devcontainer/` is already in academicpages). If Codespaces is also unacceptable, a `theme-wip` branch with Pages preview is the fallback — slower but zero local tooling.

2. **BibTeX source strategy (affects Phase 3 planning):** Should publications be authored by running `markdown_generator/pubsFromBib.py` on a `.bib` export (requires Python locally, produces files that need hand-cleaning) or authored by hand directly as `_publications/*.md` files? For 8 papers, either is reasonable. The script saves ~30 minutes but introduces a Python dependency and outputs that need auditing.

3. **News widget approach (affects Phase 4 planning):** The research recommends a `_news/` collection (separate from `_posts/`, structural guarantee of no-blog). An alternative is a `_data/news.yml` data file rendered inline on `about.md` — simpler but no per-entry permalink. Which does Harry prefer?

4. **Accent colour for the dark theme (context for Phase 5):** The default dark skin uses teal (`#00adb5`). Oxford blue (`#002147`) or a purple are both options. Does Harry have a preference? (Does not block planning, but useful before Phase 5 starts.)

5. **Talk recordings (context for Phase 3 and v1.x):** Does Harry have YouTube/Vimeo links for any conference talks? This determines whether the "embedded talk videos" differentiator can land in v1 or must wait.

## Confidence Assessment

| Area | Confidence | Notes |
|------|------------|-------|
| Stack | HIGH | All technology choices verified against pages-gem manifest and upstream repos. Jekyll 3.10.0, Ruby 3.3.4, plugin allowlist all confirmed against official sources. |
| Features | HIGH | Table-stakes features verified against academicpages template. Differentiators synthesised from 2025 Best Personal Academic Websites contest and medical-AI literature. Anti-features cross-checked against PROJECT.md decisions. |
| Architecture | HIGH | Entire directory structure verified directly against the live upstream academicpages repo. Data flow trace is sourced from actual template files. |
| Pitfalls | HIGH / MEDIUM | GitHub Pages deployment mechanics are authoritative. Community pitfall frequency is aggregated from Issues threads — well-corroborated but anecdotal. |

**Overall confidence: HIGH**

### Gaps to Address

- **Dark-mode accent colour:** Not decided yet; requires Harry's input. Does not block planning.
- **Local dev toolchain:** Whether Ruby/Codespaces is acceptable for Phase 5 is unknown; confirm before Phase 5 begins.
- **Publication metadata completeness:** The 8 papers listed in PROJECT.md are "to verify against Scholar at content phase." DOIs, PDFs, and full author lists need confirmation during Phase 3.
- **Talk recordings:** Unknown whether any conference talks were recorded. Determine during Phase 3 content audit.
- **Dataset descriptions:** Spirometry / clock-drawing data may require ethical clearance before publishing. Deferred to v2+.

## Sources

### Primary (HIGH confidence)

- [GitHub Pages dependency versions (pages-gem manifest)](https://pages.github.com/versions/) — Jekyll 3.10.0, Ruby 3.3.4, plugin allowlist
- [academicpages/academicpages.github.io](https://github.com/academicpages/academicpages.github.io) — directory structure, collection schemas, `_config.yml` defaults, `pubsFromBib.py`
- [Minimal Mistakes `_dark.scss`](https://github.com/mmistakes/minimal-mistakes/blob/master/_sass/minimal-mistakes/skins/_dark.scss) — exact Sass variable names and defaults
- [Minimal Mistakes configuration docs](https://mmistakes.github.io/minimal-mistakes/docs/configuration/) — skin system, `author.links`, override pattern
- [Minimal Mistakes stylesheets docs](https://mmistakes.github.io/minimal-mistakes/docs/stylesheets/) — variables-before-import pattern
- [GitHub Pages: user vs project sites](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages) — repo naming rules
- [jekyll-seo-tag](https://github.com/jekyll/jekyll-seo-tag) — OG/meta tag configuration

### Secondary (MEDIUM confidence)

- [Winners of Best Personal Academic Websites Contest 2025](https://theacademicdesigner.com/2025/winners-of-the-best-personal-academic-websites-contest-2025/) — differentiator patterns
- [The reproducibility issues that haunt health-care AI (Nature, 2023)](https://www.nature.com/articles/d41586-023-00023-2) — reproducibility badge rationale
- [Trust, Trustworthiness, and the Future of Medical AI (JMIR, 2025)](https://www.jmir.org/2025/1/e71236) — ethics statement rationale
- [Jekyll Talk: jekyll-scholar not whitelisted](https://talk.jekyllrb.com/t/github-pages-the-demo-site-uses-jekyll-4-and-a-third-party-plugin-both-of-which-are-currently-not-whitelisted-for-use-on-github-pages/7128) — plugin exclusion rationale
- academicpages GitHub Issues — recurring baseurl, plugin, placeholder-content pitfalls

---
*Research completed: 2026-05-17*
*Ready for roadmap: yes*
