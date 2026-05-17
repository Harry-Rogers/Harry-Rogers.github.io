---
phase: 02-content-population
plan: 02
subsystem: landing-page
tags: [content, bio, widgets, includes]
requires:
  - 02-01
  - 02-03
  - 02-08
provides:
  - landing-page-bio
  - now-recently-previously-widget
  - news-widget-include
affects:
  - _pages/about.md
  - _includes/now-recently-previously.html
  - _includes/news-widget.html
tech-stack:
  added: []
  patterns:
    - "Liquid {% include %} for reusable widget components"
    - "site.data.news iteration with limit:5 (no re-sort; owner authors in reverse-chrono order)"
    - "markdown=\"0\" attribute on wrapping divs to prevent kramdown re-processing"
key-files:
  created:
    - _includes/now-recently-previously.html
    - _includes/news-widget.html
  modified:
    - _pages/about.md
decisions:
  - "Plain inline HTML divs for the Now/Recently/Previously widget — no custom Sass (Phase 3 owns theming)"
  - "News widget renders site.data.news directly in author-listed order rather than sorting by date string — avoids constraining the owner-facing date format in _data/news.yml"
  - "Both 'Recently' and 'Previously' blocks link to /publications/ collection page (BIO-03) rather than deep-linking to individual permalinks, so the widget stays stable as publications evolve"
requirements:
  - BIO-01
  - BIO-02
  - BIO-03
metrics:
  duration: "~5 min"
  completed: "2026-05-17"
---

# Phase 02 Plan 02: Landing Page Bio + Widgets Summary

Rewrote `_pages/about.md` as Harry's real landing page (identity, role, supervisor, 4-paragraph bio framed around human-AI collaboration in medical AI) and added two reusable Liquid includes: a three-block Now/Recently/Previously research-framing widget and a news widget that iterates `site.data.news` with `limit:5`.

## What Was Built

### `_pages/about.md`
- Front matter preserved (`permalink: /`, `author_profile: true`, `redirect_from`); title changed from template placeholder to "About".
- Paragraph 1 — identity opener (PRA, IBME Oxford, Noble group).
- Paragraph 2 — current research framing, contains the literal phrase `human-AI collaboration in medical AI`.
- Paragraph 3 — prior medical-imaging work (Clock Drawing Test EPIA 2024, Home Spirometry IEEE ISCC 2025).
- Paragraph 4 — DPhil background (AgriFoRwArdS CDT, UEA, Syngenta) + earlier degrees.
- Two `{% include %}` lines under `## Now / Recently / Previously` and `## News` H2 headings.
- `<!-- TODO(owner): refine -->` markers on paragraphs 3 and 4 for owner verification.
- No raw `mailto:` (CONTACT-01 is Phase 4).

### `_includes/now-recently-previously.html`
Three labelled blocks (Now / Recently / Previously) mapping medical-education → medical-imaging → precision-agriculture trajectory. Recently and Previously blocks each link to `/publications/` (BIO-03). Plain HTML with `markdown="0"` wrapper so kramdown leaves it alone. No inline styles — Phase 3 owns theme.

### `_includes/news-widget.html`
Liquid block: `{% for entry in site.data.news limit:5 %}` rendering `<strong>{{ entry.date }}</strong> — {{ entry.item }}` per `<li>`. Empty-case handled with an `{% else %}` "No news yet." fallback. Matches the schema in `_data/news.yml` (which has 6 entries from Plan 02-08).

## Verification

All acceptance criteria from the plan passed:

| Check | Result |
|-------|--------|
| `_pages/about.md` contains "human-AI collaboration in medical AI" | pass |
| `_pages/about.md` contains "Postdoctoral Research Associate" | pass |
| `_pages/about.md` contains "Alison Noble" | pass |
| `_pages/about.md` contains "AgriFoRwArdS" | pass |
| `_pages/about.md` references both includes | pass |
| `_pages/about.md` preserves `permalink: /` | pass |
| `_pages/about.md` has no `mailto:` | pass |
| Now / Recently / Previously H3 blocks exist | pass |
| `/publications/` linked from ≥ 2 blocks | pass |
| News include references `site.data.news` with `limit:5` and `entry.date`/`entry.item` | pass |

## Deviations from Plan

None — plan executed exactly as written.

## Known Stubs

Two `<!-- TODO(owner): refine -->` markers in `_pages/about.md` flag paragraphs that synthesise PROJECT.md narrative; owner should verify wording matches preferred phrasing before public launch. These are content refinements, not blocking stubs.

## Threat Flags

None — this plan only authors content/templates, no new network endpoints, auth paths, or trust boundaries.

## Commits

- `5916fc2` — feat(02-02): author landing page bio with Now/Recently/Previously and news widgets

## Self-Check: PASSED

- `_pages/about.md` exists and contains all required strings (verified).
- `_includes/now-recently-previously.html` exists with three H3 blocks and ≥ 2 `/publications/` links (verified).
- `_includes/news-widget.html` exists and references `site.data.news` with `limit:5` (verified).
- Commit `5916fc2` present on `master` (verified via `git rev-parse --short HEAD`).
