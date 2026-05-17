---
phase: 02-content-population
plan: 04
subsystem: publications
tags: [content, publications, bibtex, academicpages]
requires:
  - 02-01
provides:
  - 8 hand-authored publication markdown files in _publications/
  - _includes/bibtex-block.html (collapsible BibTeX render)
  - PUBS-01..05 satisfied
affects:
  - /publications/ listing page
  - per-publication permalink pages under /publication/*
tech-stack:
  added: []
  patterns:
    - "Hand-authored publication .md per academicpages convention (no jekyll-scholar)"
    - "Liquid include reads page.bibtex front matter into <details><pre><code> block"
key-files:
  created:
    - _includes/bibtex-block.html
    - _publications/2020-04-01-uk-ras-seeding-agri-robot.md
    - _publications/2023-04-01-taros-precision-spraying-evaluation.md
    - _publications/2023-08-01-ieee-case-deposit-identification.md
    - _publications/2023-11-01-kdir-interpretable-quantized-cnn.md
    - _publications/2024-02-01-ncaa-domain-specific-augmentations.md
    - _publications/2024-09-01-epia-clock-drawing-test.md
    - _publications/2024-10-01-arxiv-post-spraying-evaluation.md
    - _publications/2025-06-08-ieee-iscc-spirometry-qa.md
  modified:
    - _pages/publications.html
decisions:
  - "BibTeX rendered via Liquid include from front-matter `bibtex:` block — no jekyll-scholar, per CLAUDE.md."
  - "KDIR Best Paper Award surfaced via `awards:` front matter + visible 🏆 badge in body."
  - "Where DOIs/URLs unknown, `paperurl: \"\"` and TODO(owner) markers — no fabrication."
metrics:
  duration: ~5min
  completed: 2026-05-17
---

# Phase 2 Plan 4: Publications Content Summary

Authored all 8 publications from PROJECT.md as academicpages-compatible markdown files in `_publications/`, with Harry bolded as `**Rogers, H.**` in every entry, a hand-authored BibTeX entry per paper, and a new `_includes/bibtex-block.html` Liquid include that renders each entry's `page.bibtex` inside a collapsible `<details>/<pre><code>` block.

## What Was Built

- **8 publication markdown files** with full front matter: title, collection, permalink, excerpt, date, venue, authors (Harry bolded), paperurl (empty + TODO where unknown), citations, awards, hand-authored BibTeX.
- **`_includes/bibtex-block.html`** — small Liquid include rendering `page.bibtex` in a collapsible `<details>` block. Used by every publication via `{% include bibtex-block.html %}`.
- **KDIR 2023** entry carries `awards: "Best Paper Award"` plus a visible 🏆 badge.
- **TAROS 2023** entry carries `awards: "Best Application Nomination"` (per owner profile).
- **`_pages/publications.html`** — verified reverse-chronological listing via `site.publications reversed` (already correct in academicpages default); added a comment marker confirming PUBS-01.

## Verification

All acceptance criteria pass:
- `ls _publications/*.md | wc -l` → **8** ✓
- `grep -l "\*\*Rogers, H\.\*\*" _publications/*.md | wc -l` → **8** ✓
- `grep -l "Best Paper Award" _publications/2023-11-01-kdir-interpretable-quantized-cnn.md` → match ✓
- `grep -h "^collection: publications" _publications/*.md | wc -l` → **8** ✓
- `grep -h "include bibtex-block.html" _publications/*.md | wc -l` → **8** ✓
- No fabricated DOIs — all unknown URLs use `paperurl: ""` ✓
- No `lorem`/`paper title number`/`john doe` introduced (Phase 2 SC#1 green) ✓

## TODO(owner) Markers Left for Review

Each publication file has explicit `<!-- TODO(owner): ... -->` markers Harry should sweep in one pass. Summary:

| File | Open TODOs |
|---|---|
| `2020-04-01-uk-ras-seeding-agri-robot.md` | verify abstract; add UK-RAS 2020 proceedings URL |
| `2023-04-01-taros-precision-spraying-evaluation.md` | verify abstract; add TAROS 2023 URL |
| `2023-08-01-ieee-case-deposit-identification.md` | verify abstract; add IEEE CASE 2023 DOI/URL |
| `2023-11-01-kdir-interpretable-quantized-cnn.md` | verify abstract; add KDIR 2023 proceedings DOI/URL |
| `2024-02-01-ncaa-domain-specific-augmentations.md` | verify abstract; add Neural Computing and Applications DOI |
| `2024-09-01-epia-clock-drawing-test.md` | verify abstract; add EPIA 2024 DOI/URL; confirm full author list + order |
| `2024-10-01-arxiv-post-spraying-evaluation.md` | verify abstract; add arXiv ID/URL |
| `2025-06-08-ieee-iscc-spirometry-qa.md` | verify abstract; add IEEE ISCC 2025 DOI/URL; confirm full co-author first names + order |

Abstracts were drafted from PROJECT.md context only and need verification against the published versions. Citation counts came from PROJECT.md; date precision is month-only (day defaulted to `01`, except IEEE ISCC where `2025-06-08` reflects published proceedings date guess — owner to verify).

## Deviations from Plan

None — plan executed as written. Authors for joint-first/non-first papers (EPIA 2024, IEEE ISCC 2025) were transcribed from PROJECT.md surnames only; flagged as TODO(owner) for first-name/order confirmation.

## Self-Check: PASSED

- `_includes/bibtex-block.html` exists ✓
- All 8 `_publications/*.md` files exist ✓
- Commit `0dbd12b` exists in `git log --oneline` ✓
