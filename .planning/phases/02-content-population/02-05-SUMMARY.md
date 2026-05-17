---
phase: 02-content-population
plan: 05
subsystem: content/talks
tags: [content, talks, academicpages, jekyll]
requires:
  - 02-01 (academicpages scaffold; _talks/ collection wired)
provides:
  - _talks/ collection populated with 6 real entries
  - /talks/ listing renders reverse-chronologically
affects:
  - _pages/talks.html (annotation-only)
tech_stack:
  added: []
  patterns:
    - "Talk front matter: title, collection, type, permalink, venue, date, location, paper"
    - "TODO(owner) markers for unverified facts (dates, locations, presenters, slides)"
key_files:
  created:
    - _talks/2023-04-01-taros-precision-spraying.md
    - _talks/2023-11-01-kdir-interpretable-quantized.md
    - _talks/2024-09-01-epia-clock-drawing.md
    - _talks/2025-06-08-ieee-iscc-spirometry.md
    - _talks/2024-06-01-agriforwards-chair.md
    - _talks/2024-03-01-ormiston-victory-careers.md
  modified:
    - _pages/talks.html
decisions:
  - "Used placeholder month-precision dates (YYYY-MM-01) for events without confirmed exact dates; flagged each with TODO(owner)"
  - "Did not fabricate locations or slides URLs; left location empty for conference talks where city/country not in PROJECT.md"
  - "Included EPIA 2024 and IEEE ISCC 2025 as 'Conference proceedings talk' despite Harry being co-author (not first); flagged presenter as TODO"
metrics:
  duration: "~6 minutes"
  completed_date: "2026-05-17"
  task_count: 3
  file_count: 7
---

# Phase 02 Plan 05: Talks Content Summary

Authored 6 real talk entries in `_talks/` covering 4 conference paper presentations (TAROS 2023, KDIR 2023, EPIA 2024, IEEE ISCC 2025), 1 session-chair role (AgriFoRwArdS CDT Annual Conference 2024 + Programme Committee), and 1 outreach career talk (Ormiston Victory Academy), and verified the `_pages/talks.html` listing iterates `site.talks reversed`.

## Tasks Executed

| # | Task                                            | Commit  | Files                                                                                                                                                                       |
| - | ----------------------------------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1 | Conference talk entries for paper presentations | 9c29416 | _talks/2023-04-01-taros-precision-spraying.md, _talks/2023-11-01-kdir-interpretable-quantized.md, _talks/2024-09-01-epia-clock-drawing.md, _talks/2025-06-08-ieee-iscc-spirometry.md |
| 2 | Chaired sessions + career talk                  | ba2170f | _talks/2024-06-01-agriforwards-chair.md, _talks/2024-03-01-ormiston-victory-careers.md                                                                                      |
| 3 | Verify /talks/ listing reverse-chronological    | 42aa5f6 | _pages/talks.html                                                                                                                                                           |

## Verification

- `ls _talks/*.md | wc -l` = 6 (target: 4-6) — pass
- `ls _talks/2023-*.md | wc -l` = 2 (TAROS, KDIR) — pass
- `ls _talks/2024-*.md | wc -l` = 3 (EPIA, AgriFoRwArdS chair, Ormiston) — pass
- `ls _talks/2025-*.md | wc -l` = 1 (IEEE ISCC) — pass
- Every `_talks/*.md` contains `collection: talks` and `type:` — pass
- `grep -l "Session chair" _talks/*.md` returns AgriFoRwArdS chair entry — pass
- `grep -l "Ormiston Victory" _talks/*.md` returns career-talk entry — pass
- `grep -l "AgriFoRwArdS" _talks/*.md` returns chair entry — pass
- `_pages/talks.html` exists and contains `site.talks` — pass
- No demo phrases (`paper title`, `talk 1`, `tutorial 1`, `lorem`) — pass

## Requirements Satisfied

- **TALKS-01** — Talks page with per-entry fields (title, venue, date, location, type) populated
- **TALKS-02** — Conference talks linked to publication entries via `paper: /publication/<slug>`
- **TALKS-03** — Chaired-sessions surface present (AgriFoRwArdS CDT)

## Deviations from Plan

None — plan executed exactly as written. Placeholder filenames `2024-XX-*.md` in the plan were instantiated with month-precision dates (`2024-06-01`, `2024-03-01`) per the plan's "use approximate dates with month precision" instruction.

## Cross-Plan Dependencies

- **Plan 02-04 (publications)**: Each conference talk uses `paper: /publication/<slug>` linking. Publication slugs assumed:
  - `/publication/2023-taros-precision-spraying`
  - `/publication/2023-kdir-interpretable-quantized`
  - `/publication/2024-epia-clock-drawing`
  - `/publication/2025-ieee-iscc-spirometry`
  If Plan 02-04 lands with different slugs, update `paper:` keys in `_talks/*` accordingly. Verifier should cross-check.

## TODOs for Owner Sweep

Each is inline-flagged with `<!-- TODO(owner): ... -->`. Consolidated list:

1. **Exact dates** — confirm month/day for all 6 talks (currently month-precision placeholders).
2. **Locations** — fill `location:` (city, country) for TAROS 2023, KDIR 2023, EPIA 2024, IEEE ISCC 2025.
3. **Presenter confirmation** — for EPIA 2024 and IEEE ISCC 2025, confirm whether Harry personally presented (he is co-author, not first author).
4. **Slides / posters** — add URLs to slide decks or posters where available; currently absent.
5. **Additional chaired sessions** — plan flagged "multiple AgriFoRwArdS CDT conferences". Currently only 2024 is captured. Add entries for any other years Harry chaired.
6. **Rename filenames** — once exact dates known, rename `_talks/YYYY-MM-01-*.md` to `_talks/YYYY-MM-DD-*.md` (and update `permalink` only if slug changes).

## Self-Check: PASSED

- _talks/2023-04-01-taros-precision-spraying.md — FOUND
- _talks/2023-11-01-kdir-interpretable-quantized.md — FOUND
- _talks/2024-09-01-epia-clock-drawing.md — FOUND
- _talks/2025-06-08-ieee-iscc-spirometry.md — FOUND
- _talks/2024-06-01-agriforwards-chair.md — FOUND
- _talks/2024-03-01-ormiston-victory-careers.md — FOUND
- _pages/talks.html — FOUND (annotated)
- Commit 9c29416 — FOUND
- Commit ba2170f — FOUND
- Commit 42aa5f6 — FOUND
