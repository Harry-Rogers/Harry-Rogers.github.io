---
phase: 02-content-population
plan: 06
subsystem: content/teaching
tags: [teaching, outreach, content]
requires: [02-01]
provides:
  - _teaching/*.md (5 real entries)
  - /teaching/ page populated
affects:
  - _pages/teaching.html (verified annotation only)
tech-stack:
  added: []
  patterns:
    - Academicpages `_teaching/` collection with `collection: teaching`, `type:`, `permalink:`, `venue:`, `date:`, `location:` front matter
key-files:
  created:
    - _teaching/2022-lego-mindstorms-uea.md
    - _teaching/2023-lego-mindstorms-outreach.md
    - _teaching/2023-sensor-cdt-mentoring.md
    - _teaching/2024-agriforwards-committee.md
    - _teaching/2024-drink-outside-the-box.md
  modified:
    - _pages/teaching.html
decisions:
  - "Approximate dates used (YYYY-01-01 / YYYY-06-01) with body-level TODO(owner) markers where exact dates are unknown — keeps entries shippable without fabricating facts"
  - "Cross-linked related outreach entries (Mindstorms UEA ↔ Bringing Scientists to You, CDT committee ↔ Team Challenge mentoring ↔ Drink Outside the Box) to surface a coherent outreach narrative"
metrics:
  completed: 2026-05-17
  tasks: 2
---

# Phase 02 Plan 06: Teaching and Outreach Content Summary

Authored five real teaching/outreach entries covering all TEACH-02 required items — Lego Mindstorms workshops at UEA and through *Bringing Scientists to You* (Yarmouth College, Thetford Academy), Sensor CDT Team Challenge mentoring, AgriFoRwArdS CDT organisation committees (Annual Conference 2024 Programme Committee, Summer School 2024), and Drink Outside the Box public engagement — and verified `_pages/teaching.html` still iterates `site.teaching`.

## What Was Built

- **`_teaching/2022-lego-mindstorms-uea.md`** — Workshop, UEA School of Computing Sciences. The original campus-based Lego Mindstorms outreach session.
- **`_teaching/2023-lego-mindstorms-outreach.md`** — Outreach, Yarmouth College and Thetford Academy via UEA's *Bringing Scientists to You* widening-participation programme.
- **`_teaching/2023-sensor-cdt-mentoring.md`** — Mentoring, UEA Sensor CDT annual Team Challenge.
- **`_teaching/2024-agriforwards-committee.md`** — Committee. Covers both AgriFoRwArdS Annual Conference 2024 Programme Committee and Summer School 2024 co-organiser roles in a single page (committee work, one entry).
- **`_teaching/2024-drink-outside-the-box.md`** — Public engagement, agricultural robotics talk in an informal venue.
- **`_pages/teaching.html`** — Added `<!-- Verified per TEACH-01. -->` annotation; layout untouched (still iterates `site.teaching` via the academicpages default).

Each entry has 100–300 words of substantive body content drawn from PROJECT.md owner-profile data; uncertain dates carry `<!-- TODO(owner): confirm ... -->` markers rather than fabrications.

## Verification

| Check | Result |
|---|---|
| `ls _teaching/*.md \| wc -l` ≥ 5 | 5 (PASS) |
| `grep -l "Lego Mindstorms" _teaching/*.md` ≥ 2 | 2 (PASS) |
| `grep -l "Sensor CDT" _teaching/*.md` ≥ 1 | 1 (PASS) |
| `grep -l "AgriFoRwArdS" _teaching/*.md` ≥ 1 | 1 (PASS) |
| `grep -l "Drink Outside the Box" _teaching/*.md` ≥ 1 | 1 (PASS) |
| All files have `collection: teaching` | PASS |
| No demo phrases (`teaching experience 1`, `lorem`) | PASS |
| `_pages/teaching.html` iterates `site.teaching` | PASS |

## Deviations from Plan

None — plan executed exactly as written. All five entries created with the prescribed slugs, types, and venues. No deviation rules triggered.

## Requirements Closed

- **TEACH-01** — `/teaching/` page now backed by real entries.
- **TEACH-02** — All required outreach items (Lego Mindstorms UEA + Yarmouth + Thetford, Sensor CDT mentoring, AgriFoRwArdS committees, Drink Outside the Box, Summer School 2024, Annual Conference 2024 Programme Committee) represented across the five files.

## Known Stubs

Date-precision TODOs are intentional and tagged for the owner to confirm:

| File | Stub | Reason |
|---|---|---|
| `2022-lego-mindstorms-uea.md` | `<!-- TODO(owner): confirm exact dates and frequency -->` | PROJECT.md does not specify session dates |
| `2023-lego-mindstorms-outreach.md` | `<!-- TODO(owner): confirm exact dates of Yarmouth College and Thetford Academy visits -->` | PROJECT.md lists the schools but not visit dates |
| `2023-sensor-cdt-mentoring.md` | `<!-- TODO(owner): confirm year(s) and number of teams -->` | PROJECT.md mentions the role but not cohort years |
| `2024-agriforwards-committee.md` | `<!-- TODO(owner): confirm exact dates and location -->` | Conference and summer-school dates not in PROJECT.md |
| `2024-drink-outside-the-box.md` | `<!-- TODO(owner): confirm exact date and venue -->` | PROJECT.md notes the event but not when/where it was held |

These are content-precision stubs, not functional stubs — pages render and read correctly with the approximate dates and the TODO markers are HTML comments (invisible to readers but flagged for the owner in source).

## Commits

| Hash | Message |
|---|---|
| `08641f6` | feat(02-06): add teaching and outreach entries |

## Self-Check: PASSED

- Files exist: all 5 `_teaching/*.md` and `_pages/teaching.html` modification present.
- Commit `08641f6` present on `master` (`git log --oneline` confirmed).
- All acceptance criteria and quality gates pass.
