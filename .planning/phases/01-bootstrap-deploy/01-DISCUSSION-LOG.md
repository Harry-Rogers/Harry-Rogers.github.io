# Phase 1: Bootstrap & Deploy — Discussion Log

**Date:** 2026-05-17
**Phase:** 01-bootstrap-deploy
**Mode:** default (interactive)

This log is for human reference (audits, retrospectives). Downstream agents read `01-CONTEXT.md`, not this file.

---

## Pre-analysis: what was already locked

CLAUDE.md + PROJECT.md + ROADMAP.md locked the bulk of Phase 1 before discussion:
- Stack (Jekyll 3.10.0, Ruby 3.3.4, pages-gem, vendored MM, no remote_theme)
- Force-overwrite the live remote; discard old 2020 history
- `_config.yml`: `baseurl: ""`, `url: "https://harry-rogers.github.io"`
- Use academicpages' shipped `.devcontainer/` as-is
- 5 Bootstrap Gates as exit criteria
- What NOT to use list (no jekyll-scholar, no remote_theme, no custom Actions)

Gray areas that remained were all about *bootstrap mechanics* and one config conflict to reconcile.

---

## Q1 — Bootstrap method (template materialisation)

**Options presented:**
1. Add upstream as remote, `git merge --allow-unrelated-histories`
2. Copy template files in, single commit *(recommended)*
3. Fork on GitHub, then rebase local onto fork

**User selected:** Option 2 — Copy template files in, single commit.

**Rationale:** Cleanest history. Avoids pulling academicpages' commit log; avoids the noisy merge commit; avoids the disruption of deleting and recreating the remote repo.

---

## Q2 — Old remote history

**Options presented:**
1. Force-push, discard entirely *(recommended)*
2. Tag old `main` as `legacy-2020` before force-push
3. Push old `main` to a `legacy-2020` branch before force-push

**User selected:** Option 1 — Force-push, discard entirely.

**Rationale:** Matches PROJECT.md / DEPLOY-01 verbatim ("Old commit history is discarded"). Owner has previously decided no salvage.

---

## Q3 — Plugin allowlist conflict (REQUIREMENTS.md vs CLAUDE.md)

**Context:** REQUIREMENTS.md DEPLOY-04 lists `jekyll-gist` and `jemoji` enabled and omits `jekyll-seo-tag`. CLAUDE.md tech stack research says `jekyll-gist: NO`, `jekyll-seo-tag: YES`, `jemoji: optional`.

**Options presented:**
1. Follow CLAUDE.md tech stack *(recommended)*
2. Follow REQUIREMENTS.md DEPLOY-04 verbatim
3. Union — enable everything from both

**User selected:** Option 1 — Follow CLAUDE.md tech stack.

**Implication:** REQUIREMENTS.md DEPLOY-04 wording is now stale and must be updated to match during Phase 1 (separate commit). Final v1 plugin list: `jekyll-feed`, `jekyll-sitemap`, `jekyll-seo-tag`, `jekyll-redirect-from`.

---

## Q4 — Codespaces verification (DEPLOY-05)

**Options presented:**
1. Ship devcontainer + manually verify in real Codespace *(recommended)*
2. Ship devcontainer, defer verification to Phase 3
3. Also pin local Ruby 3.3.4 setup with README instructions

**User selected:** Option 1.

**Implication:** Phase 1 plan must include an `autonomous: false` UAT task for the owner to run manually after force-push.

---

## Q5 — Demo content

**Options presented:**
1. Import demo content, delete in Phase 2 *(recommended)*
2. Strip demo content during Phase 1 import

**User selected:** Option 1.

**Rationale:** Phase 1's success criterion is build-green; demo content makes that meaningful. Phase 2's first task is already the demo-content purge per ROADMAP.

---

## Q6 — Headshot

**Options presented:**
1. Leave as-is, resize in Phase 2 *(recommended)*
2. Resize now to a web-sized variant

**User selected:** Option 1.

**Rationale:** 6.3MB blob doesn't affect build greenness. Phase 2 owns photo usage on the landing page; resize there.

---

## Deferred ideas captured

- Local Ruby rbenv/asdf setup docs — Phase 4 or skip
- `.devcontainer/` customisation (livereload, htmlproofer) — Phase 3 if needed
- `legacy-2020` tag/branch — explicitly rejected
- `jekyll-paginate`, `jemoji`, `jekyll-gist` — none planned for v1 or v2

## Scope creep redirects

- None encountered. All discussion stayed within Phase 1's stated boundary.

---

*End of discussion log.*
