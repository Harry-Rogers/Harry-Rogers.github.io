# Feature Research

**Domain:** Academic personal website — CS/ML postdoc (medical AI, human-AI collaboration)
**Researched:** 2026-05-17
**Confidence:** HIGH (academicpages defaults verified against official template + wiki; differentiator/credibility patterns synthesised from 2025 Best Personal Academic Websites contest, Nature Index, and medical-AI transparency literature)

## Feature Landscape

### Table Stakes (Users Expect These)

Features a peer/recruiter/journalist landing on `harry-rogers.github.io` will assume are present. Missing any of these makes the site feel half-built. All are either shipped by academicpages out of the box or require trivial content authoring.

| Feature | Why Expected | Complexity | Out of Box? | Notes |
|---|---|---|---|---|
| Landing page with photo, name, current role, one-paragraph research framing | First-impression "who is this person" within 10s — explicit Core Value | LOW | Yes (academicpages `about.md`) | Replace template copy; needs a real headshot |
| Affiliation + institutional links (Oxford IBME, Noble group) | Signals legitimacy; reviewers/hiring committees check this | LOW | Yes (sidebar via `_config.yml` `author:` block) | Add IBME logo or link in author sidebar |
| Contact block: email, ORCID, Google Scholar, GitHub, LinkedIn | Standard ways peers reach out / verify identity | LOW | Yes (`_config.yml` social icons) | All 5 IDs already known (see PROJECT.md) |
| Publications page rendered from a single source of truth | Researchers expect a complete, sortable paper list; expect to be able to grab a citation | MEDIUM | Partial — academicpages ships `_publications/*.md` rendering, **but BibTeX rendering is not automatic** | Either author Markdown per paper (academicpages default) OR add `jekyll-scholar` (GitHub Pages doesn't run it natively → needs GitHub Actions build, or pre-rendered HTML). Owner has chosen "BibTeX auto-render" so this is the highest-effort table-stakes item |
| Per-paper links: PDF, DOI, arXiv, code, BibTeX copy | Reviewers want one-click access to artefacts | LOW | Partial — academicpages publication template has `paperurl`, but no BibTeX-copy button | Add a small "Copy BibTeX" snippet block per entry (vanilla JS, ~20 lines) |
| Talks page in reverse-chronological order | Standard academic CV section; signals activity | LOW | Yes (`_talks/*.md`) | Owner has invited-talk content to add |
| Teaching page in reverse-chronological order | Hiring committees check teaching evidence | LOW | Yes (`_teaching/*.md`) | Includes outreach (AgriFoRwArdS, Ormiston Victory, Lego Mindstorms, mentoring) |
| Projects / portfolio with at least 3–4 highlighted items | Recruiters/students want quick "what does he build" view; differentiator for applied researchers | LOW | Yes (`_portfolio/*.md`) | Precision spraying, clock-drawing test, home spirometry, PiCar |
| Responsive / mobile layout | ~40% of academic site traffic is mobile (peers reading on phones during conferences) | LOW | Yes (Minimal Mistakes is responsive) | Verify dark-theme overrides don't break mobile |
| Working internal links + no broken external links | Broken links destroy credibility instantly | LOW | N/A | Run html-proofer in CI or pre-deploy; especially avoid the "Download CV" link being present-but-broken (PROJECT.md explicitly defers CV) |
| Favicon and `<title>` tag set correctly | Tabs/bookmarks/search snippets show institution-quality framing | LOW | Yes (`_config.yml`) | Replace academicpages defaults |
| Sensible SEO: meta description, OpenGraph image, sitemap.xml | Google "Harry Rogers Oxford" must surface this site, not an old PhD page | LOW | Yes (`jekyll-seo-tag`, `jekyll-sitemap` both supported on GH Pages) | Add OG image (headshot + name card) |
| Accessible typography: contrast, font size, focus rings | Dark-mode-only design must still hit WCAG AA contrast | LOW | Partial — needs care because owner is overriding the default light theme | See PITFALLS for dark-mode contrast traps |

### Differentiators (Memorable vs Generic academicpages Fork)

Active requirement from PROJECT.md: *"at least one small visual/structural touch that makes it feel like Harry's site, not a generic academicpages fork."* These are concrete options. Pick 2–3 — too many and the site feels gimmicky.

| Feature | Value Proposition | Complexity | Out of Box? | Notes / Why It Fits Harry |
|---|---|---|---|---|
| **Research-area framing on landing page** — three short blocks: "Now: Human-AI collaboration in medical education" / "Recently: Medical imaging (clock-drawing, spirometry)" / "Previously: Precision agriculture (PhD)" | Resolves the imaging-vs-education tension explicitly; tells the *story* of a career rather than dumping a paper list. The 2025 best-academic-sites contest specifically praises "telling the story of research" | LOW | No — custom Liquid section in `about.md` | Highest-leverage differentiator; near-zero implementation cost; directly addresses Key Decision in PROJECT.md about reconciling research framing |
| **Per-paper "TL;DR" one-liner** above the citation | Lets a busy reader skim 8 papers in 30s; makes the publication page useful, not just complete | LOW | No — add a `tldr:` front-matter field to `_publications/*.md` and render above citation | Cheap to author (Harry writes 1 sentence × 8 papers); huge readability uplift |
| **Paper grouping by theme/topic** rather than just year (e.g. "Medical AI", "Precision agriculture", "Explainability") | Contest winners explicitly praised "grouping papers and describing the relevance of each grouping". Helps a visitor understand the *coherence* of the research, not just the chronology | LOW | No — Jekyll permits grouping by front-matter field with a few lines of Liquid | Pairs well with research-area framing on the landing page |
| **Embedded talk videos** (YouTube/Vimeo iframe) on talk entries | Most academic talk pages list titles only; embedding the actual recording is rare and memorable. Signals "real speaker, not just a CV padder" | LOW | No, but trivial — `_talks/*.md` already supports a body, drop an iframe | Only useful if Harry has recorded talks; if not, omit |
| **Downloadable poster PDFs** per paper/project where available | Conference posters are usually thrown away after the event. Hosting them is rare, useful for collaborators, and signals reproducibility-mindedness | LOW | No — host PDFs in `/files/`, link from publication front-matter | Pairs with `tldr:` field |
| **Dark-mode-only palette with a single accent colour tied to Oxford** (e.g. Oxford blue `#002147` on near-black) | Owner already chose dark-mode-only; pushing it one step further to a curated palette (rather than "default dark theme") is the cheapest personality lever | LOW | No — override Minimal Mistakes Sass variables | Personality without gimmick; also satisfies the "at least one visual touch" requirement on its own |
| **Custom monospace/serif heading font** loaded locally (no Google Fonts CDN — privacy + offline) | Default Minimal Mistakes typography is instantly recognisable to anyone in academia. A swap to e.g. Inter + IBM Plex Mono, self-hosted, says "I noticed the defaults" | LOW | No — drop font files in `/assets/fonts/`, override `_sass/` variables | Privacy-respecting (no Google Fonts means no IP logging) — minor credibility signal for an ML researcher |
| **News feed limited to milestones** (paper accepted, talk given, award, role change) with date + one sentence + link | Owner explicitly wants this and explicitly rejects blog. A constrained news feed signals "active researcher" without forcing opinion content | LOW | Yes — academicpages has `_posts/` (the blog); repurpose it as a `_news/` collection with strict scope | This is the explicit substitute for a blog — see Anti-Features |
| **Citation count + "see on Scholar" link per paper**, hand-updated yearly | Reviewers/hiring committees genuinely look at this. Scholar API isn't usable from a static site, but a manually-maintained number with a "last updated YYYY-MM" caveat is honest and useful | LOW | No — front-matter field `citations:` rendered as a badge | Low maintenance: update once a year |
| **A "research at a glance" diagram** on the landing page (simple SVG, hand-authored) showing the through-line from precision agriculture → medical imaging → medical education | Visual storytelling; contest winners praised "engaging visuals". Sets the site apart from text-only academicpages forks | MEDIUM | No — author by hand in `assets/images/research-map.svg` | Higher payoff but riskier — bad diagram is worse than no diagram. Defer to a later phase once the site is up |
| **Reproducibility badges per paper** ("Code", "Data", "Pre-registered", "Pre-print") | High-credibility signal in ML and an *especially* high signal in medical AI where reproducibility is openly debated in the literature | LOW | No — front-matter list rendered as small pill badges | Maps directly to medical-AI credibility signals; cheap to implement and Harry has open code for several papers (precision spraying repos on GitHub) |
| **PGP / age public key + verification statement** linked from contact | Cryptographic identity verification is rare and memorable; signals security/CS literacy. Optional — skip if not already maintaining a key | LOW | No — static page + link to keyserver | Genuinely optional; only include if Harry already uses one |

### Domain-Specific Credibility Signals (Medical AI / Human-AI Collaboration)

These are not generic differentiators — they specifically signal credibility in *medical AI* and *human-AI collaboration* research, where reproducibility and ethics are actively-debated topics in the literature (Nature 2023; Springer 2025; JMIR 2025).

| Feature | Why It Signals Credibility | Complexity | Out of Box? | Notes |
|---|---|---|---|---|
| **Reproducibility links per paper** — explicit `code:`, `data:`, `preprint:` fields rendered as visible links/badges | The "reproducibility issues that haunt healthcare AI" (Nature, 2023) are widely-discussed; visibly linking artefacts is the strongest single signal | LOW | No — front-matter additions to `_publications/*.md` | Highest-ROI credibility signal; combine with badges above |
| **Brief ethics / responsible-AI statement** on landing page or a dedicated `/ethics/` page (3–5 sentences: data sources, IRB approval where relevant, limitations, intended use) | Trustworthy-AI frameworks (TAXAI 2026; JMIR 2025) cite intended-use disclosure as a transparency dimension. Patients/clinicians/funders increasingly look for this | LOW | No — single Markdown page | Keep it short and specific; long ethics statements read as performative |
| **"Datasets I've worked with" page or section** linking to dataset cards (or describing the dataset where not public — e.g. the home spirometry dataset, clock-drawing data) | Datasheets-for-datasets is a Gebru-et-al norm in ML; mentioning it positions Harry on the right side of the data-transparency conversation | MEDIUM | No — custom page; one entry per dataset | Defer past v1 if dataset descriptions need clearance |
| **Funding / disclosure footer** (UKRI/EPSRC/AgriFoRwArdS CDT, Syngenta collaboration, Oxford IBME) | Standard for medical-AI publications; including it on the site signals "I treat conflicts of interest seriously" | LOW | No — footer partial in `_includes/footer.html` | One sentence is sufficient |
| **Talk/poster videos from medical-AI venues** (e.g. EPIA 2024 clock-drawing presentation, IEEE ISCC 2025 spirometry) embedded on `_talks/*.md` | Demonstrates the human-side of human-AI collaboration work — Harry actually presents to clinical audiences | LOW | No — iframe in talk body | Same as embedded-talk-videos differentiator; called out separately because for medical-AI work this *is* a credibility signal, not just a flourish |
| **Collaborator / co-author list** on each publication entry, with affiliations (Noble group, Lines, Wilson, Aung, Sami, De La Iglesia, Syngenta) | Network legibility — reviewers reading a paper page see this is real interdisciplinary medical-AI work, not solo CS bench-marking | LOW | Yes — academicpages publication template supports `authors:` field | Just author it carefully |
| **Best-paper / award badges** on the relevant publication entries (Best Paper @ KDIR 2023; Best Application nomination @ TAROS 2023) | Awards are credibility shorthand; surfacing them at the paper level (not just on a CV) is more discoverable | LOW | No — front-matter `award:` field + small badge component | Already known awards from PROJECT.md |

### Anti-Features (Explicitly NOT Building)

| Feature | Why Requested / Tempting | Why Problematic | Decision |
|---|---|---|---|
| **Blog / opinion posts / "thoughts" posts** | academicpages ships a working blog collection (`_posts/`); easy to leave it on | Owner explicitly rejects: *"blogs feel cringy"*; abandoned blogs (one post from 2024, then silence) actively *hurt* credibility; opinion content invites controversy with zero career upside for an early-career researcher | **Disable the `_posts/` route entirely.** Repurpose its rendering for a constrained News collection — milestone updates only |
| **Light mode / theme toggle** | Default academicpages is light-mode; a toggle is the "neutral" choice | Doubles the styling surface area; owner has decided dark-only is part of the personality | **Dark-mode only. Remove light-mode CSS rather than hide a toggle** — fewer bytes, fewer flashes-of-light on load |
| **CV PDF download** in v1 | Standard academic feature; academicpages ships a Download CV button | Owner doesn't have an up-to-date PDF; a broken or stale link is worse than no link | **Remove the Download CV button in v1.** Add the link only when a real PDF exists. Auto-generated CV from `_publications` etc. (academicpages' built-in feature) is also out of scope for v1 to keep maintenance load down |
| **Comments / Disqus / utterances** | Common on academic blogs | No blog in v1, so no surface for comments. Comments on a static academic site invite spam and harassment with little benefit | **Not built.** If a guestbook is ever wanted, link to LinkedIn/email instead |
| **Newsletter / email signup** | Substack-era academics sometimes ship one | No content cadence (no blog) to feed it; managing a mailing list = GDPR overhead | **Not built.** Subscribe-to-Scholar-alerts is the substitute |
| **Live analytics dashboard** (visible visitor counter, GitHub stars badge, etc.) | Looks "alive" | Cheapens the page; visitor counts are 2002-era; GitHub stars are noisy for an academic | **Not built.** Privacy-respecting analytics (e.g. GoatCounter, Plausible) are fine to *use* server-side; do not surface to visitors |
| **Custom domain `harryrogers.com`** | "More professional" | Already decided against in PROJECT.md. Adds renewal cost, DNS risk, and `harry-rogers.github.io` is already a strong canonical URL | **Not built.** Keep `harry-rogers.github.io` |
| **Site-wide search beyond academicpages default** | Bigger sites need it | Site is < 30 pages; browser Ctrl-F suffices. academicpages ships a basic search; that's enough | **Not built.** Leave the default search |
| **Multi-language support** | Common on EU researcher sites | English is the working language of the field and Oxford | **Not built.** English only |
| **Auto-fetched Scholar citation counts via scraper** | Tempting because Scholar has no API | Scholar actively blocks scrapers; CI builds will fail unpredictably; legal grey area | **Not built.** Use manual, dated citation counts ("as of 2026-05") if desired |
| **AI chatbot / "ask my papers" widget** | 2025 trend on personal sites | Hallucination risk on a *medical AI* researcher's site is uniquely bad PR; runtime/$$$ cost; vendor lock-in | **Not built.** A LLM that mis-summarises Harry's medical-AI work on Harry's own site is the worst possible failure mode |
| **Animated/parallax landing page, particle effects, typewriter intro** | "Modern" web aesthetic | Reads as personal-portfolio, not academic; slows mobile; ages badly | **Not built.** Dark theme + good typography is the personality |
| **Twitter/X feed embed** | Used to be standard on academic sites | X embeds break frequently; rate-limits; reputational risk in 2026 | **Not built.** Link to profile only if active |

## Feature Dependencies

```
Publications page (table stakes)
    ├── requires: BibTeX source decision (jekyll-scholar via GH Actions, OR per-paper Markdown)
    ├── enhances ── Per-paper TL;DR (differentiator)
    ├── enhances ── Reproducibility badges (medical-AI credibility)
    ├── enhances ── Award badges (medical-AI credibility)
    ├── enhances ── Citation counts (differentiator)
    └── enhances ── Paper grouping by theme (differentiator)

Landing page (table stakes)
    ├── enhances ── Research-area framing (differentiator)
    ├── enhances ── Ethics/responsible-AI statement (medical-AI credibility)
    └── enhances ── Research-at-a-glance diagram (differentiator, defer)

Talks page (table stakes)
    └── enhances ── Embedded talk videos (differentiator + medical-AI credibility)

Dark-mode-only theme (table stakes — given owner's decision)
    ├── requires: Sass override of Minimal Mistakes variables
    ├── enhances ── Custom accent palette (differentiator)
    └── conflicts ── Default academicpages light-mode CSS (must remove, not toggle)

News feed (table stakes per owner)
    ├── requires: Repurposing _posts/ collection as _news/ with strict editorial scope
    └── conflicts ── Blog (anti-feature) — same Jekyll collection, different framing

CV PDF download
    └── blocked-by: no real PDF exists yet → feature deferred entirely (no broken link)
```

### Dependency Notes

- **Publications + BibTeX**: Owner has explicitly required "BibTeX auto-render". On GitHub Pages' default build, the `jekyll-scholar` plugin is **not whitelisted** — so this means either (a) switch to a GitHub Actions build to run Jekyll with custom plugins, or (b) author each publication as a `_publications/*.md` with a hand-pasted BibTeX block. Decision belongs in STACK.md, but flag here: this is the single highest-effort table-stakes item and its complexity is a function of which approach.
- **News feed vs blog**: same Jekyll machinery; the distinction is purely editorial. Phrase the collection as `_news/` and constrain front-matter (no body field beyond ~3 sentences) to make blog-style posts mechanically awkward.
- **Dark-mode-only**: removing light-mode CSS rather than toggling it avoids the FOUC (flash-of-unstyled-content) flicker on slow connections.
- **CV PDF**: do not add a placeholder "Coming Soon" — leave the link absent entirely.

## MVP Definition

### Launch With (v1) — maps to PROJECT.md Active requirements

- [ ] Site builds + deploys at `https://harry-rogers.github.io`
- [ ] Landing page: photo, role, three-block research framing (now/recently/previously)
- [ ] Contact block: email, Oxford affiliation, ORCID, Scholar, GitHub, LinkedIn
- [ ] Publications page with all 8 known papers, each with: title, authors, venue, year, BibTeX (rendered or copy-button), links to PDF/DOI/arXiv/code where available, award badge where relevant
- [ ] Talks page (reverse-chrono)
- [ ] Teaching page (reverse-chrono, includes outreach)
- [ ] Projects/portfolio page with 3–4 highlighted items (precision spraying, clock drawing, home spirometry, PiCar)
- [ ] Dark-mode-only theme with custom accent palette (chosen differentiator)
- [ ] News collection scaffolded with ≥1 entry, blog disabled
- [ ] Responsive on mobile
- [ ] SEO basics (title, description, OG image, sitemap)
- [ ] No broken links (no "Download CV" link, no orphan pages)

### Add Soon After Launch (v1.x)

- [ ] Per-paper TL;DR one-liner
- [ ] Reproducibility badges per paper (code/data/preprint)
- [ ] Brief ethics / responsible-AI statement (landing page or `/ethics/`)
- [ ] Paper grouping by theme on publications page
- [ ] Embedded talk videos where recordings exist
- [ ] Downloadable poster PDFs for papers where available
- [ ] Manually-curated citation counts with "as of YYYY-MM" caveat

### Future Consideration (v2+)

- [ ] CV PDF download (once PDF is authored)
- [ ] "Research at a glance" SVG diagram on landing page
- [ ] Datasets page / dataset cards
- [ ] Funding/disclosure footer (low effort but better to land in v1.x once content is settled)
- [ ] PGP key publication
- [ ] Privacy-respecting analytics (server-side only, never user-facing)

## Feature Prioritisation Matrix

| Feature | User Value | Implementation Cost | Priority |
|---|---|---|---|
| Landing page + research framing | HIGH | LOW | **P1** |
| Publications with BibTeX | HIGH | MEDIUM | **P1** |
| Contact block | HIGH | LOW | **P1** |
| Talks page | MEDIUM | LOW | **P1** |
| Teaching page | MEDIUM | LOW | **P1** |
| Projects page | MEDIUM | LOW | **P1** |
| Dark-mode-only theme | MEDIUM (owner-driven) | LOW | **P1** |
| News collection (blog disabled) | MEDIUM | LOW | **P1** |
| SEO / OG / sitemap | HIGH | LOW | **P1** |
| Per-paper TL;DR | HIGH | LOW | **P2** |
| Reproducibility badges | HIGH (credibility) | LOW | **P2** |
| Ethics statement | MEDIUM (credibility) | LOW | **P2** |
| Paper grouping by theme | MEDIUM | LOW | **P2** |
| Embedded talk videos | MEDIUM | LOW | **P2** (if videos exist) |
| Award badges on papers | MEDIUM | LOW | **P2** |
| Custom typography | LOW | LOW | **P2** |
| Manually-curated citation counts | LOW | LOW | **P2** |
| Research-at-a-glance SVG | MEDIUM | MEDIUM (design risk) | **P3** |
| Dataset cards page | MEDIUM | MEDIUM | **P3** |
| CV PDF | HIGH (once exists) | LOW | **P3** (blocked on content) |
| PGP key | LOW | LOW | **P3** |
| Blog | NEGATIVE | — | **Anti-feature** |
| Light mode | NEGATIVE | — | **Anti-feature** |
| Chatbot / Scholar scraper / X embed | NEGATIVE | — | **Anti-feature** |

## Competitor / Reference Feature Analysis

| Feature | Default academicpages fork | al-folio (alt template) | Top 2025 contest winners | Harry's plan |
|---|---|---|---|---|
| Publications page | Markdown per entry | jekyll-scholar BibTeX | Mixed — many show TL;DR + topic grouping | Markdown per entry **plus** TL;DR + theme grouping + reproducibility badges |
| Theme story | Light, default Minimal Mistakes | Light/dark toggle | Custom palette + bespoke typography | Dark-only, custom accent, custom local fonts |
| Landing page | Single bio paragraph | Bio + news preview | Bio + research-areas blocks + visual element | Three-block research framing (now/recently/previously) |
| Blog/news | Blog by default | Blog by default | Often disabled, or news-only | News only; blog disabled |
| CV | Auto-generated from collections | PDF link | Both | Neither in v1 |
| Differentiator | None | None | Visual storytelling, content depth | Research framing + paper TL;DR + reproducibility badges + dark accent |

## Sources

- [Winners of the Best Personal Academic Websites Contest 2025 — The Academic Designer](https://theacademicdesigner.com/2025/winners-of-the-best-personal-academic-websites-contest-2025/)
- [Build-your-own academic website for scientists — Nature Index](https://www.nature.com/nature-index/news/build-your-own-academic-website-for-scientists-researchers-phd)
- [academicpages repo + customisation wiki](https://github.com/academicpages/academicpages.github.io/wiki/Customizing-the-content-on-your-site)
- [academicpages live demo](https://academicpages.github.io/)
- [al-folio Jekyll theme (reference comparator)](https://github.com/alshedivat/al-folio)
- [jekyll-scholar plugin (BibTeX bibliographies in Jekyll)](https://github.com/inukshuk/jekyll-scholar)
- [AI in Healthcare: Standardized Reporting for Reproducibility — Springer Nature, 2025](https://communities.springernature.com/posts/ai-in-healthcare-standardized-reporting-for-reproducibility-validity-and-clinical-impact)
- [The reproducibility issues that haunt health-care AI — Nature, 2023](https://www.nature.com/articles/d41586-023-00023-2)
- [Trust, Trustworthiness, and the Future of Medical AI — JMIR, 2025](https://www.jmir.org/2025/1/e71236)
- [Challenges of reproducible AI in biomedical data science — PMC, 2025](https://pmc.ncbi.nlm.nih.gov/articles/PMC11724458/)
- [Piloting Transparency Assessment for Medical AI Tools — PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC9601535/)

---
*Feature research for: academic personal website (CS/ML postdoc, medical AI)*
*Researched: 2026-05-17*
