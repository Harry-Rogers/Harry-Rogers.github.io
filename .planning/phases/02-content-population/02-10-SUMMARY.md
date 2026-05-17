---
phase: 02-content-population
plan: 10
subsystem: site-config
tags: [contact, author-profile, audit, config]
requires:
  - 02-03 (author block scaffolded with name/bio/location/employer/uri/email/avatar)
  - 02-09 (nav/header pass — established author sidebar surface)
provides:
  - "author.orcid, author.googlescholar, author.github, author.linkedin, author.uri, author.email populated and verified in _config.yml"
  - "CONTACT-03 audit signed off — no iframes/widgets introduced by Phase 2 work"
affects:
  - _includes/author-profile.html (consumer — renders FontAwesome icon links from author.* keys)
tech-stack:
  added: []
  patterns:
    - "academicpages built-in sidebar contact-link rendering (no custom HTML)"
    - "static icon links over embedded widgets (CLAUDE.md: Scholar blocks iframes, ORCID widget breaks mobile)"
key-files:
  created:
    - .planning/phases/02-content-population/02-10-SUMMARY.md
  modified: []
decisions:
  - "All 6 author contact fields were already populated by prior plans (02-03 set uri/email/avatar/bio/location/employer; the ORCID/Scholar/GitHub/LinkedIn values landed in the same wave). 02-10 is a verification-only plan — no edits needed."
  - "Iframe audit surfaced 5 vendored hits, all documented below; zero matches in Phase-2-authored content."
metrics:
  duration: "~1 minute"
  completed: 2026-05-17
tasks_completed: 2
tasks_total: 2
---

# Phase 02 Plan 10: Contact Sidebar Links + Iframe Audit Summary

Verified the author profile sidebar's FontAwesome icon-link configuration (ORCID, Google Scholar, GitHub, LinkedIn, Oxford department page, email) in `_config.yml`; ran the CONTACT-03 iframe/widget audit and confirmed no embeds were introduced in Phase-2-authored files.

## What Was Done

### Task 1 — Verify author contact links

All six `author.*` keys required by CONTACT-02 were already set in `_config.yml` by upstream plans (02-03 and the same-wave authoring work that filled in the academic-profile fields). No edits required.

| Field | Value | Verified |
|---|---|---|
| `author.email` | `Harry.Rogers@eng.ox.ac.uk` | yes |
| `author.orcid` | `https://orcid.org/0000-0003-3227-5677` | yes |
| `author.googlescholar` | `https://scholar.google.com/citations?user=sPwcwvMAAAAJ` | yes |
| `author.github` | `Harry-Rogers` | yes |
| `author.linkedin` | `harry-rogers-ox` | yes |
| `author.uri` | `https://eng.ox.ac.uk/people/harry-rogers` | yes |

All `grep -q` acceptance checks pass. No `johndoe`/`john doe`/`john-doe` demo strings remain.

### Task 2 — CONTACT-03 iframe / widget audit

Ran the plan's audit command:

```bash
grep -riE "<iframe|orcid.org/.*widget|scholar.*badge|impactstory" . \
  --exclude-dir=.git --exclude-dir=.planning --exclude-dir=_site \
  --exclude-dir=node_modules --exclude-dir=_sass \
  --exclude=README.md --exclude=CLAUDE.md
```

The raw command surfaced 5 hits. **All 5 are vendored academicpages / academicons assets, none in Phase-2-authored files.** Refined audit excluding the vendored surfaces (`_includes/`, `assets/`, the empty placeholder key in `_config.yml`, and the disabled `talkmap.html` page) returns zero matches.

### Vendored matches (documented as false-positives, NOT touched per Phase 2 scope)

| File | Match | Why it's a false-positive |
|---|---|---|
| `_includes/author-profile.html` | `{% if author.impactstory %}` and the `ai-impactstory` icon `<li>` | Vendored academicpages sidebar scaffolding. The block is gated on `author.impactstory` being truthy; our `_config.yml` leaves it blank (`impactstory : # URL`), so it never renders. |
| `assets/css/academicons.css` | `.ai-impactstory:before`, `.ai-impactstory-square:before` | Vendored Academicons CSS — class definitions for an icon font. Inert unless an HTML element uses the class. No element does. |
| `assets/css/academicons.min.css` | minified Academicons stylesheet | Same as above. |
| `assets/fonts/academicons.svg` | `glyph-name="impactstory"`, `"impactstory-square"` | Vendored font glyph definitions. Not HTML; cannot render an iframe. |
| `_config.yml` | `impactstory      : # URL` (commented placeholder) | Vendored academicpages config scaffold. Blank value = sidebar renderer skips. Removing it would diverge from the upstream config layout for no functional gain. |
| `_pages/talkmap.html` | `<iframe src="/talkmap/map.html" ...>` | Vendored academicpages page for the optional "talk map" feature. Reachable only when `talkmap_link: true` in `_config.yml`. Our config sets `talkmap_link: false` (line 99), so the page is not linked from nav. The file ships with the template; deleting it would touch vendored scaffolding (out of Phase 2 scope per the plan's "do NOT touch _sass/ or theme files" guardrail). The iframe will not render on any visited page. |

**Conclusion:** No Phase 2 plan (02-01 through 02-10) introduced any iframe, citation badge, ORCID widget, or Impactstory embed. CONTACT-03 is satisfied.

## Deviations from Plan

None — plan executed exactly as written. Task 1 was a no-op verification (data already present from upstream plans); Task 2 audit surfaced only vendored false-positives, which the plan explicitly anticipates ("If any surface in `_includes/` or `_layouts/` originate from the vendored academicpages template ... document them in 02-10-SUMMARY.md but do NOT delete").

## Quality Gates

- [x] All 5 contact link fields populated in `_config.yml > author:` (orcid, googlescholar, github, linkedin, uri)
- [x] `grep -c "Harry.Rogers@eng.ox.ac.uk" _config.yml` >= 1
- [x] Iframe audit returns 0 non-vendored matches
- [x] Task 1 automated verify passed (single-line `&&`-chained grep)
- [x] All vendored matches documented above

## Known Stubs

None.

## Threat Flags

None — this plan does not change any trust boundary, network surface, or auth path.

## Requirements Closed

- CONTACT-02 — Author sidebar contact links populated (ORCID, Scholar, GitHub, LinkedIn, Oxford dept page, email)
- CONTACT-03 — No iframes / embedded citation widgets anywhere in Phase-2-authored content

## Self-Check: PASSED

- `_config.yml` author block — FOUND, all 6 fields populated
- `.planning/phases/02-content-population/02-10-SUMMARY.md` — FOUND (this file)
- No new commits to verify pre-final-commit (verification-only plan); final docs commit will be created next.
