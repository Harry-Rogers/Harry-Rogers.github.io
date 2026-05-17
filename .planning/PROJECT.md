# Harry Rogers — Academic Website

## What This Is

A personal academic website for **Harry Rogers**, Postdoctoral Research Associate at the Institute of Biomedical Engineering, University of Oxford. The site is a single canonical place for anyone (peers, students, recruiters, journalists) who lands on it to learn who he is, see his published work and talks, find his teaching, download his CV, and contact him.

The site is built by forking the [academicpages](https://github.com/academicpages/academicpages.github.io) Jekyll template (fork-then-customise rather than build-from-scratch), deployed via **GitHub Pages** at `https://harry-rogers.github.io` from the repo `Harry-Rogers/Harry-Rogers.github.io`.

## Core Value

> When someone Googles "Harry Rogers Oxford" and lands on this site, within ten seconds they can tell **who he is, what he works on, and how to reach him** — and within thirty seconds they can find his papers, talks, and CV.

Everything else is secondary.

## Owner Profile

| Field | Value |
|---|---|
| Name | Harry Rogers |
| Current role | Postdoctoral Research Associate |
| Affiliation | Institute of Biomedical Engineering (IBME), University of Oxford |
| Group | Biomedical Image Analysis (Prof. Alison Noble) |
| Research focus | Human-AI collaboration in medical AI broadly — current area: AI in medical education; prior medical-AI work in imaging (clock-drawing test, home spirometry) |
| Prior interests | Computer vision, deep learning, explainable AI (XAI), robotics, precision agriculture |
| PhD | DPhil/PhD — University of East Anglia, AgriFoRwArdS CDT, supervised by Prof. Beatriz De La Iglesia, in collaboration with Syngenta. Thesis: *"Closing The Loop on Precision Spraying"* (started Sept 2020) |
| MSc | Robotics and Autonomous Systems — UEA (Syngenta-linked). Project: *"An Empirical Comparison of Optimisation Methods for Embedded DNNs"* |
| BSc | Computer Science — University of Lincoln (dissertation on agricultural robotics, seed dispensing + GPS) |
| Email | Harry.Rogers@eng.ox.ac.uk |
| ORCID | [0000-0003-3227-5677](https://orcid.org/0000-0003-3227-5677) |
| Google Scholar | [sPwcwvMAAAAJ](https://scholar.google.com/citations?user=sPwcwvMAAAAJ) — 37 citations, h-index 3, i10-index 1 |
| GitHub | [Harry-Rogers](https://github.com/Harry-Rogers) |
| LinkedIn | [in/harry-rogers-ox](https://www.linkedin.com/in/harry-rogers-ox) |
| Oxford page | https://eng.ox.ac.uk/people/harry-rogers |
| Awards | Best Paper @ KDIR 2023; Best Application nomination @ TAROS 2023 |

### Published Work (known at project init — to verify against Scholar at content phase)

1. Rogers et al. *"An open source seeding agri-robot"* (UK-RAS 2020) — 15 citations
2. Rogers et al. *"Advancing precision agriculture: domain-specific augmentations and robustness testing..."* (Neural Computing and Applications, 2024) — 9 citations
3. Rogers et al. *"An automated precision spraying evaluation system"* (TAROS 2023) — 4 citations
4. Rogers et al. *"An agricultural precision sprayer deposit identification system"* (IEEE CASE 2023) — 3 citations
5. Rogers et al. *"Evaluating the use of interpretable quantized convolutional neural networks..."* (KDIR 2023) — 2 citations, Best Paper
6. Gardiner, Lines, Wilson, Aung, **Rogers** *"Quality Assurance for Home Spirometry using Machine Learning"* (IEEE ISCC 2025) — 1 citation
7. Rogers et al. *"Deep Learning for Precision Agriculture: Post-Spraying Evaluation..."* (arXiv, 2024)
8. Mayne, Sami, la Iglesia, **Rogers** *"Automating the Clock Drawing Test with Deep Learning and Saliency Maps"* (EPIA 2024)

## Constraints

- **Host:** GitHub Pages (free tier, Jekyll builds supported natively). Repo must be named exactly `Harry-Rogers.github.io` to deploy at `https://harry-rogers.github.io/`.
- **Brownfield: the repo already exists and is live.** The current site is a stale 2020 hand-rolled HTML/CSS single-page site from Harry's PhD-era. Owner has decided to **full-replace** without salvaging old content — the new academicpages-based site overwrites everything via force-push. Photo headshot (`assets/images/profile-original.jpg`, 4489×6734 Nikon Z 8 JPEG, 6.3 MB) is already staged locally; needs resizing/compression in Phase 2.
- **Template:** Forked from `academicpages/academicpages.github.io` (Jekyll + Minimal Mistakes). Stay close to the template to keep maintenance low; only deviate where it adds personal character.
- **Stack:** Jekyll (Ruby), Liquid templating, Sass, vanilla JS. No JS framework, no build tooling beyond what GitHub Pages runs.
- **Dark mode only.** No light-mode theme. The default academicpages light theme must be replaced.
- **No blog.** Strictly static pages. A *News* feed is allowed but limited to academic milestones (papers accepted, talks given, awards, joining/leaving institutions) — no opinion posts, no "thoughts" posts ("blogs feel cringy").
- **CV file:** Skipped for v1. The "Download CV" link is deferred — site should not include a broken link.
- **Maintenance model:** Owner edits Markdown files directly and commits to `main`; GitHub Pages auto-rebuilds. No CI required beyond the default Pages build.

## Requirements

### Validated

(None yet — ship to validate)

### Active

- [ ] Site builds and deploys at https://harry-rogers.github.io via the default GitHub Pages workflow
- [ ] Landing page introduces Harry with photo, current role, research framing (human-AI collaboration in medical AI, currently medical education)
- [ ] Contact details surfaced on landing page (email, Oxford affiliation, ORCID, Scholar, GitHub, LinkedIn)
- [ ] Papers page lists all publications with BibTeX, sourced from a single `.bib` / `_publications` data source and auto-rendered
- [ ] Talks page lists invited talks and conference presentations
- [ ] Teaching page lists teaching activities and outreach (AgriFoRwArdS workshops, Ormiston Victory career talk, Lego Mindstorms outreach, mentoring)
- [ ] Projects/portfolio page highlights notable research projects (precision spraying, clock drawing test, home spirometry, PiCar)
- [ ] Dark-mode-only theme applied site-wide (no light mode toggle)
- [ ] News section showing milestone updates only (papers accepted, talks, awards); no personal blog
- [ ] Site is responsive and readable on mobile
- [ ] Personal differentiator: at least one small visual/structural touch that makes it feel like Harry's site, not a generic academicpages fork

### Out of Scope (v1)

- **Blog / personal essays** — explicitly rejected ("blogs feel cringy"); only milestone-based news entries
- **Light mode / theme toggle** — dark-only by decision
- **CV PDF download** — deferred; no broken link in v1
- **Comments, analytics dashboards, newsletter signup** — not needed for an academic site
- **Custom domain** — `harry-rogers.github.io` is the canonical URL; no `harryrogers.com` purchase planned
- **Site search** — academicpages ships with basic search; not a v1 requirement to extend
- **Multi-language support** — English only

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Fork **academicpages** rather than al-folio or hand-roll | User preferred academicpages aesthetic; fork gives papers/talks/teaching/CV out of the box; lowest friction to a polished site | — Pending |
| Repo named **Harry-Rogers.github.io** (user site) | Deploys at clean URL `https://harry-rogers.github.io` with no project subpath; most professional for an academic | — Pending |
| **No blog, news only for academic milestones** | Owner explicitly finds blogs cringy; news section keeps the site feeling alive without forcing opinion content | — Pending |
| **Dark mode only** (no toggle) | Owner preference; also a small personality differentiator from default academicpages | — Pending |
| **CV download deferred** to a later milestone | Owner doesn't have a CV PDF ready; better to ship without a broken link than block on it | — Pending |
| **Research framing: human-AI collaboration in medical AI broadly, with medical education called out as current focus** | Reconciles owner's stated focus (medical education) with existing public framing (medical imaging) | — Pending |
| **Local dev via GitHub Codespaces** | Zero-install, academicpages ships a `.devcontainer/`; needed for tight iteration during dark-mode/Sass work | — Pending |
| **Publications hand-authored as Markdown** (no `jekyll-scholar`, no `pubsFromBib.py`) | 8 papers doesn't justify a toolchain; `jekyll-scholar` would force a custom GitHub Action and break the "no CI" decision | — Pending |
| **News stored in `_data/news.yml`** (no `_posts/`, no `_news/` collection) | Structural enforcement of "no blog" — no place to write a paragraph means no temptation to blog | — Pending |
| **Dark-theme accent colour: Oxford blue (#002147)** | Subtle nod to current affiliation; distinctive vs default teal | — Pending |
| **Full-replace the existing live site, no salvage** | Existing content is 2020-era and entirely superseded by Oxford/Scholar/CDT sources we already scraped; salvaging would only delay shipping | — Pending |
| **Photo: `assets/images/profile-original.jpg`** (6.3 MB Nikon raw export, source of truth) | Resized derivatives generated in Phase 2; never serve the original | — Pending |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd-transition`):
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone** (via `/gsd:complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---
*Last updated: 2026-05-17 after initialization*
