# Phase 1: Bootstrap & Deploy — Context

**Gathered:** 2026-05-17
**Status:** Ready for planning
**Source:** /gsd-discuss-phase

<domain>
## Phase Boundary

Phase 1 materialises the academicpages template into this working tree, force-overwrites the live remote `Harry-Rogers/Harry-Rogers.github.io` (currently serving a stale 2020 hand-rolled site), and verifies that the canonical URL `https://harry-rogers.github.io/` serves a Jekyll-rendered site from the default GitHub Pages builder.

**In scope:**
- Copy academicpages template files into this working tree (preserving `.planning/`, `CLAUDE.md`, `assets/images/profile-original.jpg`)
- Configure `_config.yml` (`url`, `baseurl: ""`, `repository`, plugin allowlist)
- Pin `.ruby-version` and commit `Gemfile.lock`
- Ship academicpages' `.devcontainer/` as-is
- Force-push to remote `main`, set Pages source = `main` branch / root
- Verify all 5 Bootstrap Gates green
- Open a real Codespace and confirm `bundle exec jekyll serve` renders at the forwarded port (DEPLOY-05 UAT)

**Out of scope (belongs to later phases):**
- Demo content purge → Phase 2 (first task there)
- Headshot resize/compression → Phase 2 (point-of-use)
- Real bio / publications / talks / teaching / news → Phase 2
- Dark theme, Oxford-blue accents → Phase 3
- SEO meta tags, OG image, favicon, htmlproofer audit, Lighthouse → Phase 4

</domain>

<decisions>
## Implementation Decisions

### Bootstrap mechanism (template materialisation)

**Approach:** Copy template files in, single commit.

- Clone `academicpages/academicpages.github.io` to a temp dir (e.g. `/tmp/academicpages-src/`).
- Copy its tracked files into this working tree **excluding** anything that would clobber existing local artifacts: `.planning/`, `CLAUDE.md`, `assets/images/profile-original.jpg`, and the local `.git/`.
- Demo content (`_publications/`, `_talks/`, `_portfolio/`, `_teaching/`, `_posts/`) **is imported** so Phase 1's build-green criterion has real pages to render. Phase 2 purges it (already specified in ROADMAP as the first Phase 2 task — `grep -ri "lorem|sample text|cras mattis|paper title number|john doe|teaching experience [0-9]"` must return zero matches at the end of Phase 2's purge).
- Commit as a single `feat: import academicpages template` (or split logically if natural — atomic commits are fine).
- Do **not** add academicpages as a git remote. We don't track upstream.

**Why this over alternatives:** Avoids polluting our history with academicpages' commit log; avoids the noisy `--allow-unrelated-histories` merge commit; avoids deleting + recreating the remote repo on GitHub.

### Old remote history

**Approach:** Force-push, discard entirely. No tag, no `legacy-2020` branch.

Per PROJECT.md / DEPLOY-01: "Old commit history is discarded." Implementation: `git push --force origin main`. Owner has explicitly decided no salvage.

### Plugin allowlist (`_config.yml > plugins:`)

**Enable exactly these four** (CLAUDE.md tech stack wins over REQUIREMENTS.md DEPLOY-04 wording):

- `jekyll-feed` — RSS for News (justified by NEWS-01..04)
- `jekyll-sitemap` — SEO discoverability
- `jekyll-seo-tag` — OpenGraph / Twitter Card / canonical URLs (foundation Phase 4 builds on)
- `jekyll-redirect-from` — per-page aliases if pages are ever renamed

**Disable / do not enable:**
- `jekyll-gist` — not used
- `jekyll-paginate` — News is milestone-only, will stay under ~15 entries (REQUIREMENTS.md NEWS-02 says "most recent 5"); skip for now
- `jemoji` — optional, not needed

**Action:** REQUIREMENTS.md DEPLOY-04 lists `jekyll-gist` and `jemoji` and omits `jekyll-seo-tag` — this is stale wording from before the tech-stack research. **As part of Phase 1**, update DEPLOY-04 to match this decision so REQUIREMENTS.md stays the canonical spec going forward. Commit that doc update separately from code changes.

### Codespaces verification (DEPLOY-05)

**Approach:** Ship the academicpages `.devcontainer/` unmodified; manually verify in a real Codespace as a UAT task in Phase 1's plan.

- The plan must include a task: after force-push and Pages source set, open a Codespace from `github.com/Harry-Rogers/Harry-Rogers.github.io`, run `bundle exec jekyll serve` inside the devcontainer, confirm the forwarded port renders the site with no Liquid or Sass errors. Record pass/fail in PLAN as a UAT criterion (`autonomous: false` — owner must run manually).
- **Do not** customise `.devcontainer/` in Phase 1. If Phase 3 (dark theme iteration) needs livereload or other tweaks, those land there.
- Do not document a local Ruby 3.3.4 setup path in Phase 1. Owner can spin up a Codespace. If a local path is needed later, it goes in Phase 4 polish.

### Demo content handling

**Approach:** Import academicpages' demo content as-is during Phase 1. Phase 2's first task purges it. Boundary is clean: Phase 1 proves the build works against real (if placeholder) pages; Phase 2 owns content authenticity.

### Headshot handling

**Approach:** Leave `assets/images/profile-original.jpg` (6.3MB) untouched in Phase 1. Phase 2 will produce web-sized variants (e.g. `profile-400.jpg`, `profile-800.jpg`) at point of use on the landing page. The 6.3MB blob does not affect build greenness or DEPLOY-01..05 gates.

### Claude's Discretion

- Exact path / commands for the `cp -a` (or `rsync --exclude`) operation during template import
- Whether to split the import into multiple atomic commits (e.g. `feat: import academicpages collections`, `feat: import academicpages layouts/includes`) vs a single commit — judgment call, both fine
- Specific contents of `Gemfile` (use `github-pages` meta-gem as locked by CLAUDE.md)
- Specific `_config.yml` keys beyond `url`/`baseurl`/`repository`/`plugins` — keep academicpages defaults unless they break a gate

</decisions>

<canonical_refs>
## Canonical References

**Downstream agents MUST read these before planning or implementing.**

### Project-level decisions and scope
- `.planning/PROJECT.md` — Project charter, constraints, core value, owner profile, brownfield note
- `.planning/REQUIREMENTS.md` — All 44 v1 requirements; DEPLOY-01..05 are this phase's scope (DEPLOY-04 wording will be updated this phase)
- `.planning/ROADMAP.md` — Phase 1 goal, 5 Bootstrap Gates, success criteria, brownfield note
- `CLAUDE.md` — Project instructions including full Technology Stack research (Jekyll 3.10.0, Ruby 3.3.4, pages-gem, plugin allowlist rationale, what NOT to use)

### Project research (informs but does not override CLAUDE.md or PROJECT.md)
- `.planning/research/SUMMARY.md` — Synthesised research overview
- `.planning/research/STACK.md` — Stack rationale
- `.planning/research/ARCHITECTURE.md` — Site architecture
- `.planning/research/FEATURES.md` — Feature inventory
- `.planning/research/PITFALLS.md` — Known traps (e.g. `jekyll-scholar` not allowlisted, `remote_theme` breaks vendored MM)

### External (read once during planning, cite locally — do not re-fetch repeatedly)
- `https://pages.github.com/versions/` — Authoritative GitHub Pages dependency manifest (Jekyll, Ruby, plugin versions)
- `https://github.com/academicpages/academicpages.github.io` — Upstream template (Gemfile, `.devcontainer/`, `_config.yml` defaults)

</canonical_refs>

<specifics>
## Specific Ideas

- The local working tree currently contains ONLY: `assets/images/profile-original.jpg`, `CLAUDE.md`, `.planning/**`, `.git/**`. Five commits exist on local `main`; no remote is configured locally yet (`git remote -v` is empty).
- The remote `Harry-Rogers/Harry-Rogers.github.io` exists and serves a stale 2020 hand-rolled HTML/CSS site. Must be added as `origin` and force-pushed.
- `_config.yml` MUST end up with exactly:
  - `url: "https://harry-rogers.github.io"`
  - `baseurl: ""` (empty string — not `"/"`, not `"/academicpages"`)
  - `repository: "Harry-Rogers/Harry-Rogers.github.io"`
- `.ruby-version` MUST contain exactly the single line `3.3.4`.
- `Gemfile` MUST use the `github-pages` meta-gem (not a hand-pinned `jekyll` version).
- `Gemfile.lock` MUST be committed (locked decision from CLAUDE.md tech stack — local dev parity matters even though Pages ignores it).
- DEPLOY-05 verification needs the owner to manually open a Codespace — this is the only `autonomous: false` task in the phase.

</specifics>

<deferred>
## Deferred Ideas

- Local Ruby setup docs (rbenv/asdf path) — deferred to Phase 4 polish or skipped entirely; Codespaces is the supported dev path
- Customising `.devcontainer/` (livereload, htmlproofer pre-installed) — deferred to Phase 3 if iteration speed becomes an issue
- Headshot resize/compression — Phase 2
- Demo content removal — Phase 2 (first task per ROADMAP)
- Splitting old 2020 history off into a `legacy-2020` tag — rejected; owner chose clean force-push
- Adding `jekyll-paginate`, `jemoji`, `jekyll-gist` to plugins — not needed in v1, none currently planned for v2 either

</deferred>

---

*Phase: 01-bootstrap-deploy*
*Context gathered: 2026-05-17 via /gsd-discuss-phase*
