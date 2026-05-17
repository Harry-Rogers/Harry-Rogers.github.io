# Requirements — Harry Rogers Academic Website (v1)

Source of truth: [PROJECT.md](PROJECT.md). Research backing: [research/SUMMARY.md](research/SUMMARY.md).

REQ-ID format: `[CATEGORY]-[NN]`. Every v1 requirement maps to exactly one phase in [ROADMAP.md](ROADMAP.md).

## v1 Requirements

### Deployment & Build (DEPLOY)

- [ ] **DEPLOY-01**: Repo `Harry-Rogers/Harry-Rogers.github.io` (already exists, currently serves a stale 2020 hand-rolled site) is **force-overwritten** with the academicpages-based fork so the site is served from `https://harry-rogers.github.io/` with no project subpath. Old commit history is discarded.
- [ ] **DEPLOY-02**: `_config.yml` has `url: "https://harry-rogers.github.io"` and `baseurl: ""` — verified by a successful Pages build with no broken CSS or image paths
- [ ] **DEPLOY-03**: GitHub Pages source is set to `main` branch, root directory; the default Jekyll builder runs (no GitHub Actions workflow needed for v1)
- [ ] **DEPLOY-04**: `.ruby-version` pinned to `3.3.4`; the `Gemfile` uses the `github-pages` meta-gem; plugins enabled in `_config.yml` are exactly the four GitHub Pages allowlist plugins this site uses: `jekyll-feed`, `jekyll-sitemap`, `jekyll-seo-tag`, `jekyll-redirect-from`. No other plugins are enabled in v1. See [`CLAUDE.md`](../CLAUDE.md) "Whitelisted Plugins" and "What NOT to Use" tables for rationale.
- [ ] **DEPLOY-05**: GitHub Codespaces dev environment works out-of-the-box using academicpages' shipped `.devcontainer/`; running `bundle exec jekyll serve` inside the codespace renders the site at the forwarded port

### Landing Page & Identity (BIO)

- [ ] **BIO-01**: Landing page (`_pages/about.md`) introduces Harry by name, current role (Postdoctoral Research Associate, IBME, University of Oxford), and supervisor/group (Prof. Alison Noble, Biomedical Image Analysis)
- [ ] **BIO-02**: Landing page includes a profile photo and a 2-4 paragraph bio framed as "human-AI collaboration in medical AI broadly, currently medical education" — reconciles the imaging vs education tension explicitly
- [ ] **BIO-03**: Landing page features a "Now / Recently / Previously" three-block research-framing widget (medical education → medical imaging → precision agriculture), each block linking to relevant publications
- [ ] **BIO-04**: Author profile sidebar shows: name, role, affiliation, location, photo

### Contact & External Links (CONTACT)

- [ ] **CONTACT-01**: Email `Harry.Rogers@eng.ox.ac.uk` surfaced on landing page in an obfuscated form (HTML entities or JS — not a raw `mailto:`) to reduce scraper exposure
- [ ] **CONTACT-02**: Author sidebar links: ORCID (0000-0003-3227-5677), Google Scholar (sPwcwvMAAAAJ), GitHub (Harry-Rogers), LinkedIn (in/harry-rogers-ox), Oxford department page
- [ ] **CONTACT-03**: All external links use FontAwesome icons (no iframe widgets — Scholar blocks them, ORCID's widget breaks at <480px)

### Publications (PUBS)

- [ ] **PUBS-01**: `/publications/` page lists every publication in reverse-chronological order, grouped by year
- [ ] **PUBS-02**: Each publication has its own Markdown file in `_publications/YEAR-slug.md` with front matter: title, authors (Harry **bold**), venue, year, DOI/arXiv/preprint URLs, PDF link, citation count, abstract
- [ ] **PUBS-03**: Each publication page includes a copyable BibTeX block (hand-authored in the front matter; rendered via a custom Liquid include — no `jekyll-scholar`)
- [ ] **PUBS-04**: All 8 known publications populated (UK-RAS 2020, NCAA 2024, TAROS 2023, IEEE CASE 2023, KDIR 2023 [Best Paper badge], IEEE ISCC 2025, arXiv 2024, EPIA 2024)
- [ ] **PUBS-05**: KDIR 2023 paper visibly tagged "Best Paper Award"

### Talks (TALKS)

- [ ] **TALKS-01**: `/talks/` page lists conference talks, invited talks, and career talks (e.g., Ormiston Victory Academy career talk) in reverse-chronological order
- [ ] **TALKS-02**: Each talk entry has: title, venue, date, location, link to slides/poster if available
- [ ] **TALKS-03**: Talks page includes presentation sessions chaired (multiple AgriFoRwArdS CDT conferences)

### Teaching & Outreach (TEACH)

- [ ] **TEACH-01**: `/teaching/` page lists teaching activities and outreach work
- [ ] **TEACH-02**: Outreach entries include: Lego Mindstorms workshops (UEA, Yarmouth College, Thetford Academy via "Bringing Scientists to You"), Sensor CDT Team Challenge mentoring, AgriFoRwArdS CDT organisation committees (Drink Outside the Box, Summer School 2024, Annual Conference 2024 Programme Committee)

### Projects / Portfolio (PROJ)

- [ ] **PROJ-01**: `/projects/` (or `/portfolio/`) page highlights notable research projects with short descriptions and links to code/papers
- [ ] **PROJ-02**: Projects covered: Precision Spraying ("Closing The Loop", Syngenta partnership), Clock Drawing Test automation, Home Spirometry QA, PiCar (open-source Raspberry Pi agri-robot)

### News (NEWS)

- [ ] **NEWS-01**: A `_data/news.yml` file holds milestone entries (no `_posts/`, no `_news/` collection — structurally enforces "no blog")
- [ ] **NEWS-02**: News widget on the landing page shows the most recent 5 entries with date + one-line item (e.g. "Jan 2026: paper accepted at X")
- [ ] **NEWS-03**: Entries restricted to academic milestones only — papers accepted/published, talks given, awards, joining/leaving institutions. No personal/opinion entries
- [ ] **NEWS-04**: At least three real seed entries populated (e.g. starting Oxford postdoc, ISCC 2025 paper, KDIR 2023 Best Paper)

### Theme & Dark Mode (THEME)

- [ ] **THEME-01**: `minimal_mistakes_skin: "dark"` set in `_config.yml`; site renders dark by default
- [ ] **THEME-02**: Accent colour overridden to **Oxford blue (#002147)** via a forked Sass partial (e.g. `_sass/_harry_dark.scss`) — used for links, headers, button highlights
- [ ] **THEME-03**: No light-mode toggle UI present anywhere (masthead, sidebar, footer)
- [ ] **THEME-04**: Rouge syntax-highlighting CSS swapped to a dark variant (so code blocks don't flash white)
- [ ] **THEME-05**: No `@media (prefers-color-scheme: light)` rules — dark is the only theme
- [ ] **THEME-06**: `<meta name="theme-color">` set to the dark background colour so mobile browser chrome matches
- [ ] **THEME-07**: Light-mode-leak audit completed: masthead, footer, search overlay, author-profile borders, archive items, code blocks, Rouge — all visually verified on a deployed page

### Polish & Quality (POLISH)

- [ ] **POLISH-01**: Site is responsive: layouts work on a 375px-wide viewport (mobile) without horizontal scroll or overflowing text
- [ ] **POLISH-02**: Favicon set (replacing the academicpages default)
- [ ] **POLISH-03**: Open Graph image + meta tags set so link previews on social/Slack render with a real card
- [ ] **POLISH-04**: SEO meta: site title, description, author, ORCID structured data; verified via View Source on the deployed site
- [ ] **POLISH-05**: `htmlproofer` (or equivalent) run shows zero broken internal links and no broken images
- [ ] **POLISH-06**: All academicpages demo content (`_posts/2010-08-14-blog-post-2.md`, demo publications, demo talks, demo portfolio, "markdown.html" page, "guide" page) deleted — `grep -ri "lorem\|sample text\|cras mattis"` returns zero results
- [ ] **POLISH-07**: Footer shows a `last_updated` line driven by build date, not hardcoded
- [ ] **POLISH-08**: `/cv/` route and any "Download CV" nav links removed entirely (no broken link or "coming soon" stub — CV is out of scope for v1)
- [ ] **POLISH-09**: Lighthouse mobile score ≥ 90 on the landing page (Performance + Accessibility + SEO)

## v2 Requirements (deferred from v1, valid candidates for the next milestone)

- [ ] CV PDF download (owner doesn't have a CV ready)
- [ ] Embedded talk videos (conditional on whether recordings exist for EPIA 2024, ISCC 2025, KDIR 2023, TAROS 2023)
- [ ] Downloadable poster PDFs from conferences
- [ ] Reproducibility badges per paper (code/data/preprint availability) — high-signal for medical-AI credibility but content-dependent
- [ ] Brief ethics/disclosure statement footer (low effort, but content needs owner sign-off)

## Out of Scope

- **Blog / personal essays** — explicitly rejected by owner ("blogs feel cringy"). News feed (NEWS-*) replaces this need for milestone updates. The `_data/news.yml` approach makes blogging structurally impossible without a deliberate change
- **Light mode / theme toggle** — dark-only by decision; not a stretch goal
- **CV PDF download in v1** — deferred to v2; v1 actively removes any CV link rather than ship a broken/placeholder one
- **Custom domain (e.g. harryrogers.com)** — `harry-rogers.github.io` is canonical; no domain purchase planned
- **Site search beyond academicpages default** — basic search ships with the template; no extension planned
- **Comments, analytics dashboards, newsletter signup, RSS for non-news content** — not academic-site essentials
- **Multi-language** — English only
- **AI chatbot / "ask my papers"** — hallucination risk on a medical-AI researcher's site is unacceptable
- **Auto-fetched Scholar citation counts via scraping** — fragile, ToS-grey; static numbers updated manually are fine
- **`jekyll-scholar` plugin** — not on the GitHub Pages allowlist; using it would force a GitHub Actions custom build, which contradicts the "no CI" decision in PROJECT.md
- **Twitter/X embeds** — owner has no listed Twitter; avoid the dependency

## Traceability

44/44 v1 requirements mapped to phases in [ROADMAP.md](ROADMAP.md). 100% coverage, no orphans, no duplicates.

| Requirement | Phase | Status |
|---|---|---|
| DEPLOY-01 | Phase 1: Bootstrap & Deploy | Pending |
| DEPLOY-02 | Phase 1: Bootstrap & Deploy | Pending |
| DEPLOY-03 | Phase 1: Bootstrap & Deploy | Pending |
| DEPLOY-04 | Phase 1: Bootstrap & Deploy | Pending |
| DEPLOY-05 | Phase 1: Bootstrap & Deploy | Pending |
| POLISH-06 | Phase 2: Content Population | Pending |
| BIO-01 | Phase 2: Content Population | Pending |
| BIO-02 | Phase 2: Content Population | Pending |
| BIO-03 | Phase 2: Content Population | Pending |
| BIO-04 | Phase 2: Content Population | Pending |
| CONTACT-02 | Phase 2: Content Population | Pending |
| CONTACT-03 | Phase 2: Content Population | Pending |
| PUBS-01 | Phase 2: Content Population | Pending |
| PUBS-02 | Phase 2: Content Population | Pending |
| PUBS-03 | Phase 2: Content Population | Pending |
| PUBS-04 | Phase 2: Content Population | Pending |
| PUBS-05 | Phase 2: Content Population | Pending |
| TALKS-01 | Phase 2: Content Population | Pending |
| TALKS-02 | Phase 2: Content Population | Pending |
| TALKS-03 | Phase 2: Content Population | Pending |
| TEACH-01 | Phase 2: Content Population | Pending |
| TEACH-02 | Phase 2: Content Population | Pending |
| PROJ-01 | Phase 2: Content Population | Pending |
| PROJ-02 | Phase 2: Content Population | Pending |
| NEWS-01 | Phase 2: Content Population | Pending |
| NEWS-02 | Phase 2: Content Population | Pending |
| NEWS-03 | Phase 2: Content Population | Pending |
| NEWS-04 | Phase 2: Content Population | Pending |
| THEME-01 | Phase 3: Dark Theme & Customisation | Pending |
| THEME-02 | Phase 3: Dark Theme & Customisation | Pending |
| THEME-03 | Phase 3: Dark Theme & Customisation | Pending |
| THEME-04 | Phase 3: Dark Theme & Customisation | Pending |
| THEME-05 | Phase 3: Dark Theme & Customisation | Pending |
| THEME-06 | Phase 3: Dark Theme & Customisation | Pending |
| THEME-07 | Phase 3: Dark Theme & Customisation | Pending |
| CONTACT-01 | Phase 4: Polish & Ship | Pending |
| POLISH-01 | Phase 4: Polish & Ship | Pending |
| POLISH-02 | Phase 4: Polish & Ship | Pending |
| POLISH-03 | Phase 4: Polish & Ship | Pending |
| POLISH-04 | Phase 4: Polish & Ship | Pending |
| POLISH-05 | Phase 4: Polish & Ship | Pending |
| POLISH-07 | Phase 4: Polish & Ship | Pending |
| POLISH-08 | Phase 4: Polish & Ship | Pending |
| POLISH-09 | Phase 4: Polish & Ship | Pending |

---
*Last updated: 2026-05-17 after roadmap creation*
