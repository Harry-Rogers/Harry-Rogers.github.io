# Roadmap — Harry Rogers Academic Website (v1)

Source of truth: [PROJECT.md](PROJECT.md). Requirements: [REQUIREMENTS.md](REQUIREMENTS.md). Research: [research/SUMMARY.md](research/SUMMARY.md).

**Granularity:** coarse (4 phases, strictly sequential)
**Coverage:** 44/44 v1 requirements mapped
**Build order rationale:** horizontal layers — `deploy → content → theme → polish`. Each layer's outputs are preconditions for the next (you cannot evaluate a dark palette against placeholder text, and you cannot SEO-audit a site whose URL canonicalisation is broken).

## Phases

- [ ] **Phase 1: Bootstrap & Deploy** — Fork academicpages, rename repo, fix URL/baseurl/branch/plugins, verify default Pages build serves at the canonical URL.
- [ ] **Phase 2: Content Population** — Purge all demo content, then populate real bio, publications, talks, teaching, projects, and news data.
- [ ] **Phase 3: Dark Theme & Customisation** — Force dark-only theme with Oxford-blue accent, kill all light-mode leaks, set theme-color metadata.
- [ ] **Phase 4: Polish & Ship** — Favicon, OG, SEO meta, email obfuscation, mobile + Lighthouse audit, broken-link gate, CV route removal, ship.

## Phase Details

### Phase 1: Bootstrap & Deploy
**Goal**: The existing `Harry-Rogers/Harry-Rogers.github.io` repo (currently serving a stale 2020 hand-rolled site) is **force-overwritten** with an academicpages-based fork, and the site at `https://harry-rogers.github.io/` builds automatically on every push to `main` via the default Pages builder, with all five "Bootstrap gates" green.
**Depends on**: none
**Brownfield note**: The repo exists and is live. Owner has decided to discard the old content and history entirely (no salvage). The first task in this phase's plan is to mirror the academicpages template into this working tree, replace remote `main` with our local `main`, and confirm Pages rebuilds.
**Requirements**: DEPLOY-01, DEPLOY-02, DEPLOY-03, DEPLOY-04, DEPLOY-05
**Success Criteria** (what must be TRUE):
  1. `curl -I https://harry-rogers.github.io/` returns 200 from the root URL with no project subpath (i.e. no `/academicpages.github.io/` or `/Harry-Rogers.github.io/` in the served URL).
  2. Settings → Pages shows source = `main` branch, root directory, and the most recent `pages-build-deployment` workflow run is green.
  3. `_config.yml` contains exactly `url: "https://harry-rogers.github.io"`, `baseurl: ""` (empty string, not `"/"`), and `repository: "Harry-Rogers/Harry-Rogers.github.io"` — verified by view-source on the deployed homepage showing canonical link at `harry-rogers.github.io` (no `academicpages` references).
  4. `Gemfile` uses the `github-pages` meta-gem, `Gemfile.lock` is committed, and `.ruby-version` pins `3.3.4`; the `_config.yml` `plugins:` list is a strict subset of the GitHub Pages allowlist at pages.github.com/versions/.
  5. Opening the repo in GitHub Codespaces and running `bundle exec jekyll serve` inside the shipped `.devcontainer/` renders the site at the forwarded port with no Liquid or Sass errors.
**Plans**: 5 plans
- [ ] 01-01-PLAN.md — Import academicpages template into working tree (preserve .planning/, CLAUDE.md, headshot)
- [ ] 01-02-PLAN.md — Configure _config.yml (url/baseurl/repository/plugins), pin .ruby-version=3.3.4, switch Gemfile to github-pages meta-gem, commit Gemfile.lock
- [ ] 01-03-PLAN.md — Reconcile REQUIREMENTS.md DEPLOY-04 wording with locked plugin allowlist
- [ ] 01-04-PLAN.md — Add origin, force-push to main, owner sets Pages source=main/root, verify canonical URL HTTP 200 with no academicpages refs
- [ ] 01-05-PLAN.md — Codespaces UAT: owner runs bundle exec jekyll serve inside shipped devcontainer, confirms forwarded port renders
**UI hint**: yes

**Phase 1 Bootstrap Gates (must ALL pass before exiting):**
  - [ ] G1: Canonical URL resolves at root, no subpath
  - [ ] G2: `baseurl: ""` (empty string), `url: "https://harry-rogers.github.io"`
  - [ ] G3: `main` is the Pages build source, last build green
  - [ ] G4: Plugins are allowlist-only (no `jekyll-scholar` or other non-whitelisted gems)
  - [ ] G5: `Gemfile.lock` committed, `.ruby-version` = `3.3.4`

---

### Phase 2: Content Population
**Goal**: Every demo file is gone and every real-world content surface (landing page, publications, talks, teaching, projects, news, author profile, navigation) is populated with Harry's actual academic data — so anyone who lands on the site within 10 seconds learns who he is, what he works on, and how to reach him.
**Depends on**: Phase 1
**Requirements**: POLISH-06, BIO-01, BIO-02, BIO-03, BIO-04, CONTACT-02, CONTACT-03, PUBS-01, PUBS-02, PUBS-03, PUBS-04, PUBS-05, TALKS-01, TALKS-02, TALKS-03, TEACH-01, TEACH-02, PROJ-01, PROJ-02, NEWS-01, NEWS-02, NEWS-03, NEWS-04
**Success Criteria** (what must be TRUE):
  1. Running `grep -ri "lorem\|sample text\|cras mattis\|paper title number\|john doe\|teaching experience [0-9]" .` at the repo root returns **zero matches** — confirmed BEFORE any real content is written (this is the first task of the phase, not the last).
  2. A visitor loading the landing page sees Harry's photo, his name, current role (Postdoctoral Research Associate, IBME, Oxford / Noble group), a 2-4 paragraph "human-AI collaboration in medical AI broadly, currently medical education" bio, and a three-block "Now / Recently / Previously" research-framing widget (medical education → medical imaging → precision agriculture).
  3. The `/publications/` page lists all 8 known papers in reverse-chronological order, each with title, authors (Harry bolded), venue, year, DOI/arXiv/preprint link, and an inline copyable BibTeX block; the KDIR 2023 entry visibly shows a "Best Paper Award" badge.
  4. The `/talks/`, `/teaching/`, and `/portfolio/` pages each render real entries (no template demo items remain), and the author profile sidebar shows ORCID, Google Scholar, GitHub, LinkedIn, and Oxford department links — each rendered as a FontAwesome icon link (no iframes, no embedded widgets).
  5. A News widget on the landing page shows at least 3 milestone entries (e.g. "starting Oxford postdoc", "ISCC 2025 paper accepted", "KDIR 2023 Best Paper"), sourced from `_data/news.yml` (NOT `_posts/` and NOT a `_news/` collection — the absence of any post-style file structurally enforces "no blog").
**Plans**: 10 plans
- [ ] 02-01-PLAN.md — Purge demo content (grep gate)
- [ ] 02-02-PLAN.md — Landing page: bio, Now/Recently/Previously widget, news widget include
- [ ] 02-03-PLAN.md — Headshot resize and avatar wiring
- [ ] 02-04-PLAN.md — Publications (8 entries + BibTeX include + KDIR Best Paper badge)
- [ ] 02-05-PLAN.md — Talks (conference + chaired sessions + career)
- [ ] 02-06-PLAN.md — Teaching and outreach entries
- [ ] 02-07-PLAN.md — Portfolio (4 projects)
- [ ] 02-08-PLAN.md — _data/news.yml seed entries
- [ ] 02-09-PLAN.md — Navigation: remove CV and Blog Posts entries
- [ ] 02-10-PLAN.md — Author sidebar links (ORCID/Scholar/GitHub/LinkedIn/Oxford) + iframe audit
**UI hint**: yes

---

### Phase 3: Dark Theme & Customisation
**Goal**: The entire site renders dark on every page with an Oxford-blue accent — no white flashes, no light-mode UI leaks, no theme toggle anywhere — and mobile browser chrome matches the dark palette.
**Depends on**: Phase 2 (real content must be visible to evaluate whether the palette actually works against real publication titles, venues, and citation strings)
**Requirements**: THEME-01, THEME-02, THEME-03, THEME-04, THEME-05, THEME-06, THEME-07
**Success Criteria** (what must be TRUE):
  1. Hard-reloading the landing page, publications page, an individual publication detail page, the talks page, and the search overlay produces no white flash and no white background on any UI element (tables, inline `<code>`, fenced code blocks, form controls, author sidebar borders, archive items, footer, masthead all dark).
  2. Links, header rules, and button highlights render in Oxford blue (`#002147`) — overridden via a forked Sass partial `_sass/_harry_dark.scss` (NOT by editing anything under `_sass/minimal-mistakes/`).
  3. View-source on any page contains `<meta name="theme-color" content="...">` matching the dark background colour, so iOS Safari and Android Chrome address bars render dark — verified on a real phone or DevTools device emulator.
  4. There is no theme-toggle UI anywhere (masthead, sidebar, footer) and `grep -r "prefers-color-scheme: light" _sass/ assets/` returns zero matches — dark is unconditional, not preference-driven.
  5. Code blocks render against a dark Rouge syntax theme (e.g. monokai or base16.dark) — no light syntax-highlighting background visible on any publication page or markdown sample.
**Plans**: TBD
**UI hint**: yes

**Local dev requirement:** Iterating Sass variables via the 30-60 s push-and-rebuild loop is impractical for this phase. **GitHub Codespaces (using the academicpages-shipped `.devcontainer/`) or a local Ruby 3.3.4 + `bundle exec jekyll serve --livereload` setup is REQUIRED** for tight iteration. Phase 1's Codespaces verification (DEPLOY-05) is the foundation for this.

---

### Phase 4: Polish & Ship
**Goal**: The site is shareable on social/Slack with a real link preview, indexable by Google with correct metadata, has zero broken links, hits Lighthouse mobile ≥ 90, hides the email from naive scrapers, removes any "Download CV" surface entirely, and shows a build-driven `last_updated` footer.
**Depends on**: Phase 3
**Requirements**: CONTACT-01, POLISH-01, POLISH-02, POLISH-03, POLISH-04, POLISH-05, POLISH-07, POLISH-08, POLISH-09
**Success Criteria** (what must be TRUE):
  1. Running `bundle exec htmlproofer ./_site --disable-external false` returns **zero broken internal links and zero broken images** — and `/cv/`, `/cv`, and any "Download CV" nav link return 404 (i.e. the CV surface is removed entirely, NOT stubbed as "coming soon").
  2. Lighthouse mobile audit on the landing page scores **≥ 90 on Performance, Accessibility, and SEO** (run via Chrome DevTools or `lhci` against the deployed site), and the layout has no horizontal scroll or overflowing text at a 375px-wide viewport.
  3. Pasting the canonical URL into LinkedIn's Post Inspector (or opengraph.xyz) renders a real OG card with Harry's name, headshot/branded image, and description — favicon also visible in browser tabs (not the academicpages default globe).
  4. View-source on the deployed homepage shows: a site `<title>`, meta description, author tag, `og:image`, ORCID structured data, AND **no plain-text `mailto:Harry.Rogers@eng.ox.ac.uk`** (the email surfaces only via HTML-entity or JS obfuscation — verified by searching the raw HTML for the literal address string).
  5. The footer displays a `last_updated` line driven by `{{ site.time | date: "%B %Y" }}` (Liquid, build-date driven) — not a hard-coded string — and updates automatically on the next push.
**Plans**: TBD
**UI hint**: yes

---

## Progress

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Bootstrap & Deploy | 0/5 | Not started | - |
| 2. Content Population | 0/? | Not started | - |
| 3. Dark Theme & Customisation | 0/? | Not started | - |
| 4. Polish & Ship | 0/? | Not started | - |

## Phase Dependency Graph

```
Phase 1: Bootstrap & Deploy
        │
        ▼
Phase 2: Content Population
        │
        ▼
Phase 3: Dark Theme & Customisation
        │
        ▼
Phase 4: Polish & Ship  ──▶  v1 ship
```

Strictly sequential. No parallelisation opportunity at this scale.

---
*Last updated: 2026-05-17 after initialization*
