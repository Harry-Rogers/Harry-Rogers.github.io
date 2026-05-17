---
gsd_state_version: 1.0
milestone: v1.0
milestone_name: milestone
status: Awaiting `/gsd-plan-phase 1`
last_updated: "2026-05-17T16:10:24.773Z"
progress:
  total_phases: 4
  completed_phases: 0
  total_plans: 0
  completed_plans: 0
  percent: 0
---

# State — Harry Rogers Academic Website

## Project Reference

**Project:** Harry Rogers — Academic Website (`harry-rogers.github.io`)
**Core value:** When someone Googles "Harry Rogers Oxford" and lands on this site, within ten seconds they can tell who he is, what he works on, and how to reach him — and within thirty seconds they can find his papers, talks, and CV.
**Current focus:** Initialization complete — ready to begin Phase 1 (Bootstrap & Deploy).

## Current Position

- **Milestone:** v1
- **Phase:** Phase 1 — Bootstrap & Deploy (not started)
- **Plan:** none yet
- **Status:** Awaiting `/gsd-plan-phase 1`
- **Progress:** `[░░░░░░░░░░] 0/4 phases complete`

## Performance Metrics

| Metric | Value |
|---|---|
| v1 requirements | 44 |
| Phases | 4 |
| Requirements mapped | 44/44 (100%) |
| Phases complete | 0/4 |

## Accumulated Context

### Decisions

(Inherited from PROJECT.md — see "Key Decisions" there for the authoritative list.)

Roadmap-level decisions made during this initialization:

- **4-phase coarse roadmap.** Granularity is `coarse` per config; research synthesizer proposed 6 phases (Bootstrap, Config, Content, News, Theme, Polish) but Config folds into Bootstrap (both are `_config.yml`-centric setup) and News folds into Content (both are content authoring). Result: Bootstrap & Deploy → Content Population → Dark Theme & Customisation → Polish & Ship.
- **Horizontal-layer build order, not vertical slices.** This is a static site rebuild; you cannot meaningfully test a dark palette against placeholder text, and you cannot SEO-audit a site whose URL canonicalisation is broken. Each layer is a precondition for the next.
- **Bootstrap gates (G1–G5) are exit-blocking for Phase 1.** Canonical URL at root, `baseurl: ""`, `main` branch as build source, allowlist-only plugins, `Gemfile.lock` committed.
- **Demo-content purge is the FIRST task of Phase 2, not the last.** `grep -ri "lorem\|sample text\|cras mattis"` must return zero results before any real content is authored — prevents accidental mixing of real and placeholder text.
- **News stored as `_data/news.yml`, not `_news/` collection and not `_posts/`.** Structurally guarantees "no blog" — no place to write a paragraph means no temptation to blog. (This narrows the research synthesizer's `_news/` collection recommendation in favour of the PROJECT.md decision.)
- **Codespaces is a Phase 1 deliverable so Phase 3 can rely on it.** DEPLOY-05 requires the academicpages `.devcontainer/` works end-to-end before Phase 3's Sass iteration loop needs it.

### Open Questions

1. **Local dev preference:** Codespaces vs local Ruby? Phase 1 must verify Codespaces works (DEPLOY-05); Phase 3 strongly recommends it. Confirm before Phase 3.
2. **Publications metadata completeness:** PROJECT.md lists 8 papers "to verify against Scholar at content phase." DOIs, PDFs, and full author lists need confirmation during Phase 2.
3. **Talk recordings:** Unknown whether any conference talks were recorded. Determine during Phase 2 content audit; if recordings exist, can land in v1 — if not, defer to v1.x.

### Active TODOs

- [ ] Run `/gsd-plan-phase 1` to decompose Phase 1 into executable plans.

### Blockers

None.

## Session Continuity

**Last action:** Roadmap created (4 phases, 44/44 requirements mapped, 100% coverage).
**Next action:** `/gsd-plan-phase 1` — decompose Phase 1: Bootstrap & Deploy into executable plans.
**Resumable from:** any new session — read `.planning/ROADMAP.md` + this file + `.planning/PROJECT.md` to reconstitute full state.

---
*Last updated: 2026-05-17 after roadmap creation*
